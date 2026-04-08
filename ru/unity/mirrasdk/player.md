---
title: Игрок
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/player
---

# Игрок

Получить полное имя игрока (гарантированное поле на всех площадках):

```csharp
string displayName = MirraSDK.Player.DisplayName;
```

Получить имя игрока:

```csharp
string firstName = MirraSDK.Player.FirstName;
```

Получить фамилию игрока:

```csharp
string lastName = MirraSDK.Player.LastName;
```

Получить тег игрока:

```csharp
string username = MirraSDK.Player.Username;
```

Получить уникальный идентификатор игрока:

```csharp
string uniqueId = MirraSDK.Player.UniqueId;
```

Проверить, авторизован ли игрок:

```csharp
bool isLoggedIn = MirraSDK.Player.IsLoggedIn;
```

Вызвать авторизацию игрока:

```csharp
MirraSDK.Player.InvokeLogin(
    () => Debug.Log("авторизация успешна"),
    () => Debug.LogError("ошибка авторизации")
);
```