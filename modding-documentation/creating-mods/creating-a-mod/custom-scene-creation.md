---
description: >-
  this guide will go over custom scene creation, the tools available and how to
  upload your custom scenes to steam workshop or test them in-game.
---

# Custom Scene Creation

First, make sure you've updated your game files and game assets! Once you've done so you may begin with clicking on the `PugMod` menu option at the top, and proceeding to `Open Mod SDK Window`.

<figure><img src="../../.gitbook/assets/image (29).png" alt=""><figcaption></figcaption></figure>

You should then proceed to the `Mod Management` tab and enter a name for your new mod, once you've set a name you may press `Create Mod`. This may take a few minutes while Unity creates your new mod.

<figure><img src="../../.gitbook/assets/image (30).png" alt=""><figcaption></figcaption></figure>

After Unity has finished processing, you will notice that in your assets you now have a new folder named after your mod, everything within this folder will be built into your mod later down the line.

<figure><img src="../../.gitbook/assets/image (31).png" alt=""><figcaption></figcaption></figure>

Double click your mod's folder, and then double click the `Data` folder. Inside of it you will see an asset which you should click once in order to open it up in the inspector window. Once you've done so, click `Open Scriptable Data Editor`. If the Scriptable Data window is already open next to the Inspector window, you may click that instead.

<figure><img src="../../.gitbook/assets/image (32).png" alt=""><figcaption></figcaption></figure>

After navigating to the Scriptable Data Editor Window you will need to select your mod's directory, you can do so by clicking on `Data` next to the `Data Blocks` label and then selecting your mod's directory.

<figure><img src="../../.gitbook/assets/image (33).png" alt=""><figcaption></figcaption></figure>

You'll then need to navigate to the `Custom Scene` type, you can do so by pressing the dropdown right below the directory dropdown, pressing the `<` arrow if needed and selecting `Custom Scene`.

<figure><img src="../../.gitbook/assets/image (34).png" alt=""><figcaption></figcaption></figure>

### Creating a Custom Scene

Next press the `+` sign and select `Data Block`. You'll have to name the scene and then press enter to save.

<figure><img src="../../.gitbook/assets/image (35).png" alt=""><figcaption></figcaption></figure>

You will now create your custom scene! Press `Create scene from template`.

<figure><img src="../../.gitbook/assets/image (36).png" alt=""><figcaption></figcaption></figure>

Once your custom scene is created, you'll notice that there are a few interesting fields, namely:

* Max Occurences, which represents the maximum amount of times that your custom scene may spawn in the world. You may increase this to any number but it should always be at least 1.
* Biomes To Spawn In, which represents which biomes your custom scene can spawn in. If you'd like to make your custom scene spawn in another biome other than `Slime`, you can click on the dropdown and select another biome. If you'd like to make your scene spawn in multiple biomes, you may press the `+` button to add an entry to the list of biomes which your scene may spawn in. These should be unique entries.&#x20;

The rest of the settings are advanced use cases, so we won't cover them in this guide, although if you've installed your custom scene mod and can't spawn your scene, I'd recommend pressing `Reprocess this scene`.

<figure><img src="../../.gitbook/assets/image (37).png" alt=""><figcaption></figcaption></figure>

You may now exit out of the Scriptable Data Editor Window and press `Open Tile Editor Window`.

<figure><img src="../../.gitbook/assets/image (38).png" alt=""><figcaption></figcaption></figure>

This will open up the custom scene creation window, which you can use to customize your custom scene. If you'd like to zoom in or zoom out you may use the mouse scroll wheel. To move your camera around you can use `WASD` or mouse keys alongside your right mouse click to control the camera angle.

You will notice some errors in the console log, these are harmless and can be ignored, and will be removed in a future version.&#x20;

<figure><img src="../../.gitbook/assets/image (39).png" alt=""><figcaption></figcaption></figure>

You may for example select the dirt ground icon, select the `Fill` button and press your left mouse key in the scene to draw.

<figure><img src="../../.gitbook/assets/image (40).png" alt=""><figcaption></figcaption></figure>

You'll notice that some highlights are green, while some are yellow. A green highlight indicates that a new block will be placed, and yellow indicates that an existing block will be replaced. There's also a red highlight which indicates that a tile cannot be placed in the red highlighted area.

<figure><img src="../../.gitbook/assets/image (41).png" alt=""><figcaption></figcaption></figure>

### How to use the Custom Scene Creation tools

#### Tile Palette

Tiles, in Core Keeper, are Ground and Wall Blocks and other similar items that can be used in the game. Tiles also include Bridges, Floor Tiles (such as Wood Floor, Stone Floor, Rugs, etc.) and Fences.

In the Tile palette section, you'll also be able to see all small details that can be added to a scene to give it a more natural look, which includes Ceiling Holes, Water, Abyss, Roots, Small Stones, Grass and many more.

You'll notice that some of these are also separated by color. So if you'd like to use a specific colored tile, then you'll need to select the correct color before placing it in your scene.

