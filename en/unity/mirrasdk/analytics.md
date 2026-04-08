---
title: Analytics
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/analytics
---

# Analytics

## Sending Gameplay Events

Check if gameplay event reporting is available in the environment where the game is running:

```csharp
bool isAvailable = MirraSDK.Analytics.IsGameplayReporterAvailable;
```

### Game Ready Event

The Game Ready method should be called at the exact moment when all progress bars, engine logos, and other startup screens have finished loading. The project must be ready for interaction. If the screen displays story text or a tutorial that cannot be skipped, Game Ready should be triggered after they are completed. When the player can interact with clickable elements.

```csharp
MirraSDK.Analytics.GameIsReady();
```

### Gameplay Start Event

Send an event indicating that gameplay has entered an active state (the player is actively playing the game, not in the main menu or paused):

```csharp
MirraSDK.Analytics.GameplayStart(int level = 0);
```

Send an event indicating that gameplay has been restarted (the player started the game over, for example, after losing or restarting a level):

```csharp
MirraSDK.Analytics.GameplayRestart(int level = 0);
```

Send an event indicating that gameplay has entered a passive state (the player is not actively playing and is in the main menu or the game is paused):

```csharp
MirraSDK.Analytics.GameplayStop(int level = 0);
```

## Sending Analytics Events

Check if analytics is available in the environment where the game is running:

```csharp
bool isAvailable = MirraSDK.Analytics.IsEventsReporterAvailable;
```

Send a simple event by name, where eventName is the event name:

```csharp
MirraSDK.Analytics.Report("event");
```

Send an event by name, where eventName is the event name and value is the event value:

```csharp
MirraSDK.Analytics.Report("event", "value");
```

Send an event by name with multiple parameters, where eventName is the event name as a string, and the eventParameters dictionary has the structure Dictionary\<string, object>.

```csharp
MirraSDK.Analytics.Report(
    eventName: "tutorial", 
    eventParameters: new() {
        ["timeToComplete"] = 100,
        ["doneCompletely"] = true
    }
);
```
