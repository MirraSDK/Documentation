---
title: Площадка
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/platform
---

# Площадка

Получить тип платформы, на которой запущена игра:

```csharp
PlatformType platform = MirraSDK.Platform.Current;
```

Получить тип окружения, в котором запущена игра:

```csharp
DeploymentType deployment = MirraSDK.Platform.Deployment;
```

Получить уникальный идентификатор игры на платформе:

```csharp
string appId = MirraSDK.Platform.AppId;
```

Поделиться игрой:

```csharp
MirraSDK.Platform.ShareGame("message text");
```

Оценить игру:

```csharp
MirraSDK.Platform.RateGame();
```