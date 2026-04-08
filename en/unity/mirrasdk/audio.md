---
title: Audio Settings
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/audio
---

# Audio Settings

When integrating MirraSDK into your game, you must replace all calls to `AudioListener.volume` with `MirraSDK.Audio.Volume`, and `AudioListener.pause` with `MirraSDK.Audio.Pause`. This replacement is necessary to avoid conflicts, as MirraSDK manages these fields to control the game's pause and resume functionality.

By using the properties provided by MirraSDK instead of the properties directly from Unity, you solve the problem where the SDK's pause functionality may not work correctly. Additionally, this approach allows caching intermediate property values, for example, continuing slow-motion mode in the game after a pause.

## Audio Volume

Get the current audio volume:

```csharp
float currentVolume = MirraSDK.Audio.Volume;
```

Set the audio volume:

```csharp
MirraSDK.Audio.Volume = Mathf.Clamp01(newVolume);
```

## Audio Pause

Get the current audio pause state:

```csharp
bool isAudioPaused = MirraSDK.Audio.Pause;
```

Set the audio pause state:

```csharp
MirraSDK.Audio.Pause = true;
```
