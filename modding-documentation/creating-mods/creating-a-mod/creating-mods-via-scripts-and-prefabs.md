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

Head over to the PugMod window in Unity and select `Open Mod SDK Window`. Then head over to the Mod Settings tab and select `New Mod`. Afterwards name your mod and press `Create`. This will generate a mod folder including a scriptable object containing the mods' build settings under the `Assets\<YourModNameFolder>` path. An assembly will also be generated which'll reference all of the games' assemblies that you've fetched by updating game files, this is nothing very useful for now but good to know in the future in case you run into outdated assembly issues.
{% endstep %}

{% step %}
### Creating a script&#x20;

To create a script for your mod, head over to `Assets\<YourModNameFolder>` and inside of it right click, then select `Create > C# Script`. This will cause Unity to recompile and is expected. Once you've made a script you can double click it in Unity and that'll open it using your default text editor. Inside the script you will be expected to inherit and implement the IMod interface, you don't have to do this for every script in your mods' folder, just the one that you want to do something special whenever your mod is loaded/updated/etc.&#x20;
{% endstep %}

{% step %}
### Using the API&#x20;

The IMod interface is the basic interface used to initialize your scripts. From there you can access most of the game files. The functions provided under the PugMod.API are the most stable currently so try to use them whenever possible.&#x20;

Here are a couple of common API calls that you can use:

* API.Server.DropObject can be used to spawn anything that has an ObjectID.
* API.Effects.PlayPuff\` can be used to play VFX.
* API.Audio.PlaySfx can be used to play SFX.
* API.Server.World can be used to interact with the server.
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
### Creating a prefab

To create a prefab for your mod, head over to `Assets/<YourModNameFolder>` and inside of it right click, then select `Create > Prefab`. The most common purpose if a prefab is to implement components that will determine how an item for example behaves in-game. Think of it as a data container. There are different types of authoring components that you can add to your prefab, but here are the most common ones that you'd want to add to for example make a mod which adds a new Sword.
{% endstep %}

{% step %}
### Authoring Components and adding them to prefabs

**Object Authoring** - this component determines the Object ID of your item, whatever you set as the items' Object name will become its' Object ID. You can also select the type of Object Type you want it to be which might change its' behavior. Rarity can be set via the rarity drop-down.

<figure><img src="../../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

**Inventory Item Authoring** - this component will determine if your item is stackable in the players' inventory, what the sell value or buy value should be, the items' lootsprite, crafting settings, and more.

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

**Durability Authoring** - you can use this component to determine how high or low the durability/reinforce cost of your item will be, this is mostly relevant if the item that you're making is a weapon or some sort of equipment.

<figure><img src="../../.gitbook/assets/image (2).png" alt=""><figcaption></figcaption></figure>

**Cooldown Authoring** - determines the cooldown of the item when used in Handheld item slots (1-9 keys).

<figure><img src="../../.gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

**Gives Conditions When Equipped Authoring** - this component determines what "stats" your item should have, as an example +5% crit chance, +40% melee attack speed, +9001% damage against bosses.&#x20;

<figure><img src="../../.gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

**Weapon Damage Authoring** - here you can adjust the damage scaling for the item by modifying the damage multiplier, this is only relevant for weapons. Make sure that if you select magic/ranged that your object Type in Object Authoring also matches by using "Range Weapon" which counts for both Ranged and Magic weapons.&#x20;

<figure><img src="../../.gitbook/assets/image (5).png" alt=""><figcaption></figcaption></figure>

**Localization Authoring** - This will be the key to translate your item, which you'll need to match in your localization file. Core Keeper localization uses TextDataBlocks. Language Genders can be left empty.

<figure><img src="../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

**Secondary Use Authoring** - Mostly relevant for tools and weapons, this can enable a weapon such as a sword to have a secondary on-use ability which'll by default be right click.&#x20;

<figure><img src="../../.gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>
{% endstep %}
{% endstepper %}
