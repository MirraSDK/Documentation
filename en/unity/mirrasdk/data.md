---
title: Saving Progress
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/data
---

# Saving Progress

All Get methods have an optional `defaultValue` parameter that is used if the value for the key is not found. For example, `MirraSDK.Data.GetInt("key", 100)` will return `100` if the key is not found.

All Set methods have an optional `important` parameter that defaults to `true`. If you set `important` to `false`, the method will not trigger auto-saving of progress.

Get a bool value:

```csharp
bool value = MirraSDK.Data.GetBool("key");
```

Save a bool value:

```csharp
MirraSDK.Data.SetBool("key", true);
```

Get an int value:

```csharp
int value = MirraSDK.Data.GetInt("key");
```

Save an int value:

```csharp
MirraSDK.Data.SetInt("key", 512);
```

Get a float value:

```csharp
float value = MirraSDK.Data.GetFloat("key");
```

Save a float value:

```csharp
MirraSDK.Data.SetFloat("key", 3.14f);
```

Get a string value:

```csharp
MirraSDK.Data.GetString("key");
```

Save a string value:

```csharp
MirraSDK.Data.SetString("key", "value");
```

Get a Serializable object:

```csharp
Vector3 value = MirraSDK.Data.GetObject<Vector3>("key");
```

Save a Serializable object:

```csharp
MirraSDK.Data.SetObject<Vector3>("key", Vector3.one);
```

Request data saving:

```csharp
MirraSDK.Data.Save();
```

Check if data exists for a key:

```csharp
bool valueExists = MirraSDK.Data.HasKey("key");
```

Delete a key and its value:

```csharp
MirraSDK.Data.DeleteKey("key");
```

Delete all saved data:

```csharp
MirraSDK.Data.DeleteAll();
```
