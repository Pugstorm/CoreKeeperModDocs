---
description: >-
  this page explains how you can give an item an inventory name and a
  description as well as localize it to other languages when the games' language
  is changed.
---

# How to Localize Your Mod

### TextDataBlock

<figure><img src="../../.gitbook/assets/image (1) (1).png" alt=""><figcaption></figcaption></figure>

A `TextDataBlock` can define two strings: a title and a description. Both are optional, as long as the other is provided (meaning you can have a `TextDataBlock` with just a title or just a description).

The key used for runtime lookups is the name of the data block. Keys should use the following conventions:

* Use a letter (as opposed to a number) for the first character.
* Use alphanumeric characters only (with the exception of underscores).

### How to localize an item using a TextDataBlock

First, make sure you have a Scriptable Data Directory asset in your mods' folder, then head over to the Scriptable Data Editor Window and select your mods' Data Block Directory, you can also open this window by clicking "Open Scriptable Data Editor" directly on your Scriptable Data Directory asset. Make sure to sort by Text, and click "Add new Data Block" in the bottom left corner of the window.

<figure><img src="../../.gitbook/assets/image (12).png" alt=""><figcaption></figcaption></figure>

Name your TextDataBlock after your items' Title and proceed to fill out the DataBlocks' Title as well as Description for English as well as any languages that you'd like to localize it to. Finally, press "Mark Localization as Up-To-Date".

<figure><img src="../../.gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>

Your item will now have an in-game title and description when viewed in the inventory in the localized languages.&#x20;

### How to upgrade existing .CSV files to use TextDataBlocks

First, make sure that your .CSV files are separated as such. I find this to be the most convenient format however more formats are supported by selecting another Separator character when Importing Localization using the Scriptable Data Editor Window.&#x20;

<figure><img src="../../.gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

Once we've formatted our .CSV files to match the conventions, we can proceed to open the Scriptable Data Editor window and sort by the Scriptable Data Directory into which you want to add the TextDataBlock for localization as well as filter by Text, then click on the cogWheel and select "Import Localization".

<figure><img src="../../.gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

Here you must check "Missing Data Blocks" and "Overwrite Primary Language" and then choose your Separator Character, we will use | in this example as that's what we use to separate entries in our .CSV file above, then select the path to your .CSV file. Make sure your .CSV file is closed or it will throw an error.

<figure><img src="../../.gitbook/assets/image (6) (1).png" alt=""><figcaption></figcaption></figure>

Once you've selected your .CSV file, press Import to create a new TextDataBlock using information from the file.

<figure><img src="../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src="../../.gitbook/assets/image (9).png" alt=""><figcaption></figcaption></figure>

Once you select your TextDataBlock you must mark it as up to date by pressing "Mark Localization as Up-To-Date", if you'd like to localize more languages than you had previously you can do so by filling out extra language entries on the datablock itself.

<figure><img src="../../.gitbook/assets/image (10).png" alt=""><figcaption></figcaption></figure>

You may now move your .CSV file to another directory to save it as a back-up, and build your mod to test that the TextDataBlock has been set up correctly.

<figure><img src="../../.gitbook/assets/image (11).png" alt=""><figcaption></figcaption></figure>
