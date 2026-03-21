---
description: >-
  This example shows how to replace a Burst direct-call function pointer at
  runtime without disabling Burst for the calling system.
---

# Burst Direct-Call Hook Example

This is useful when:

* The code you want to affect runs from Burst compiled systems or jobs.
* Disabling Burst is too broad or too expensive.

This example changes the nearby chest range used for crafting by replacing the direct-call pointer for `InventoryUtility.GetNearbyChestsForCraftingByDistance(...)`.

## Helper

```csharp
using System;
using System.Linq;
using System.Reflection;
using System.Runtime.InteropServices;

public sealed class BurstDirectCallPointerHook
{
    private readonly Type _ownerType;
    private readonly string _methodName;
    private readonly Delegate _detourDelegate;
    private readonly IntPtr _detourPointer;

    private Type _burstDirectCallType;
    private FieldInfo _pointerField;
    private MethodInfo _getFunctionPointerMethod;
    private IntPtr _originalPointer;
    private bool _originalPointerCaptured;

    public BurstDirectCallPointerHook(Type ownerType, string methodName, Delegate detourDelegate)
    {
        _ownerType = ownerType ?? throw new ArgumentNullException(nameof(ownerType));
        _methodName = !string.IsNullOrWhiteSpace(methodName) ? methodName : throw new ArgumentException("method name is required", nameof(methodName));
        _detourDelegate = detourDelegate ?? throw new ArgumentNullException(nameof(detourDelegate));
        _detourPointer = Marshal.GetFunctionPointerForDelegate(_detourDelegate);
    }

    public bool TryEnable(out string error)
    {
        if (!TryResolve(out error))
        {
            return false;
        }

        if (!_originalPointerCaptured)
        {
            try
            {
                _originalPointer = (IntPtr)_getFunctionPointerMethod.Invoke(null, null);
                _originalPointerCaptured = true;
            }
            catch (Exception ex)
            {
                error = $"Failed to capture original Burst pointer: {ex.Message}";
                return false;
            }
        }

        _pointerField.SetValue(null, _detourPointer);
        error = null;
        return true;
    }

    public bool TryDisable(out string error)
    {
        if (!TryResolve(out error))
        {
            return false;
        }

        _pointerField.SetValue(null, _originalPointerCaptured ? _originalPointer : IntPtr.Zero);
        error = null;
        return true;
    }

    private bool TryResolve(out string error)
    {
        error = null;

        if (_pointerField != null && _getFunctionPointerMethod != null)
        {
            return true;
        }

        _burstDirectCallType = _ownerType
            .GetNestedTypes(BindingFlags.NonPublic | BindingFlags.Public)
            .FirstOrDefault(type =>
                type.Name.IndexOf(_methodName, StringComparison.Ordinal) >= 0 &&
                type.Name.IndexOf("BurstDirectCall", StringComparison.Ordinal) >= 0);

        if (_burstDirectCallType == null)
        {
            error = $"Failed to find BurstDirectCall type for {_ownerType.FullName}.{_methodName}";
            return false;
        }

        _pointerField = _burstDirectCallType.GetField("Pointer", BindingFlags.Static | BindingFlags.NonPublic);
        _getFunctionPointerMethod = _burstDirectCallType.GetMethod("GetFunctionPointer", BindingFlags.Static | BindingFlags.NonPublic);

        if (_pointerField == null || _getFunctionPointerMethod == null)
        {
            error = $"Failed to resolve BurstDirectCall members on {_burstDirectCallType.FullName}";
            return false;
        }

        return true;
    }
}
```

## Example

```csharp
using System;
using System.Runtime.InteropServices;
using Inventory;
using PugMod;
using Unity.Collections;
using Unity.Entities;
using Unity.Mathematics;
using Unity.Physics;
using Unity.Transforms;
using Object = UnityEngine.Object;

public class ExtendedCraftingChestRange : IMod
{
    private const float ExtendedRange = 25f;
    private const int MaxInventories = 20;

    [UnmanagedFunctionPointer(CallingConvention.Cdecl)]
    private delegate void CraftingRangeDirectCallDelegate(
        ref float3 position,
        ref CollisionWorld collisionWorld,
        ref ComponentLookup<InventoryAutoTransferEnabledCD> inventoryAutoTransferEnabledLookup,
        ref ComponentLookup<LocalTransform> localTransformLookup,
        ref NativeList<Entity> inventories);

    private static readonly CraftingRangeDirectCallDelegate DetourDelegate = DetourInvoke;
    private static readonly BurstDirectCallPointerHook Hook = new BurstDirectCallPointerHook(
        typeof(InventoryUtility),
        nameof(InventoryUtility.GetNearbyChestsForCraftingByDistance),
        DetourDelegate);

    public void EarlyInit()
    {
    }

    public void Init()
    {
        Hook.TryEnable(out _);
    }

    public void Shutdown()
    {
        Hook.TryDisable(out _);
    }

    public void ModObjectLoaded(Object obj)
    {
    }

    public void Update()
    {
    }

    private static void DetourInvoke(
        ref float3 position,
        ref CollisionWorld collisionWorld,
        ref ComponentLookup<InventoryAutoTransferEnabledCD> inventoryAutoTransferEnabledLookup,
        ref ComponentLookup<LocalTransform> localTransformLookup,
        ref NativeList<Entity> inventories)
    {
        InventoryUtility.GetNearbyChestsByDistance(
            in position,
            in collisionWorld,
            in inventoryAutoTransferEnabledLookup,
            in localTransformLookup,
            ref inventories,
            ExtendedRange,
            MaxInventories);
    }
}
```

