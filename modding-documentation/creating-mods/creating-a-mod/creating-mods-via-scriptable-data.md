---
description: >-
  This page aims to guide the user through the workflow of getting started with
  Scriptable Data mods as well as provide tips and tricks.
---

# Creating Mods via Scriptable Data

### Scriptable Data Editor Window

You can discover the Scriptable Data Editor by going to `Window > Scriptable Data Editor`.

<figure><img src="../../.gitbook/assets/image (26).png" alt=""><figcaption></figcaption></figure>

### How to create new assets, and where to find games' assets

All the Sprite Assets that are used in Core Keeper will be located at this path once you've updated the game assets: `Packages/dev.pugstorm.corekeeper.assets/Data/SpriteAsset`.&#x20;

If you want to add new assets you can do so by adding a Scriptable Data Directory asset in your mods' folder and then opening up the Scriptable Data Editor Window and creating an asset of that type.

<figure><img src="../../.gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

To create a new Sprite Asset you'd filter by Sprite Asset type in top Left (make sure your mods' Scriptable Data Directory is selected as well as that's where the Sprite Asset will be created) and clicking Add new Data Block. The same process can be repeated for other Asset Types such as Sprite Asset Skins, Gradient Maps and so on.

### How to convert old Sprite Assets and Sprite Asset Skins from Scriptable Objects to ScriptableDataBlocks.

Create a Scriptable Data Directory assets via the create menu, and make sure your Sprite Assets are located in a sub-directory of where your Scriptable Data Directory asset is located, then open the asset in the Scriptable Data Editor Window.

## Step-by-step guide

{% stepper %}
{% step %}
### Creating a mod base

Head over to the `PugMod` window in Unity, and select `Open Mod SDK Window`. Then head over to the Mod Settings tab, and select `New Mod`, afterwards name your mod and press `Create`. This will generate a mod folder including a scriptable object containing the mods' build settings under the `Assets/<YourModNameFolder>` path. An assembly will also be generated which'll reference all of the games' assemblies that you've fetched by updating game files, this is nothing very useful for now but good to know in the future in case you run into outdated assembly issues.
{% endstep %}

{% step %}
### Creating Scriptable Data Directory config

To mod a ScriptableDataBlock you will first need to make sure that you have a Scriptable Object in your Mods' Folder called \<ModName>, of the type `ScriptableDataDirectory`.&#x20;

<figure><img src="../../.gitbook/assets/image (6) (1) (1).png" alt=""><figcaption></figcaption></figure>

If your mod is older then you can simply do this by heading over to your mods' folder and then generating the asset by going to the Assets window, and selecting `Create > Scriptable Data > Additional Data Directory`. This will generate the required asset. Make sure it's in the mod folder in which you want to mod Scriptable Data Blocks!

<figure><img src="../../.gitbook/assets/image (7) (1) (1).png" alt=""><figcaption></figcaption></figure>
{% endstep %}

{% step %}
### Choosing a ScriptableDataBlock to mod

Open up the Scriptable Data Editor Window using `Window > Scriptable Data Editor` <mark style="color:$info;">,</mark> once it's open you'll see a dropdown for ScriptableDataBlock types, these types are retrieved from the assemblies that are imported into the ModSDK during the "Updating Game Files" step. Once you've picked a ScriptableDataBlock which you think would be fun to mod you can right click it on the left-hand side in the Scriptable Data Editor Window and click `Overload to > <YourScriptableDataDirectoryConfig>`.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1) (1).png" alt=""><figcaption></figcaption></figure>

Make sure the config is located in the right mod folder for which you want to build this Scriptable Data mod!
{% endstep %}

{% step %}
### Editing your modded ScriptableDataBlock

ScriptableDataBlocks can be edited in the inspector by selecting the instance of that ScriptableDataBlock in your mods' folder, but it's easier and more convenient due to several QoL features to do so in the Scriptable Data Editor Window.&#x20;

<figure><img src="../../.gitbook/assets/image (1) (1) (1) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Simply make the changes that you'd like to make to the ScriptableDataBlock and then build the mod. The easiest way to test that you've set up everything correctly and that the Scriptable Data Block mods will work is to assign a new `GradientMapDataBlock` to a Sprite Asset.

<figure><img src="../../.gitbook/assets/image (2) (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Then once you've saved the changes and built the mod, you can launch your game and test if the ScriptableDataBlock mod changes are reflected in-game.
{% endstep %}
{% endstepper %}
