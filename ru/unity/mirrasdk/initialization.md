---
title: Инициализация
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/initialization
---

# Инициализация

К MirraSDK можно обращаться только после того, как убедитесь, что он инициализирован и готов к работе. До этого, любое обращение к его интерфейсам приведет к исключению, критической ошибке или вылету. Безопасно можно обращаться к методам и свойствам ниже, чтобы узнать статус готовности MirraSDK.

Свойство IsInitialized отвечает за готовность MirraSDK и возвращает состояние готовности всех компонентов конфигурации, которым требуется асинхронная загрузка:

```csharp
bool isSDKInitialized = MirraSDK.IsInitialized;
```

Пример 1. Используя корутину:

```csharp
public IEnumerator WaitForMirraSDK() {
    yield return WaitUntil(() => MirraSDK.IsInitialized);
    // MirraSDK инициализирован.
}
```

Пример 2. Используя колбек.

```csharp
MirraSDK.WaitForProviders(() => {
    // MirraSDK инициализирован.
});
```