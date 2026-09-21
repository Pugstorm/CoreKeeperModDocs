---
description: This page lists all known issues.
icon: bug
---

# Known Issues

#### Steam Workshop uploading tool occupying Core Keeper APP

Currently the tooling required for this to not happen is in place but not working as intended, this is fixed on our end and will be shipped with the next stability patch.

#### Ghost Component cap

Creating more than a handful new ghost components results in the game crashing. This will be fixed in the next stability patch, most likely early next week.

#### SDK Error: LimitExceeded

This happens when you are trying to upload an art asset to Steam Workshop that exceeds their file limit. This limit is set to 5MB by Steam and we are currently working on implementing this file limit on the SDK side as well. For now, please refrain from using images that are larger than 5MB.&#x20;
