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

		// Arm any worlds that already exist, so the registration is not missed — see below
		foreach (var world in World.All)
		{
			BurstDisabler.AddWorld(world);
		}
		
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

## Dedicated Servers

Without the `AddWorld` pass in `Init()` above, your patch still binds but its prefix never runs — no error, no log line. In practice that means the mod works when a player hosts the game and quietly does nothing on a dedicated server; the split is measured rather than guaranteed, which is the reason to write the pass unconditionally instead of branching on the build. `SpawnEnvironmentObjectsInNewAreaSystem` runs only in the ServerSimulation world, which lives inside the hosting player's own process and in the dedicated server process, so there is no client-world copy of it doing the same work.

For an `ISystem` like this one, `DisableBurstForSystem<T>()` takes effect per system type while the Burst bypass is keyed per world: `BurstDisabler.AddWorld(world)` is what resolves a registered type into the system handle for a given world. A managed `SystemBase` takes a different path that never reaches that registry, so the per-world arming described here does not apply to it. The game does that pass itself while ECS starts up, after the worlds exist, and it can only arm what has been registered by then. What differs between the builds is whether your registration is in place by then, and that is the part the API leaves open. Measured on both: when a player hosts, `Init()` has run before the worlds are created, so the game's own pass picks the registration up; on a dedicated server the worlds are built first and that pass finds nothing.

`AddWorld` only sees what was registered before it runs, so register every system you need first and do the pass once afterwards. Writing it unconditionally is safe: where the worlds do not exist yet at `Init()` time, the pass finds nothing to arm and the game's own startup handles it.

{% hint style="warning" %}
Neither half of this belongs in `EarlyInit()`, and they fail there for different reasons. `DisableBurstForSystem<T>()` cannot run that early — the type initialization it depends on has not happened yet, which is what the comment in the example above refers to. Moving only the `World.All` pass there is quieter and no better: the registration set it arms from is still empty that early, since `DisableBurstForSystem<T>()` fills it from `Init()`, so the pass arms nothing at all.
{% endhint %}
