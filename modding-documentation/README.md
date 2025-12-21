# Setting up the Modding Environment

The Core Keeper Mod SDK (Software Development Kit) is a Unity project which can be opened with Unity version 6000.0.59f2.&#x20;

1. Download the project to your PC, either using Git or by clicking _Code > Download ZIP_ and extracting the archive anywhere.
2. Install and open Unity Hub. Press "Add -> Add project from disk" and select the folder where you downloaded the SDK. The project should now appear in the Unity Hub projects list.
3. (Optional) Press the three dot menu next to the project, select "Add command line arguments", add `-disable-assembly-updater` and press Save. This is not strictly required, but it will remove some errors from the log when you open the project.
4. Open the project. If you do not already have a matching Unity Editor version, Unity Hub will guide you through installing it. Make sure to enable "Linux Build Support (Mono)" or you won't be able to install or build your mods. All other options can be left at their default values.

You might occasionally see some error in the log. This is not necessarily always a problem, especially before fully loading the game files into the project, but we will try to keep this as clean as possible.
