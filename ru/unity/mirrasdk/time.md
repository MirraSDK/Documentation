---
title: Время
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/time
---

# Время

Обязательно следует заменить все вызовы Time.timeScale на MirraSDK.Time.Scale. Делать это нужно для того, чтобы избежать конфликтных ситуаций использования Time.timeScale всеми подряд, потому что MirraSDK контролирует Time.timeScale для включения / отключения паузы в игре.

Вместо использования Time.timeScale необходимо использовать:

```csharp
// получить скорость времени
float timeScale = MirraSDK.Time.Scale;

// изменить значение скорости времени
MirraSDK.Time.Scale = 1.0f;
```

Получить текущее время в виде DateTime:

```csharp
DateTime currentDate = MirraSDK.Time.CurrentDate;
```

Получить текущий праздник:

```csharp
HolidayType holiday = MirraSDK.Time.CurrentHoliday;
```