---
description: >-
  This page aims to provide all necessary resources to get started with making
  mods using scripts and prefabs.
---

# Creating Mods via Scripts & Prefabs

## Step-by-step guide

{% stepper %}
{% step %}
### Creating a mod base

Head over to the PugMod window in Unity and select `Open Mod SDK Window`.&#x20;

<figure><img src="../../.gitbook/assets/image (71).png" alt=""><figcaption></figcaption></figure>

Then head over to the Mod Settings tab and select `New Mod`. Afterwards name your mod and press `Create`.&#x20;

<figure><img src="../../.gitbook/assets/image (72).png" alt=""><figcaption></figcaption></figure>

This will generate a mod folder including a scriptable object containing the mods' build settings under the `Assets\<YourModNameFolder>` path.&#x20;

A script assembly will also be generated which'll reference all of the games' assemblies that you've fetched by updating game files, this is nothing very useful for now but good to know in the future in case you run into outdated assembly issues.
{% endstep %}

{% step %}
### Creating a script&#x20;

To create a script for your mod, head over to `Assets\<YourModNameFolder>` and inside of it right click, then select `Create > MonoBehaviour Script`.&#x20;

<figure><img src="../../.gitbook/assets/image (73).png" alt=""><figcaption></figcaption></figure>

This will cause Unity to recompile, which is expected and can take a few minutes.&#x20;

Once you've made a script and it has finished loading you can double click the script in Unity and that'll open it using your default text editor.

Inside the script you will be expected to inherit and implement the IMod interface, you don't have to do this for every script in your mods' folder, just the one that you want to do something special whenever your mod is loaded/updated/etc.&#x20;

Here is an example of implementing the IMod interface:

```csharp
using PugMod;
using UnityEngine;

public class EnableConsole : IMod
{
public void EarlyInit()
{
// This enables Console Commands.
Manager.enableConsole = true; // Make sure to run this in EarlyInit()!
}
// ...Init, ModObjectLoaded(...), Shutdown(), Update()...
}
```
{% endstep %}

{% step %}
### Using the API&#x20;

The IMod interface is the basic interface used to initialize your scripts.&#x20;

From there you can access most of the game files.&#x20;

The functions provided under the PugMod.API are the most stable currently so try to use them whenever possible.&#x20;

Here are a couple of common API calls that you can use:

* API.Server.DropObject can be used to spawn anything that has an ObjectID.
* API.Effects.PlayPuff\` can be used to play VFX.
* API.Audio.PlaySfx can be used to play SFX.
* API.Server.World can be used to interact with the server.

Here is an example of using some of these API calls:

```csharp
using HarmonyLib;
using Unity.Mathematics;
using UnityEngine;
using PugMod;
using System;
using PlayerEquipment;
using Unity.Entities;

public class SpawnStuffFromTiles : IMod
{
   /* 
    * We have these flags and the entire script split into an IMod implementation and a Harmony patch because the API calls we use must run on Unitys' main thread.
    * If we tried to run them in ECS which is where ShovelSlot.PlayDigEffects() runs, it would break.
    * So the Harmony patch just detects when ShovelSlot.PlayDigEffects() has finished and sets a flag to true, 
    * and then the Update() method which runs every frame on the main thread checks that flag and runs our logic.
    */
    public static bool spawnObjectAndFX = false;
    public static float3 diggingPosition;
    private static Unity.Mathematics.Random random;

    public void EarlyInit()
    {
    }

    public void Init()
    {
        BurstDisabler.DisableBurstForSystem<EquipmentUpdateSystem>();
    }

    public void ModObjectLoaded(UnityEngine.Object obj)
    {
    }

    public void Shutdown()
    {
    }

    public void Update()
    {
        var player = Manager.main.player;

        if (player == null)
        {
            return;
        }

        // Check if conditions have been met for us to run our function.
        if (spawnObjectAndFX)
        {
            // Reset the flag so that once it gets set to true it doesn't keep spawning FX and Objects forever. A good thing to know is that Update in Unity runs every frame.
            spawnObjectAndFX = false;
            SpawnObjectAndDoFX();
        }
    }

    private void SpawnObjectAndDoFX()
    {
        // Get the local players' instance.
        var player = Manager.main.player;

        if (player == null)
        {
            return;
        }

        // Random number generating utility that is thread-safe, use this over Random.Range when working in ECS.
        // CreateFromIndex is needed when creating it from sequential values like index or time/ticks.
        random = Unity.Mathematics.Random.CreateFromIndex((uint)DateTime.Now.Ticks);

        var playerPosition = player.transform.position;
        Vector3 FXPosition = new(playerPosition.x, 0.5f, playerPosition.z);

        var objectSpawnPosition = diggingPosition + new float3(0, 0.5f, 0);

        // Play VFX at local players' position.
        API.Effects.PlayPuff((int)PuffID.AncientEnergyBurst, FXPosition, 50);

        // Play a sound effect.
        API.Audio.PlaySfx(SfxTableID.acidLarvaDeath, FXPosition, pitchMultiplier: 1f, volumeMultiplier: 2f);

        // Get a random Object ID between 1001 and 1011, on Core Keepers' end these are all the bars from bronze to relucite.
        int randomObjectID = random.NextInt(1001, 1011);

        // Drop the object.
        if (API.Server.World != null)
        {
            API.Server.DropObject(randomObjectID, 0, 1, objectSpawnPosition);
        }
    }
}

