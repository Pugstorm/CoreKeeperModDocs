---
description: >-
  The BurstDisabler example is a simple show-case for the BurstDisabler API
  which can be used to selectively disable burst on systems.
---

# BurstDisabler Example

```csharp
using PugMod;
using Unity.Burst;
using Unity.Entities;
using UnityEngine;

public class BurstDisable : IMod
{
	public void EarlyInit()
	{
		// Can't call BurstDisabled here since we are missing some necessary type initialization at this point
	}

	public void Init()
	{
		// Call to disable burst for these specific systems so we can patch them
		BurstDisabler.DisableBurstForSystem<SpawnEnvironmentObjectsInNewAreaSystem>();
		
		// Note: Patches via HarmonyPatchAttribute are applied automatically unless disabled via ModBuilderSettings
	}

	public void Shutdown()
	{
	}

	public void ModObjectLoaded(Object obj)
	{
	}

	public void Update()
	{
	}
}
```

```csharp
using HarmonyLib;
using Unity.Collections;
using Unity.Entities;

/// <summary>
/// Disables spawning of a bunch of environment objects, like grass and plants (outside scenes, dungeons, and sub-biomes).
/// </summary>
[HarmonyPatch(typeof(SpawnEnvironmentObjectsInNewAreaSystem), "OnUpdate")]
public static class DisableEnvironmentSpawnPatch
{
	// The Prefix runs right before the original function (return controls whether to continue with original function)
	[HarmonyPrefix]
	public static bool Prefix(ref SystemState state)
	{
		var ecb = new EntityCommandBuffer(Allocator.Temp);
		state.Dependency = default;
		
		using var environmentSpawnQuery = state.EntityManager.CreateEntityQuery(ComponentType.ReadOnly<SpawnEnvironmentObjectsInAreaCD>());

		// This is the minimal setup required to pass on the new area to the next step in the world gen pipeline
		ecb.RemoveComponent<SpawnEnvironmentObjectsInAreaCD>(environmentSpawnQuery, EntityQueryCaptureMode.AtRecord);
		ecb.AddComponent<SpawnTerritoriesInAreaCD>(environmentSpawnQuery, EntityQueryCaptureMode.AtRecord);
		
		ecb.Playback(state.EntityManager);
		ecb.Dispose();
		
		// Return false to not call the original method
		return false; 
	}
}
```
