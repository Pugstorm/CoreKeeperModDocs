---
description: >-
  This mod changes the color of the common cow from purple+white to be
  completely purple and showcases how to set up a recolor of an existing asset.
---

# GradientMapDataBlock Example

First, open the Scriptable Data Editor Window and make sure your selected Data Block directory is **Core Keeper Assets (readonly)**, if you don't see these then you first need to update game files and then update game assets. Once you've made sure you're in the right directory, you may select the Data Block type to filter by in that Directory, in this case we will filter for **Gradient Maps**.

<figure><img src=".gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

Here you can select the Gradient Map that you'd like to modify, once you've found one that you'd like to work on simply select it.

<figure><img src=".gitbook/assets/image (1) (1) (1).png" alt=""><figcaption></figcaption></figure>

Now you will need to right click the Gradient Map that you've chosen and select Overload to > your mods folder'. If you don't have a mod folder yet then you'll need to go to Mod SDK Window > Mod Management in the Unity Editor and create a new mod. If you've already made a mod but can't see your mods' folder then you either need to enable overloading in the Scriptable Data Directory asset or create one. You can enable overloading by going to this folder: \<ModName/Data/ModName.asset> and selecting the object, then in inspector checking the Enable Overloading option.&#x20;

<figure><img src=".gitbook/assets/image (28).png" alt=""><figcaption></figcaption></figure>

If you don't have a Scriptable Data Directory, then you'll need to create it by right clicking in the mods' folder and choosing Create > Scriptable Data > Additional Data Directory. Once it's created make sure Overloading is enabled and proceed to overload your gradient map to your mods' directory.

<figure><img src=".gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

Once it has been overloaded, should change the directory in the Scriptable Data Editor Window to be your mods' directory, which in this case is GradientMapDataBlockExample, you will then see that there's a new Gradient Map Data Block created here. Any changes that you make to this asset will be reflected in-game once its' built into a mod, here we change the color to be fully purple for example by changing each entry in the gradient map array to be purple.&#x20;

<figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>

If you'd like to see what your Gradient Map changes will look like, you can go back to the **Core Keeper Assets (readonly)** directory and sort by Sprite Assets, then find the Cow sprite asset and overload it to your mods' folder.&#x20;

<figure><img src=".gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

You can now see how your recolor will affect the sprite.
