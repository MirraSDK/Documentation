---
title: Platform
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/platform
---

# Platform

Get the platform type on which the game is running:

```csharp
PlatformType platform = MirraSDK.Platform.Current;
```

Get the environment type in which the game is running:

```csharp
DeploymentType deployment = MirraSDK.Platform.Deployment;
```

Get the unique game identifier on the platform:

```csharp
string appId = MirraSDK.Platform.AppId;
```

Share the game:

```csharp
MirraSDK.Platform.ShareGame("message text");
```

Rate the game:

```csharp
MirraSDK.Platform.RateGame();
```
