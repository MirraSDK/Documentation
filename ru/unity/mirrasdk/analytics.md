---
title: Сбор аналитики
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/analytics
---

# Сбор аналитики

## Отправка игровых событий

Проверить есть ли отправка игровых событий в окружении где открыта игра:

```csharp
bool isAvailable = MirraSDK.Analytics.IsGameplayReporterAvailable;
```

### Событие Game Ready

Метод Game Ready нужно вызывать ровно в момент, когда завершена загрузка всех прогресс-баров, логотипов движка и других стартовых экранов. Проект должен быть готов к взаимодействию. Если на экране отображается текст сюжета или обучение, которые, например, не пролистываются, то Game Ready нужно активировать после их завершения. Когда игрок сможет нажимать на интерактивные элементы.

```csharp
MirraSDK.Analytics.GameIsReady();
```

### Событие начала игрового процесса

Отправить событие о том, что геймплей перешел в активное состояние (игрок непосредственно играет в игру, не находится в главном меню или игра на паузе):

```csharp
MirraSDK.Analytics.GameplayStart(int level = 0);
```

Отправить событие о том, что геймплей перезапущен (игрок начал игру заново, например, после проигрыша или перезапуска уровня):

```csharp
MirraSDK.Analytics.GameplayRestart(int level = 0);
```

Отправить событие о том, что геймплей перешел в пассивное состояние (игрок фактически не играет в игру и находится в главном меню или игра на паузе):

```csharp
MirraSDK.Analytics.GameplayStop(int level = 0);
```

## Отправка событий в аналитику

Проверить есть ли аналитика в окружении где открыта игра:

```csharp
bool isAvailable = MirraSDK.Analytics.IsEventsReporterAvailable;
```

Отправить простое событие по имени, eventName это имя события:

```csharp
MirraSDK.Analytics.Report("event");
```

Отправить событие по имени, где eventName это имя события, а value это значение события:

```csharp
MirraSDK.Analytics.Report("event", "value");
```

Отправить событие по имени и множеством параметров, где eventName это имя события в виде string, а словарь eventParameters имеет структуру Dictionary\<string, object>.

```csharp
MirraSDK.Analytics.Report(
    eventName: "tutorial", 
    eventParameters: new() {
        ["timeToComplete"] = 100,
        ["doneCompletely"] = true
    }
);
```
