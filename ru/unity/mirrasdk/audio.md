---
title: Настройки звука
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/audio
---

# Настройки звука

При интеграции MirraSDK в вашу игру необходимо заменить все вызовы `AudioListener.volume` на `MirraSDK.Audio.Volume`, а `AudioListener.pause` на `MirraSDK.Audio.Pause`. Эта замена необходима, чтобы избежать конфликтов, поскольку MirraSDK управляет этими полями для контроля функций паузы и возобновления игры.

Используя свойства, предоставляемые MirraSDK, вместо свойств непосредственно из Unity, вы решаете проблему, когда функциональность паузы в SDK может работать некорректно. Кроме того, такой подход позволяет кэшировать промежуточные значения свойств, например, продолжать режим замедленной съемки в игре после паузы.

## Громкость аудио

Получение текущей громкости аудио:

```csharp
float currentVolume = MirraSDK.Audio.Volume;
```

Установка громкости аудио:

```csharp
MirraSDK.Audio.Volume = Mathf.Clamp01(newVolume);
```

## Пауза аудио

Получение текущего состояния паузы аудио:

```csharp
bool isAudioPaused = MirraSDK.Audio.Pause;
```

Установка состояния паузы аудио:

```csharp
MirraSDK.Audio.Pause = true;
```
