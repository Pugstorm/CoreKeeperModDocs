---
description: This page aims to help you get started with Scriptable Data modding.
---

# Creating Mods via Scriptable Data

### What is Scriptable Data?

Scriptable Data is what Core Keeper uses to store and organize information about objects within the game.&#x20;

For example: the player character's eye color, or the appearance of an armor piece.

The core idea of Scriptable Data modding is that players can override this information by creating their own pieces of Scriptable Data, called Scriptable Data Blocks.

There are many different types of Scriptable Data Blocks which store different information, some may store an armor skin, while others may store the color of the fog in an area in-game.

The main way to interact with Scriptable Data Block types and to manage them is through the Scriptable Data Editor Window.

### Why would we want to use Scriptable Data for modding?

Scriptable Data offers an easy alternative to get into modding, since it doesn't require you to write any code.&#x20;

It's still a bit technical, but with this guide you will be able to make new Scriptable Data Blocks which override an existing object's information.

Scriptable Data also enables modding which is difficult through code alone, such as adding new custom scenes to Core Keeper!

### Scriptable Data Editor Window

You can discover the Scriptable Data Editor Window by going to:

`Window > Scriptable Data Editor`.

<figure><img src="../../.gitbook/assets/image (51).png" alt=""><figcaption></figcaption></figure>

### Navigating the Scriptable Data Editor Window

The first time that you open it, your Scriptable Data Editor will look something like this.&#x20;

<figure><img src="../../.gitbook/assets/image (53).png" alt=""><figcaption></figcaption></figure>

There are a couple of important things to know before going further.&#x20;

* **Scriptable Data Directory** - this is an asset which will be located in your mod's Data folder, it is used to sort Scriptable Data Blocks by that directory.&#x20;

You can have multiple Scriptable Data Directories per mod if you'd like to.&#x20;

A simple way to think about these directory assets is that it's a way to sort data blocks by their location.

<figure><img src="../../.gitbook/assets/image (54).png" alt=""><figcaption></figcaption></figure>

Clicking on the highlighted dropdown will allow you to choose which Scriptable Data Directory you want to sort by.&#x20;

If you already created a mod via the ModSDK Window then you will find a directory asset in this dropdown named after your mod, let's sort by the CustomScene directory for example!

<figure><img src="../../.gitbook/assets/image (55).png" alt=""><figcaption></figcaption></figure>

You'll notice that the list of items on the left is now empty, this is because in our CustomScene directory we haven't created any Cursor Skin Data Block collections, which is called a Scriptable Data Block Type.

* **Scriptable Data Block Type** is another way to sort our data even further, which can be used to only show data blocks belonging to a specific type.

A few examples of different types would be:&#x20;

* Gradient Map Data Block which stores the color variations of cattle.
* Sprite Asset Data Block which stores the base textures of many objects in-game.
* Custom Scene Data Block which stores the data of custom scenes.&#x20;

These are all different types of Scriptable Data Blocks which store different data, but at the end of the day they are all Scriptable Data Blocks.&#x20;

Let's sort by the Custom Scene type! You can click the currently selected type to activate the dropdown, and then select Custom Scene from the dropdown.

<figure><img src="../../.gitbook/assets/image (57).png" alt=""><figcaption></figcaption></figure>

Now we're in the directory which we want to work in, with the Scriptable Data Block type selected which we want to work with.&#x20;

Let's create a new Custom Scene data block, you can do so by pressing `Add new Data Block` at the bottom left corner.

<figure><img src="../../.gitbook/assets/image (59).png" alt=""><figcaption></figcaption></figure>

You'll have the option to name your data block, if you'd like to rename it at any point you may right click it and select Rename.&#x20;

<figure><img src="../../.gitbook/assets/image (60).png" alt=""><figcaption></figcaption></figure>

Some data blocks types will have values inside of them that you can modify, textures you can reference, and so on.

The Custom Scene Data Block in particular holds the data of scenes which are generated in-game, such as this one:

<figure><img src="../../.gitbook/assets/image (62).png" alt=""><figcaption></figcaption></figure>

This is a pre-built scene, alternatively called a custom scene which was created in Unity, with it's data stored in a Custom Scene Data Block and then created in-game from that data.

