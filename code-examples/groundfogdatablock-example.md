---
description: >-
  This mod adds a mist to certain tiles, however the modification of existing
  ground fog data blocks via overloading is also possible.
---

# GroundFogDataBlock Example

Once we've created a mod which'll contain our ground fog, we head over to the Scriptable Data Editor Window and create a new data block in our mods' data block directory.

<figure><img src=".gitbook/assets/image (3) (1).png" alt=""><figcaption></figcaption></figure>

Not all Tile Types and not all Tileset combinations will generate a fog, I'd recommend testing out with the Tile Type: Pit and Tileset: Dirt first to make sure that everything is set up and working, then test it out in-game once you've built the mod by shoveling a pit or going to an area with a pit.

<figure><img src=".gitbook/assets/image (4) (1).png" alt=""><figcaption></figcaption></figure>

If the Alpha Value is set to 0, then the fog will be invisible.

<figure><img src=".gitbook/assets/image (5) (1).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (24).png" alt=""><figcaption></figcaption></figure>
