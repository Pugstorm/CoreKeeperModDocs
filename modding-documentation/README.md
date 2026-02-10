---
description: >-
  This page will walk you through how to set up unity and install the ModSDK
  unity project.
---

# Setting up the Modding Environment

You will need to have Unity to open up the ModSDK.

The Core Keeper Mod SDK (Software Development Kit) is a Unity project which can be opened with Unity version `6000.0.59f2`.&#x20;

1.  Download the project to your PC, either using Git or by clicking the green `Code > Download ZIP` button on the right side of the page and extracting the archive anywhere.\
    Git is a version control system that helps track changes and if needed revert them. Please see [this link](https://docs.github.com/en/get-started/using-git/about-git) for additional information.

    <figure><img src=".gitbook/assets/image (2) (1).png" alt=""><figcaption></figcaption></figure>
2. Install and open Unity Hub. You can install it from [here](https://unity.com/download).
3.  Please install the required Unity version from [this link](https://unity.com/releases/editor/archive) (6000.0.59f2). You will have to filter by "LTS" so that you can find the correct version. Once you found the correct version, press the blue install button.

    <figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
4. In Unity Hub, press "Add -> Add project from disk" in the top right of the window and select the folder where you downloaded the SDK. The project should now appear in the Unity Hub projects list.\
   <img src=".gitbook/assets/image (3) (1).png" alt="" data-size="original">
5. (Optional) Press the three dot menu next to the project, select "Add command line arguments", add `-disable-assembly-updater` and press Save. This is not strictly required, but it will remove some errors from the log when you open the project.\
   ![](<.gitbook/assets/image (8).png>)![](<.gitbook/assets/image (9).png>)
6. Open the project. If you do not already have a matching Unity Editor version, Unity Hub will guide you through installing it. Make sure to enable "Linux Build Support (Mono)" or you won't be able to install or build your mods. All other options can be left at their default values.

You might occasionally see some errors in the log. This is not necessarily always a problem, especially before fully loading the game files into the project, but we will try to keep this as clean as possible.
