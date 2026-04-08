---
title: Time
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/time
---

# Time

It is mandatory to replace all calls to Time.timeScale with MirraSDK.Time.Scale. This is necessary to avoid conflicts caused by multiple systems using Time.timeScale simultaneously, because MirraSDK controls Time.timeScale to enable/disable the game's pause functionality.

Instead of using Time.timeScale, you should use:

```csharp
// get the time scale
float timeScale = MirraSDK.Time.Scale;

// set the time scale value
MirraSDK.Time.Scale = 1.0f;
```

Get the current time as a DateTime:

```csharp
DateTime currentDate = MirraSDK.Time.CurrentDate;
```

Get the current holiday:

```csharp
HolidayType holiday = MirraSDK.Time.CurrentHoliday;
```
