---
title: Устройство
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/device
---

# Устройство

## Информация об устройстве

Проверить мобильное ли устройство:

```csharp
bool isMobile = MirraSDK.Device.IsMobile;
```

Получить тип операционной системы:

```csharp
SystemType systemType = MirraSDK.Device.SystemType;
```

## Настройка курсора

Показать или спрятать курсор:

```csharp
// показать курсор
MirraSDK.Device.CursorVisible = true;

// спрятать курсор
MirraSDK.Device.CursorVisible = false;
```

Заблокировать или разблокировать курсор:

```csharp
// заблокировать курсор
MirraSDK.Device.CursorLock = CursorLockMode.Locked;

// разблокировать курсор
MirraSDK.Device.CursorLock = CursorLockMode.None;
```

## Браузер устройства

Открыть страницу в браузере:

```csharp
MirraSDK.Device.OpenURL("https://example.com");
```
