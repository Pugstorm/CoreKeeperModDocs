---
description: This page lists all known issues.
icon: bug
---

# Known Issues

#### Ghost Component cap

Creating more than a handful new ghost components results in the game crashing. ~~This will be fixed in the next stability patch, most likely early next week~~. This issue has been fixed, but a related issue has appeared where exceeding the player archetypes' limit causes similar crashes. The workaround for this is the same as before - to have less components. We are investigating the issue.

#### SDK Error: LimitExceeded

This happens when you are trying to upload an art asset to Steam Workshop that exceeds their file limit. This limit is set to 5MB by Steam and we are currently working on implementing this file limit on the SDK side as well. For now, please refrain from using images that are larger than 5MB.&#x20;
