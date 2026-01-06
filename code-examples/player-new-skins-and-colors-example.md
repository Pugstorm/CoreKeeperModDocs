---
description: This mod adds new player skins and recolors for those skins.
---

# Player New Skins & Colors Example

We can create a new player skin by creating a new Texture that follows the conventions of Core Keepers' player skins. The way that I made this example was by taking the Miner\_skin texture which you can find in the Core Keeper Assets package or the Vampire\_skin in the PlayerNewSkinsAndColorsExample mod, and modifying it in a pixel art editor such as Pixelorama.&#x20;

You can repeat this process for any of the player characters' textures, primarily Eyes, Shirt, Pants, Skin.&#x20;

<figure><img src=".gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

Some assets use AssetReference fields. These are weak references which are resolved by Addressables which is why you need to make an asset an Addressable by checking the "Addressable" checkmark at the top of the inspector before you can assign it to an AssetReference field.

Addressables aren't used to build mods though. We add the necessary identifier to mods for AssetReference to work and then the game loads the identifier and asset into Addressables when we load the mod.

This means Addressables settings in the Mod SDK project does not have any effect, so you can ignore any label and group setting even though some assets might get some labels etc. automatically.

<figure><img src=".gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (7).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

Once you've created the data blocks and referenced the textures you must mark those DataBlocks as dynamic on the left-hand side, this will dynamically add them to the Skins collection, do this for every new texture that isn't an overload.

Next we will need to set up the source colors for our players' recolors, the source colors will target which colors on the texture to replace, for example if our base pants texture that we've referenced on the pants data block has a red color, then we must get that colors' code and add it as a new color in our source color data block. Whenever we make a replacement color data block and reference it on our pants skin data block it'll replace the targeted colors with what's in our replacement color data block. This happens when we skim left or right during player customization, with that skin type selected.&#x20;

<figure><img src=".gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>

Once you've finished customizing your new player skin mod and have installed it you can skim left or right on the body type in character customization to discover your new skin!

<figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
