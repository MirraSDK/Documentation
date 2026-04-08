---
title: Initialization
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/initialization
---

# Initialization

You can only access MirraSDK after ensuring that it is initialized and ready to use. Before that, any access to its interfaces will result in an exception, a critical error, or a crash. You can safely call the methods and properties below to check MirraSDK's readiness status.

The IsInitialized property indicates MirraSDK's readiness and returns the readiness state of all configuration components that require asynchronous loading:

```csharp
bool isSDKInitialized = MirraSDK.IsInitialized;
```

Example 1. Using a coroutine:

```csharp
public IEnumerator WaitForMirraSDK() {
    yield return WaitUntil(() => MirraSDK.IsInitialized);
    // MirraSDK is initialized.
}
```

Example 2. Using a callback.

```csharp
MirraSDK.WaitForProviders(() => {
    // MirraSDK is initialized.
});
```
