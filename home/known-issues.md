---
description: This page lists all known issues.
icon: bug
---

# Known Issues

#### Unity takes up the "Core Keeper" game on Steam

This is a known issue when you have initialized steam. Since the SDK hooks into the Steam Client to upload mod to the Workshop, Steam will treat the Unity process as the game. Only fix once this happens is to close Unity.

For now it's best to only initialize steam once you have tested the mod and are ready to upload to the Workshop. We are looking into ways to prevent this in the future though.

#### SDK Error: LimitExceeded

This happens when you are trying to upload an art asset to Steam Workshop that exceeds their file limit. This limit is set to 5MB by Steam and we are currently working on implementing this file limit on the SDK side as well. For now, please refrain from using images that are larger than 5MB.&#x20;
