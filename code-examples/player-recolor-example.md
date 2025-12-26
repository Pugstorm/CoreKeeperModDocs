# Player Recolor Example

Some assets use AssetReference fields. These are weak references which are resolved by Addressables which is why you need to make an asset an Addressable by checking the "Addressable" checkmark at the top of the inspector before you can assign it to an AssetReference field.

Addressables aren't used to build mods though. We add the necessary identifier to mods for AssetReference to work and then the game loads the identifier and asset into Addressables when we load the mod.

This means Addressables settings in the Mod SDK project does not have any effect, so you can ignore any label and group setting even though some assets might get some labels etc. automatically.

<figure><img src=".gitbook/assets/image (15).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (17).png" alt=""><figcaption></figcaption></figure>

<figure><img src=".gitbook/assets/image (18).png" alt=""><figcaption></figcaption></figure>
