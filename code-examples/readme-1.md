---
description: >-
  The Workbench Example showcases how to add a new workbench with craftable
  items added via the same or another mod.
---

# Workbench Example

To learn more about how to add a new workbench I'd highly recommend checking out how workbenches are structured either in the Core Keeper Assets package, or by importing the Workbench example from the Examples.zip, as this guide will only briefly go over how to set up the main components unique to modding.

The primary Authoring Components needed are:

* Object Authoring, which primarily determines the Object Type of your item, and also Object ID of the item which must be unique relative to other existing items in the game. To give your item an Object ID you must set its' Object Name which will be converted to an Object ID when the game starts.
* Inventory Item Authoring, which makes your item into an inventory item and also gives it an inventory icon.
* Crafting Authoring, which allows us to add craftable items to the workbench.

You can set up Crafting Authoring by adding the Crafting Authoring component to your workbench and then setting the Object ID field to None.&#x20;

This will enable a new field called Modded Object ID. Into that field you can type in an Object Name, which can be found in another modded items' Object Authoring component.

<figure><img src=".gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

Once the modded items which you've referenced here are built into a mod and also the workbench is built into a mod you'll be able to craft them from your new workbench in-game.&#x20;

<figure><img src=".gitbook/assets/image.png" alt=""><figcaption></figcaption></figure>
