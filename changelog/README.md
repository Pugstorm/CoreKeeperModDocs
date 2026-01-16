---
description: >-
  These are the primary changes happening from patch 1.1.2.9 going into patch
  1.2
---

# Patch 1.2.0

* UnityExtensions has moved to a new PugUnityExtensions package and Pug.UnityExtensions namespace.
* PugProperties has moved to a new PugProperties package and Pug.Properties namespace.
* TileMapLookup.GetBlockingTile and TileMapLookup.HasBlockingTile has been deprecated in favor of TileMapLookup.TryGetBlockingTile and will get removed in a future release.
* PugAutomation namespace has been renamed to Pug.Automation.
* PugElectricity namespace has been merged into Pug.Automation.
* ElectricityCD is now part of the Pug.Automation namespace.
* PugConversion has been extracted as a separate package. There are a few changes that may require you to update any existing converter code.
* PugConversion namespace has been renamed to Pug.Conversion.
* PugConverter and PugPostConverter have been renamed to Converter and PostConverter, respectively.
* The ConditionsTable asset is no longer cached on each individual converter. Converters that need access to the conditions table should load and cache it themselves. See e.g. LevelEntitiesBufferConverter for an example.
* Added BurstDisabler API to selectively disable burst compiler for specific systems. This is currently limited to disabling Burst for ISystem. We are working on extending this to work with Burst-compiled jobs as well. You may see some jobs running without Burst using this API currently, but results are inconsistent.
* Core Keeper now uses ScriptableDataBlocks which are just like Scriptable Objects but hold an address synced to the assets' GUID.
* Gradient Maps are now stored as a texture array inside of GradientMapDataBlocks.
* SpriteAssets are now SpriteAssetDataBlocks.
* SpriteAssetSkins are now SpriteAssetSkinDataBlocks.
* PlayerCustomizationTable is now PlayerCustomizationTableDataBlock.
* Sprite Objects now reference SpriteAssetDataBlocks and GradientMapDataBlocks.
* Mods can now be loaded from Steam Workshop as well.
* Localization now works through the TextDataBlock system, to add or change in-game text you will need to create a new TextDataBlock instead of a Localization.csv file.
* A majority of the ModSDK has been moved from assets into a PugMod package.
