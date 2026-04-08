---
title: Loading Files
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/assets
---

# Loading Files

## Addressables

Load an Addressable by key:

```csharp
MirraSDK.Assets.LoadAddressable<GameObject>("addressablePath", (addressable) => {
    Debug.Log($"Addressable loaded: '{addressable}'");
}, () => {
    Debug.LogError("Failed to load Addressable.");
});
```

Unload an Addressable by key:

```csharp
MirraSDK.Remote.ReleaseAddressable("addressablePath");
```

## AssetBundles

Load an AssetBundle by key and path:

```csharp
MirraSDK.Assets.LoadBundle("bundleTag", "https://example.com/bundle", (bundle) => {
    Debug.Log($"AssetBundle loaded: '{bundle}'");
}, () => {
    Debug.LogError("Failed to load AssetBundle.");
});
```

Unload an AssetBundle and all its objects by key:

```csharp
MirraSDK.Assets.ReleaseBundle("bundleTag", unloadAllObjects: true);
```

## StreamingAssets

Load an audio file from the StreamingAssets folder:

```csharp
MirraSDK.Assets.LoadStreamingAudioClip("example/audio.ogg", AudioType.MPEG, (audioClip) => {
    Debug.Log($"Audio file loaded: '{audioClip}'");
}, () => {
    Debug.LogError("Failed to load audio file.");
});
```

Load a text file from the StreamingAssets folder:

```csharp
MirraSDK.Assets.LoadStreamingText("example/text.txt", (text) => {
    Debug.Log($"Text file loaded: '{text}'");
}, () => {
    Debug.LogError("Failed to load text file.");
});
```

Load a texture from the StreamingAssets folder:

```csharp
MirraSDK.Assets.LoadStreamingTexture2D("example/texture.tga", (texture) => {
    Debug.Log($"Texture loaded: '{texture}'");
}, () => {
    Debug.LogError("Failed to load texture.");
});
```

Load a serialized JSON object from the StreamingAssets folder:

```csharp
[System.Serializable]
public class Example {
    public string name;
    public int value;
}

MirraSDK.Assets.LoadStreamingJSON<LevelData>("example/object.json", (example) => {
    Debug.Log($"JSON object loaded: '{example}'");
}, () => {
    Debug.LogError("Failed to load JSON object.");
});
```
