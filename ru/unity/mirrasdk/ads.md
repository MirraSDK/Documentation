---
title: Рекламная монетизация
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/ads
---

# Рекламная монетизация

Проверить есть ли реклама в окружении где открыта игра:

```csharp
bool isAdsAvailable = MirraSDK.Ads.IsAvailable;
```

## Баннерная реклама

Проверить что баннерная реклама готова к показу:

```csharp
bool isBannerReady = MirraSDK.Ads.IsBannerReady;
```

Проверить, что баннерная реклама показывается игроку прямо сейчас:

```csharp
bool isBannerVisible = MirraSDK.Ads.IsBannerVisible;
```

Проверить, доступна ли баннерная реклама в окружении, где открыта игра:

```csharp
bool isBannerAvailable = MirraSDK.Ads.IsBannerAvailable;
```

Показать баннерную рекламу:

```csharp
MirraSDK.Ads.InvokeBanner();
```

Обновить содержимое баннерной рекламы:

```csharp
MirraSDK.Ads.RefreshBanner();
```

Спрятать баннерную рекламу:

```csharp
MirraSDK.Ads.DisableBanner();
```

## Межстраничная реклама

Проверить, что межстраничная реклама готова к показу:

```csharp
bool isInterstitialReady = MirraSDK.Ads.IsInterstitialReady;
```

Проверить, что межстраничная реклама показывается игроку прямо сейчас:

```csharp
bool isInterstitialVisible = MirraSDK.Ads.IsInterstitialVisible;
```

Проверить, доступна ли межстраничная реклама в окружении, где открыта игра:

```csharp
bool isInterstitialAvailable = MirraSDK.Ads.IsInterstitialAvailable;
```

Узнать точное время последнего закрытия межстраничной рекламы.\
Вернет `null` если межстраничная реклама ни разу не была `закрыта` за игровую сессию:

```csharp
DateTime? lastInterstitialSuccess = MirraSDK.Ads.GetLastInterstitialSuccess();
if (lastInterstitialSuccess.HasValue) {
    Debug.Log($"Последний раз межстраничная реклама была закрыта '{lastInterstitialSuccess.Value}'");
    TimeSpan timeSinceSuccess = DateTime.Now - lastInterstitialSuccess.Value;
    Debug.Log($"Прошло {timeSinceSuccess.TotalSeconds} секунд с момента последнего закрытия межстраничной рекламы");
} else {
    Debug.Log("Межстраничная реклама никогда не была закрыта за игровую сессию");
}
```

Показать межстраничную рекламу игроку:

```csharp
MirraSDK.Ads.InvokeInterstitial(
    onOpen: () => Debug.Log("Межстраничная реклама открыта"),
    onClose: (isSuccess) => Debug.Log("Межстраничная реклама закрыта")
);
```

## Реклама за вознаграждение

Проверить, что реклама за вознаграждение готова к показу:

```csharp
bool isRewardedReady = MirraSDK.Ads.IsRewardedReady;
```

Проверить, что реклама за вознаграждение показывается игроку прямо сейчас:

```csharp
bool isRewardedVisible = MirraSDK.Ads.IsRewardedVisible;
```

Проверить, доступна ли реклама за вознаграждение в окружении, где открыта игра:

```csharp
bool isRewardedAvailable = MirraSDK.Ads.IsRewardedAvailable;
```

Узнать точное время последнего закрытия рекламы за вознаграждение.\
Можно использовать без тега, чтобы узнать время последнего закрытия любой рекламы за вознаграждение, или с тегом, чтобы узнать время закрытия конкретной рекламы за вознаграждение.\
Вернет `null` если межстраничная реклама ни разу не была `закрыта` за игровую сессию:

```csharp
DateTime? lastRewardedSuccess = MirraSDK.Ads.GetLastRewardedSuccess("extra_lives");
if (lastRewardedSuccess.HasValue) {
    Debug.Log($"Последний раз 'extra_lives' был закрыт '{lastRewardedSuccess.Value}'");
    TimeSpan = DateTime.Now - lastRewardedSuccess.Value;
    Debug.Log($"Прошло {TimeSpan.TotalSeconds} секунд с момента последнего закрытия 'extra_lives'");
} else {
    Debug.Log("'extra_lives' никогда не был закрыт за игровую сессию");
}
```

Показать рекламу за вознаграждение:

```csharp
MirraSDK.Ads.InvokeRewarded(
    onSuccess: () => Debug.Log("Реклама за вознаграждение успешно показана"),
    onOpen: () => Debug.Log("Реклама за вознаграждение открыта"),
    onClose: (isSuccess) => Debug.Log($"Реклама за вознаграждение закрыта с наградой '{isSuccess}'"),
    rewardTag: "extra_lives" // Тег для отслеживания закрытия рекламы за вознаграждение
);
```