## Why this works

Burst compiled methods that use direct-call generate a nested helper type which caches a native function pointer in a private static `Pointer` field.

When the game calls the managed wrapper method, that wrapper usually:

* checks if Burst is enabled
* gets the cached pointer
* calls the pointer directly if it exists
* falls back to a managed implementation otherwise

This example replaces the cached pointer with a pointer to our own cdecl delegate thunk. After that, callers still go through the same Burst direct-call path, but they land in our code instead of the original function.

If you decompile Pug.Other.dll and look at the InventoryUtility class, you can spot the exact structures we are using on our hooks:

```csharp
    [UnmanagedFunctionPointer(CallingConvention.Cdecl)]
	internal delegate void GetNearbyChestsByDistance_00007320_0024PostfixBurstDelegate(in float3 position, in CollisionWorld collisionWorld, in ComponentLookup<InventoryAutoTransferEnabledCD> inventoryAutoTransferEnabledLookup, in ComponentLookup<LocalTransform> localTransformLookup, ref NativeList<Entity> inventories, float maxDistance, int maxInventories);

	internal static class GetNearbyChestsByDistance_00007320_0024BurstDirectCall
	{
        // This is the pointer we are replacing
		private static IntPtr Pointer;

        [BurstDiscard]
		private static void GetFunctionPointerDiscard(ref IntPtr P_0)
		{
			if (Pointer == (IntPtr)0)
			{
				Pointer = BurstCompiler.CompileFunctionPointer<GetNearbyChestsByDistance_00007320_0024PostfixBurstDelegate>(GetNearbyChestsByDistance).Value;
			}
			P_0 = Pointer;
		}

		private static IntPtr GetFunctionPointer()
		{
			nint result = 0;
			GetFunctionPointerDiscard(ref result);
			return result;
		}

		public unsafe static void Invoke(in float3 position, in CollisionWorld collisionWorld, in ComponentLookup<InventoryAutoTransferEnabledCD> inventoryAutoTransferEnabledLookup, in ComponentLookup<LocalTransform> localTransformLookup, ref NativeList<Entity> inventories, float maxDistance, int maxInventories)
		{
			if (BurstCompiler.IsEnabled)
			{
				IntPtr functionPointer = GetFunctionPointer();
				if (functionPointer != (IntPtr)0)
				{
					((delegate* unmanaged[Cdecl]<ref float3, ref CollisionWorld, ref ComponentLookup<InventoryAutoTransferEnabledCD>, ref ComponentLookup<LocalTransform>, ref NativeList<Entity>, float, int, void>)functionPointer)(ref position, ref collisionWorld, ref inventoryAutoTransferEnabledLookup, ref localTransformLookup, ref inventories, maxDistance, maxInventories);
					return;
				}
			}
			GetNearbyChestsByDistance_0024BurstManaged(in position, in collisionWorld, in inventoryAutoTransferEnabledLookup, in localTransformLookup, ref inventories, maxDistance, maxInventories);
		}
	}
```

Noticed how the Cdecl calling convetion is defined on the generated delegate, we match that in our code. We use reflection to look for the `GetNearbyChestsByDistance_00007320_0024BurstDirectCall` class by looking for `GetNearbyChestsByDistance` and `BurstDirectCall` on the same type. 

To perform the hook, we switch Pointer to our own function and store a reference to the original one for reverting if needed.

## Drawbacks

* This depends on generated private internals. If Unity or Burst changes the direct-call codegen, this can break.
* The delegate signature must match the original unmanaged signature exactly.
* This only works for methods that actually use this direct-call pattern.
* This is harder to maintain than a normal Harmony patch.
* This is less portable across runtime changes than patching managed code.
* It requires reflection, so your mod will need elevated permissions.
* It won't produce a pretty exception when it fails, you're looking at a segfault in that case.
* It may collide with other mods if they disable burst for the methods you are hooking.

