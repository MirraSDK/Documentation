---
title: Player
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/player
---

# Player

Get the player's full name (guaranteed field on all platforms):

```csharp
string displayName = MirraSDK.Player.DisplayName;
```

Get the player's first name:

```csharp
string firstName = MirraSDK.Player.FirstName;
```

Get the player's last name:

```csharp
string lastName = MirraSDK.Player.LastName;
```

Get the player's tag:

```csharp
string username = MirraSDK.Player.Username;
```

Get the player's unique identifier:

```csharp
string uniqueId = MirraSDK.Player.UniqueId;
```

Check if the player is authorized:

```csharp
bool isLoggedIn = MirraSDK.Player.IsLoggedIn;
```

Invoke player authorization:

```csharp
MirraSDK.Player.InvokeLogin(
    () => Debug.Log("authorization successful"),
    () => Debug.LogError("authorization error")
);
```