To continue with creating a custom scene, please [continue here](custom-scene-creation.md)!

### Where to find Scriptable Data Blocks used in Core Keeper

All the Scriptable Data Blocks which are used in Core Keeper will be located at this path once you've updated the game assets: `Packages/dev.pugstorm.corekeeper.assets/Data`.&#x20;

<figure><img src="../../.gitbook/assets/image (63).png" alt=""><figcaption></figcaption></figure>

Sometimes you might not want to create a completely new object though! Let's say you want to change the way that an existing object looks or behaves, in these kind of cases it's great to know about **Scriptable Data Block Overloading**.

You can "overload" a data block, or rather overwrite it by right clicking any data block from the Core Keeper Assets directory, via the Scriptable Data Editor Window.

<figure><img src="../../.gitbook/assets/image (65).png" alt=""><figcaption></figcaption></figure>

You will need to pick which directory you want to overload your data block to, these will usually named after your mods, so make your choice based on which mod you want to contain these changes.

Once you've made your choice, a copy of the data block which you overloaded will be created in that directory.&#x20;

<figure><img src="../../.gitbook/assets/image (66).png" alt=""><figcaption></figcaption></figure>

Any changes that you have made to this copy, will then be represented in-game after you've built your mod.

You may make any type of changes to these data blocks and overwrite any fields as you see fit.

Overloading can also be a method to easily create copies of existing data blocks, all you need to do in the case that you want it to act as it's own data block and not overwrite anything is to clear the reference in the overload field on your new copy.

<figure><img src="../../.gitbook/assets/image (67).png" alt=""><figcaption></figcaption></figure>

You can clear the reference by selecting the referenced data block as shown above, and pressing the `Del` key.

## Step-by-step guide

{% stepper %}
{% step %}
### Creating a mod base

Head over to the `PugMod` menu in Unity, and select `Open Mod SDK Window`.&#x20;

Then head over to the Mod Settings tab, and select `New Mod`, afterwards name your mod and press `Create`.&#x20;

This will generate a mod folder including a scriptable object containing the mods' build settings under the `Assets/<YourModNameFolder>` path.&#x20;

An assembly will also be generated which'll reference all of the games' assemblies that you've fetched by updating game files, this is nothing very useful for now but good to know in the future in case you run into outdated assembly issues.
{% endstep %}

{% step %}
### Choosing an existing ScriptableDataBlock to mod

Open up the Scriptable Data Editor Window using `Window > Scriptable Data Editor` .

Once it's open you'll see a dropdown for ScriptableDataBlock types, these types are retrieved from the assemblies that are imported into the ModSDK during the "Updating Game Files" step.

Once you've picked a ScriptableDataBlock which you think would be fun to mod you can right click it on the left-hand side in the Scriptable Data Editor Window and click `Overload to > <YourScriptableDataDirectoryConfig>`.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Make sure the config is located in the right mod folder for which you want to build this Scriptable Data mod!
{% endstep %}

{% step %}
### Editing your modded ScriptableDataBlock

ScriptableDataBlocks can be edited in the inspector by selecting the instance of that ScriptableDataBlock in your mod's folder, but it's easier and more convenient due to several QoL features to do so in the Scriptable Data Editor Window.

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Simply make the changes that you'd like to make to the ScriptableDataBlock and then build the mod.&#x20;

The easiest way to test that you've set up everything correctly and that the Scriptable Data Block mods will work is to assign a new `GradientMapDataBlock` to a Sprite Asset.

{% hint style="warning" %}
Currently if you reference anything from the Core Keeper Assets package, those references may be lost the next time you update game assets.

We're working on a solution for this issue, but be mindful that you should create copies of assets and then drag them into your mod's folder, and to then reference those copies instead.
{% endhint %}

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Then, once you've saved the changes you are ready to build the mod.&#x20;

For this head on over to the Mod SDK window again, go to Mod Management and click `Build and Install Mod`. It's normal for the building process to take a few minutes.

<figure><img src="../../.gitbook/assets/image (68).png" alt=""><figcaption></figcaption></figure>

Once you've built the mod, you can launch your game and test if the ScriptableDataBlock mod changes are reflected in-game.
{% endstep %}
{% endstepper %}
