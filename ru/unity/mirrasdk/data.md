---
title: Сохранение прогресса
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/data
---

# Сохранение прогресса

У всех Get-методов есть необязательный параметр `defaultValue`, который используется, если значение по ключу не найдено. Например, `MirraSDK.Data.GetInt("key", 100)` вернет `100`, если ключ не найден.

У всех Set-методов есть необязательный параметр `important`, который по умолчанию `true`. Если указать `important` как `false`, то метод не вызовет автосохранение прогресса.

Получить bool-значение:

```csharp
bool value = MirraSDK.Data.GetBool("key");
```

Сохранить bool-значение:

```csharp
MirraSDK.Data.SetBool("key", true);
```

Получить int-значение:

```csharp
int value = MirraSDK.Data.GetInt("key");
```

Сохранить int-значение:

```csharp
MirraSDK.Data.SetInt("key", 512);
```

Получить float-значение:

```csharp
float value = MirraSDK.Data.GetFloat("key");
```

Сохранить float-значение:

```csharp
MirraSDK.Data.SetFloat("key", 3.14f);
```

Получить string-значение:

```csharp
MirraSDK.Data.GetString("key");
```

Сохранить string-значение:

```csharp
MirraSDK.Data.SetString("key", "value");
```

Получить Serializable-объект:

```csharp
Vector3 value = MirraSDK.Data.GetObject<Vector3>("key");
```

Сохранить Serializable-объект:

```csharp
MirraSDK.Data.SetObject<Vector3>("key", Vector3.one);
```

Запросить сохранение данных:

```csharp
MirraSDK.Data.Save();
```

Проверить на наличие данных по ключу:

```csharp
bool valueExists = MirraSDK.Data.HasKey("key");
```

Удалить ключ и его значение:

```csharp
MirraSDK.Data.DeleteKey("key");
```

Удалить все сохранения:

```csharp
MirraSDK.Data.DeleteAll();
```
