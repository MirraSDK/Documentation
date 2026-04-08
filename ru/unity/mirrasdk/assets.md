---
title: Подгрузка файлов
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/assets
---

# Подгрузка файлов

## Addressables

Загрузить Addressable по ключу:

```csharp
MirraSDK.Assets.LoadAddressable<GameObject>("addressablePath", (addressable) => {
    Debug.Log($"Addressable загружен: '{addressable}'");
}, () => {
    Debug.LogError("Не удалось загрузить Addressable.");
});
```

Выгрузить Addressable по ключу:

```csharp
MirraSDK.Remote.ReleaseAddressable("addressablePath");
```

## AssetBundles

Загрузить AssetBundle по ключу и пути:

```csharp
MirraSDK.Assets.LoadBundle("bundleTag", "https://example.com/bundle", (bundle) => {
    Debug.Log($"AssetBundle загружен: '{bundle}'");
}, () => {
    Debug.LogError("Не удалось загрузить AssetBundle.");
});
```

Выгрузить AssetBundle и все его объекты по ключу:

```csharp
MirraSDK.Assets.ReleaseBundle("bundleTag", unloadAllObjects: true);
```

## StreamingAssets

Загрузить аудио файл из папки StreamingAssets:

```csharp
MirraSDK.Assets.LoadStreamingAudioClip("example/audio.ogg", AudioType.MPEG, (audioClip) => {
    Debug.Log($"Аудио файл загружен: '{audioClip}'");
}, () => {
    Debug.LogError("Не удалось загрузить аудио файл.");
});
```

Загрузить текстовый файл из папки StreamingAssets:

```csharp
MirraSDK.Assets.LoadStreamingText("example/text.txt", (text) => {
    Debug.Log($"Текстовый файл загружен: '{text}'");
}, () => {
    Debug.LogError("Не удалось загрузить текстовый файл.");
});
```

Загрузить текстуру из папки StreamingAssets:

```csharp
MirraSDK.Assets.LoadStreamingTexture2D("example/texture.tga", (texture) => {
    Debug.Log($"Текстура загружена: '{texture}'");
}, () => {
    Debug.LogError("Не удалось загрузить текстуру.");
});
```

Загрузить серилизованный JSON объект из папки StreamingAssets:

```csharp
[System.Serializable]
public class Example {
    public string name;
    public int value;
}

MirraSDK.Assets.LoadStreamingJSON<LevelData>("example/object.json", (example) => {
    Debug.Log($"JSON объект загружен: '{example}'");
}, () => {
    Debug.LogError("Не удалось загрузить JSON объект.");
});
```