<figure><img src="../../.gitbook/assets/image (42).png" alt=""><figcaption></figcaption></figure>

#### Favorite Tiles

You may favorite or unfavorite tiles so that you don't need to constantly swap between different tilesets. You can favorite any tile which you have currently selected by pressing `Add Tile to Favorites`.

#### Placeable Objects

Objects, in Core Keeper, are all items that are prefabs. All items in this area are part of the Ghost Collection (something we can see when processing scenes). That's why you'll notice that all these items will have a small Ghost icon on the tile where they were placed.

Objects are separated in categories, which are similar to the tilesets for the tiles.

You can also search for Placeable Objects by name, instead of using the dropdown.

Some of these items will not be placeable objects that'll occupy a tile, such as armor or swords.

<figure><img src="../../.gitbook/assets/image (43).png" alt=""><figcaption></figcaption></figure>

### Available tools

#### Paint

The Paint tool is the primary utility used for drawing individual tiles or objects onto the scene.

You may adjust your brush size when drawing tiles by using `[` or `]` keys. These can be re-key bound in `Actions` -> `Keyboard Shortcuts`.

Holding the Shift key while using this tool will turn it into an eraser.

#### Fill

The Fill tool can be used to fill an area with your selected tile type.

#### BoxFill

The BoxFill tool allows you to click and drag a rectangular box to paint tiles across a large area.

While dragging, you will notice the before mentioned green, yellow and red highlights which indicate where new tiles will be placed, or replaced or where tiles can't be placed at all with the selected tile type. Releasing the mouse button applies the tile choice to the rectangular box. Holding Shift while dragging will clear all tiles within rectangle.

#### Select

The Select tool is used to select a rectangular region and modify it.

Once an area is highlighted, you can cut, copy, or paste its full contents (including both tiles and prefabs), drag the selected block to move it to a new position, or delete tiles and prefabs independently.

The tool also provides quick transformation options to flip the selection horizontally or vertically, or rotate the selected area.

#### Center

The Center tool is used for navigating to the center of your scene.

Activating this tool instantly snaps and centers the Unity Scene View camera directly onto the main map coordinate origin (0, 0, 0).

This serves as a quick reset button if you lose your orientation or scroll too far away from your custom scene boundaries while editing.

#### Sample

The Sample tool acts as an eyedropper utility, enabling you to clone assets directly from the scene view.

Clicking on any placed tile or prefab in the scene view automatically samples it, updating your active brush palette to the matching tile type, tileset, or object category without requiring you to look it up manually.

#### Shortcuts Window

By heading over to `Actions` in the top right corner of the custom scene creation window and pressing `Keyboard Shortcuts` you may see all the available shortcuts for using these tools more seamlessly. Those which have a dropdown may be re-key bound.

<figure><img src="../../.gitbook/assets/image (44).png" alt=""><figcaption></figcaption></figure>

### Building your scene into a mod

To build your custom scene you should proceed back to the Mod SDK Window, by pressing `PugMod` in the menu options and then selecting `Open Mod SDK Window`. Once you're there you should proceed to the `Mod Management` tab and press `Build and Install Mod`.

You will receive a pop-up asking you if you would like for your custom scene to be processed, you should select `Ok`. Once your custom scene has been processed you may press `Ok` again, your mod will now be built for you locally.

<figure><img src="../../.gitbook/assets/image (45).png" alt=""><figcaption></figcaption></figure>

### Testing the scene in-game

To test your custom scene in-game you should install the `Mod Utilies` mod as well, by extracting the examples.zip into your assets folder, and then building the `Mod Utilities` mod locally via the `Build and Install Mod` option, after selecting it from the dropdown.

Then, in-game you will be able to press `Ctrl+x` to open up the developer console, and enter `modding.spawnCustomScene Mod_{sceneName}` . <br>

<figure><img src="../../.gitbook/assets/image (47).png" alt=""><figcaption></figcaption></figure>

This will spawn your custom scene in-game!&#x20;

<figure><img src="../../.gitbook/assets/image (48).png" alt=""><figcaption></figcaption></figure>

### Publishing your scene

To publish your custom scene you should open the `Steam Workshop` tab in the Mod SDK Window and select your mod from the dropdown, then fill out the fields as you see fit! Once you're ready, press `Upload Mod to Steam Workshop`.&#x20;

From now on forth will be able to update your mod's tags, visibility, description, title or contents by adjusting any of these fields and pressing `Update Mod on Steam Workshop`.

<figure><img src="../../.gitbook/assets/image (49).png" alt=""><figcaption></figcaption></figure>

Once it has been uploaded, you can press `Go to Mod page` to go to your mod's page on Steam Workshop.&#x20;

Other players will now be able to download and use your scene in-game by subscribing to this mod.

<figure><img src="../../.gitbook/assets/image (50).png" alt=""><figcaption></figcaption></figure>
