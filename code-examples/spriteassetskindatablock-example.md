---
description: >-
  This mod recolors existing sprite variants (Sprite Asset Skins) using
  GradientMap referencing.
---

# SpriteAssetSkinDataBlock Example

First we should find a Sprite Asset Skin which we'd like to recolor or change. Almost all of the properties on the Sprite Asset Skin such as its' color and the texture it uses can be changed.

The `Target Asset Ref` field should not be changed, as it points to the Sprite Asset of which this Sprite Asset Skin is a variant of.

If you replace the sprites you should stick to similar naming conventions as the textures had which you replaced, since animations depend on the names. This will only be relevant for sprite asset skins which had animations.

To work with modding an existing Sprite Asset Skin, you must overload that Sprite Asset Skin to your mods' scriptable data directory.

<figure><img src=".gitbook/assets/image (22).png" alt=""><figcaption></figcaption></figure>

Once you've done so it'll create a copy of that Sprite Asset Skin in your mods' directory which you can modify in order to change the overloaded Sprite Asset in-game.&#x20;

To simply recolor the Sprite Asset Skin you can assign a GradientMapDataBlock. You may either use an existing GradientMapDataBlock from the Core Keeper Assets directory, or create a new one in your directory.

<figure><img src=".gitbook/assets/image (23).png" alt=""><figcaption></figcaption></figure>
