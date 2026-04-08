---
title: Experiments
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/flags
---

# Experiments

All Get methods have an optional `defaultValue` parameter that is used if the value for the key is not found. For example, `MirraSDK.Flags.GetInt("key", 100)` will return `100` if the key is not found.

Get a bool value:

```csharp
bool value = MirraSDK.Flags.GetBool("key");
```

Get an int value:

```csharp
int value = MirraSDK.Flags.GetInt("key");
```

Get a float value:

```csharp
float value = MirraSDK.Flags.GetFloat("key");
```

Get a string value:

```csharp
string value = MirraSDK.Flags.GetString("key");
```

Check if data exists for a key:

```csharp
bool valueExists = MirraSDK.Flags.HasKey("key");
```
