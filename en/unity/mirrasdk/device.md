---
title: Device
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/device
---

# Device

## Device Information

Check if the device is mobile:

```csharp
bool isMobile = MirraSDK.Device.IsMobile;
```

Get the operating system type:

```csharp
SystemType systemType = MirraSDK.Device.SystemType;
```

## Cursor Settings

Show or hide the cursor:

```csharp
// show cursor
MirraSDK.Device.CursorVisible = true;

// hide cursor
MirraSDK.Device.CursorVisible = false;
```

Lock or unlock the cursor:

```csharp
// lock cursor
MirraSDK.Device.CursorLock = CursorLockMode.Locked;

// unlock cursor
MirraSDK.Device.CursorLock = CursorLockMode.None;
```

## Device Browser

Open a page in the browser:

```csharp
MirraSDK.Device.OpenURL("https://example.com");
```
