---
title: Эксперименты
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/flags
---

# Эксперименты

У всех Get-методов есть необязательный параметр `defaultValue`, который используется, если значение по ключу не найдено. Например, `MirraSDK.Flags.GetInt("key", 100)` вернет `100`, если ключ не найден.

Получить bool-значение:

```csharp
bool value = MirraSDK.Flags.GetBool("key");
```

Получить int-значение:

```csharp
int value = MirraSDK.Flags.GetInt("key");
```

Получить float-значение:

```csharp
float value = MirraSDK.Flags.GetFloat("key");
```

Получить string-значение:

```csharp
string value = MirraSDK.Flags.GetString("key");
```

Проверить на наличие данных по ключу:

```csharp
bool valueExists = MirraSDK.Flags.HasKey("key");
```