[HarmonyPatch(typeof(ShovelSlot), "PlayDigEffects")]
public class SpawnObjectAndPlayFXAfterShoveling
{
    private static Unity.Mathematics.Random random;

    // Runs after ShovelSlot.PlayDigEffects() has already finished running.
    [HarmonyPostfix]
    public static void PostPlayDigEffects(float3 position, EquipmentUpdateAspect equipmentUpdateAspect)
    {
        // Get player instance
        var player = Manager.main.player;

        // Check if our player is the one that has eaten something and should be teleported.
        if (equipmentUpdateAspect.entity != player.entity)
        {
            // Return early if it's not our player so that we don't get teleported whenever other party members eat.
            return;
        }

        // Random number generating utility that is thread-safe, use this over Random.Range when working in ECS.
        random = new Unity.Mathematics.Random((uint)DateTime.Now.Ticks);

        // Roll 1-100 and if we roll above 50 then the Object gets spawned and SFX+VFX play.
        if (random.NextInt(0, 100) < 50)
        {
            // Tile that's causing the effect to occur.
            SpawnStuffFromTiles.diggingPosition = position;

            // Tells Update that we're now meeting all the conditions to spawn the Object and play the SFX+VFX.
            SpawnStuffFromTiles.spawnObjectAndFX = true;
        }
    }
}

/*
 * This is a workaround for the job running with burst since it starts after
 * OnUpdate. This slows down the game since we wait for all jobs to finish on
 * the main thread.
 */
[HarmonyPatch(typeof(EquipmentUpdateSystem), "OnUpdate")]
public static class ForceJobCompletePatch
{
    [HarmonyPostfix]
    [HarmonyPriority(Priority.High)] // Not needed for ISystem, but for SystemBase we want to make sure this runs before burst is enabled again
    public static void Postfix(ref SystemState state)
    {
        state.Dependency.Complete();
    }
}

```
{% endstep %}

{% step %}
### Scripting Restrictions

The only limit to what you can access are things like I/O and network operations that might be damaging to the host computer. Otherwise the only limitation is that the more “internal” functions that are accessed, the more likely this will break in future updates.

The main restricted parts are:

* System.IO
* System.Diagnostics
* System.Net
* System.Runtime.InteropServices
* System.Reflection
* System.AppDomain
* Application.Quit
* Accessing native assemblies

You can overcome these limitations by finding the Mod Builder Settings Scriptable Object instance in your Assets folder, it will be named after your mods' folder. Once you've located it, check **Skip Safety Checks** and **Accesses Extra Assemblies**. The downside of this is that your built mod will be marked with a tag informing mod users that these assemblies have skipped safety checks and may access other assemblies.

<figure><img src="../../.gitbook/assets/image (9) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Creating a prefab (game object)

A prefab in Unity is a game object which holds data, for example the rotation of an item and such.&#x20;

To create a prefab for your mod, head over to `Assets/<YourModNameFolder>` and inside of it right click, then select `Create > Scene > Prefab`.&#x20;

<figure><img src="../../.gitbook/assets/image (74).png" alt=""><figcaption></figcaption></figure>

The most common purpose if a prefab is to implement components that will determine how an item for example behaves in-game.&#x20;

Think of it as a data container. There are different types of authoring components that you can add to your prefab, but here are the most common ones that you'd want to add to for example make a mod which adds a new Sword.
{% endstep %}

{% step %}
### Authoring Components and adding them to prefabs

**Object Authoring** - this component determines the Object ID of your item, whatever you set as the items' Object name will become its' Object ID. You can also select the type of Object Type you want it to be which might change its' behavior. Rarity can be set via the rarity drop-down.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**Inventory Item Authoring** - this component will determine if your item is stackable in the players' inventory, what the sell value or buy value should be, the items' lootsprite, crafting settings, and more.

<figure><img src="../../.gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

**Durability Authoring** - you can use this component to determine how high or low the durability/reinforce cost of your item will be, this is mostly relevant if the item that you're making is a weapon or some sort of equipment.

<figure><img src="../../.gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

**Cooldown Authoring** - determines the cooldown of the item when used in Handheld item slots (1-9 keys).

<figure><img src="../../.gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

**Gives Conditions When Equipped Authoring** - this component determines what "stats" your item should have, as an example +5% crit chance, +40% melee attack speed, +9001% damage against bosses.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

**Weapon Damage Authoring** - here you can adjust the damage scaling for the item by modifying the damage multiplier, this is only relevant for weapons. Make sure that if you select magic/ranged that your object Type in Object Authoring also matches by using "Range Weapon" which counts for both Ranged and Magic weapons.&#x20;

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

**Localization Authoring** - This will be the key to translate your item, which you'll need to match in your localization file. Core Keeper localization uses TextDataBlocks. Language Genders can be left empty.

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

**Secondary Use Authoring** - Mostly relevant for tools and weapons, this can enable a weapon such as a sword to have a secondary on-use ability which'll by default be right click.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}
