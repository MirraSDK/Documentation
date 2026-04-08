---
title: Ad Monetization
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/ads
---

# Ad Monetization

Check if ads are available in the environment where the game is running:

```csharp
bool isAdsAvailable = MirraSDK.Ads.IsAvailable;
```

## Banner Ads

Check if a banner ad is ready to be shown:

```csharp
bool isBannerReady = MirraSDK.Ads.IsBannerReady;
```

Check if a banner ad is currently being shown to the player:

```csharp
bool isBannerVisible = MirraSDK.Ads.IsBannerVisible;
```

Check if banner ads are available in the environment where the game is running:

```csharp
bool isBannerAvailable = MirraSDK.Ads.IsBannerAvailable;
```

Show a banner ad:

```csharp
MirraSDK.Ads.InvokeBanner();
```

Refresh the banner ad content:

```csharp
MirraSDK.Ads.RefreshBanner();
```

Hide the banner ad:

```csharp
MirraSDK.Ads.DisableBanner();
```

## Interstitial Ads

Check if an interstitial ad is ready to be shown:

```csharp
bool isInterstitialReady = MirraSDK.Ads.IsInterstitialReady;
```

Check if an interstitial ad is currently being shown to the player:

```csharp
bool isInterstitialVisible = MirraSDK.Ads.IsInterstitialVisible;
```

Check if interstitial ads are available in the environment where the game is running:

```csharp
bool isInterstitialAvailable = MirraSDK.Ads.IsInterstitialAvailable;
```

Get the exact time of the last interstitial ad closure.\
Returns `null` if the interstitial ad has never been `closed` during the game session:

```csharp
DateTime? lastInterstitialSuccess = MirraSDK.Ads.GetLastInterstitialSuccess();
if (lastInterstitialSuccess.HasValue) {
    Debug.Log($"The last time an interstitial ad was closed '{lastInterstitialSuccess.Value}'");
    TimeSpan timeSinceSuccess = DateTime.Now - lastInterstitialSuccess.Value;
    Debug.Log($"{timeSinceSuccess.TotalSeconds} seconds have passed since the last interstitial ad closure");
} else {
    Debug.Log("An interstitial ad has never been closed during this game session");
}
```

Show an interstitial ad to the player:

```csharp
MirraSDK.Ads.InvokeInterstitial(
    onOpen: () => Debug.Log("Interstitial ad opened"),
    onClose: (isSuccess) => Debug.Log("Interstitial ad closed")
);
```

## Rewarded Ads

Check if a rewarded ad is ready to be shown:

```csharp
bool isRewardedReady = MirraSDK.Ads.IsRewardedReady;
```

Check if a rewarded ad is currently being shown to the player:

```csharp
bool isRewardedVisible = MirraSDK.Ads.IsRewardedVisible;
```

Check if rewarded ads are available in the environment where the game is running:

```csharp
bool isRewardedAvailable = MirraSDK.Ads.IsRewardedAvailable;
```

Get the exact time of the last rewarded ad closure.\
Can be used without a tag to get the time of the last closure of any rewarded ad, or with a tag to get the closure time of a specific rewarded ad.\
Returns `null` if the rewarded ad has never been `closed` during the game session:

```csharp
DateTime? lastRewardedSuccess = MirraSDK.Ads.GetLastRewardedSuccess("extra_lives");
if (lastRewardedSuccess.HasValue) {
    Debug.Log($"The last time 'extra_lives' was closed '{lastRewardedSuccess.Value}'");
    TimeSpan = DateTime.Now - lastRewardedSuccess.Value;
    Debug.Log($"{TimeSpan.TotalSeconds} seconds have passed since the last closure of 'extra_lives'");
} else {
    Debug.Log("'extra_lives' has never been closed during this game session");
}
```

Show a rewarded ad:

```csharp
MirraSDK.Ads.InvokeRewarded(
    onSuccess: () => Debug.Log("Rewarded ad successfully shown"),
    onOpen: () => Debug.Log("Rewarded ad opened"),
    onClose: (isSuccess) => Debug.Log($"Rewarded ad closed with reward '{isSuccess}'"),
    rewardTag: "extra_lives" // Tag for tracking rewarded ad closure
);
```
