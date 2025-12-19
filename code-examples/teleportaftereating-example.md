---
description: >-
  This is a mod which makes the player teleport in a random direction up to 50
  tiles around its' radius after eating anything in Core Keeper.
---

# TeleportAfterEating Example

```csharp
using HarmonyLib;
using Unity.Mathematics;
using PugTilemap;
using System;
using PlayerEquipment;


[HarmonyPatch(typeof(EatableSlot), "EatItem")]
public class TeleportAfterEating
{
    private static Unity.Mathematics.Random random;

    [HarmonyPostfix]//we use HarmonyPostFix because we want EatableSlot.EatItem() to finish first, and then call the teleportation afterwards.
    public static void TeleportIfViableTile(EquipmentUpdateAspect equipmentUpdateAspect)
    {
        var player = Manager.main.player;//get player instance

        if (equipmentUpdateAspect.entity != player.entity)//check if our player is the one that has eaten something and should be teleported.
        {
            return;//return early if it's not our player so that we don't get teleported whenever other party members eat.
        }

        random = new Unity.Mathematics.Random((uint)DateTime.Now.Ticks);//random number generating utility that is thread-safe, use this over Random.Range when working in ECS.

        var tileLookup = Manager.multiMap.GetTileLayerLookup();//a way for us to look up tiles, good for checking if a target tile is of a specific TileType.
        var playerPosition = player.WorldPosition;//get players' local position

        for (int i = 0; i < 100; i++)//iterate over up to 100 tile lookups and see if we can find a tile that we can walk on.
        {
            float2 playerOffset = random.NextFloat2(-50f, 50f);//we offset the player by up to 35 tiles in any direction from the players' current position.
            float2 targetTile = new float2(playerPosition.x, playerPosition.z) + playerOffset;//try to place the player in this position.

            var tile = tileLookup.GetTopTile((int2)math.round(targetTile)).tileType;//check what the tile type is of the tile which we want to place our player on.

            if (tile != TileType.none && tile != TileType.water && tile != TileType.pit && TileTypeUtility.IsWalkableTile(tile) && !TileTypeUtility.IsBlockingTile(tile))//check if the tile type that we retrieved is a viable tile for the player to be on.
            {
                player.QueueInputAction(new UIInputActionData//teleport the player, we do this instead of transform.position because Core Keeper is server authoritative, meaning you have to tell the server what to do.
                                                             //if you try to do something like this on the client your player will rubberband because the server will see a mismatch on what is the players' actual position.
                {
                    action = UIInputAction.Teleport,//the actual action of teleporting.
                    position = targetTile//where to teleport the player to.
                });
                break;//end the loop early since we found a viable tile to teleport the player to, so the loop doesn't keep running until i reaches 100.
            }
        }
    }
}

```
