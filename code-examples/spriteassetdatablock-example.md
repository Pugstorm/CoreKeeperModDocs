---
description: This mod recolors existing sprite assets using GradientMap referencing.
---

# SpriteAssetDataBlock Example

First we should find a Sprite Asset which we'd like to recolor or change. Almost all of the properties on the Sprite Asset such as its' color and the texture it uses can be changed.&#x20;

However if you replace the sprites you should stick to similar naming conventions as the textures had which you replaced, since animations depend on the names. This will only be relevant for sprite assets which had animations.

<figure><img src=".gitbook/assets/image (19).png" alt=""><figcaption></figcaption></figure>

To work with modding an existing Sprite Asset, you must overload that Sprite Asset to your mods' scriptable data directory.

<figure><img src=".gitbook/assets/image (20).png" alt=""><figcaption></figcaption></figure>

Once you've done so it'll create a copy of that Sprite Asset in your mods' directory which you can modify in order to change the overloaded Sprite Asset in-game.&#x20;

To simply recolor the Sprite Asset you can assign a GradientMapDataBlock. You may either use an existing GradientMapDataBlock from the Core Keeper Assets directory, or create a new one in your directory.

<figure><img src=".gitbook/assets/image (21).png" alt=""><figcaption></figcaption></figure>
