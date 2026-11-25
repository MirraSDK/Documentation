---
title: RuStore
nav_order: 200
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/rustore
---

# RuStore

The `RuStore API for MirraSDK5` package runs a game on RuStore through the usual MirraSDK API: in-game purchases, reviews, player sign-in, and optionally in-app updates and remote config. Game code does not call RuStore directly.

Version 2.0.0 works with RuStore Pay 11.2.0, Review 10.5.2 and Core 10.5.1, installed from RuStore's package registry.

### Requirements

1. Unity 2022.3.38 or newer.
2. MirraSDK5 5.2.5 or newer.
3. External Dependency Manager for Unity (`com.google.external-dependency-manager`). It adds the RuStore Android libraries to the build.
4. `Player Settings > Android > Other Settings > Minimum API Level` 24 or higher.
5. `Application Entry Point` set to `Activity`. RuStore payments do not work with `GameActivity`.
6. A game registered in RuStore Console with the same package name as the build.

### Installation

1. Open `MirraSDK5 > Open Toolkit` and go to the package list.
2. Select `RuStore API for MirraSDK5` and press its install button. The Toolkit adds the `RuStore Nexus` registry to the project, installs `ru.rustore.core`, `ru.rustore.pay` and `ru.rustore.review`, then installs the package itself.
3. Optional modules have their own buttons on the same card: `Install RuStore In-app Updates` and `Install RuStore Remote Config`. Install only what the game uses.
4. In `Player Settings > Android > Publishing Settings` enable `Custom Main Gradle Template`, `Custom Gradle Properties Template` and `Custom Gradle Settings Template`, then run `Assets > External Dependency Manager > Android Resolver > Force Resolve`.

The card description lists every RuStore package with its installed version. When a newer package version is published, the same card shows an update button that also updates the RuStore packages.

### Setup

1. Switch the platform to Android and choose `RuStore` as the build configuration in the Toolkit.
2. Run `MirraSDK5 > RuStore > Fix setup`. It creates the custom Android manifest when the project has none, creates the RuStore Pay settings, adds the RuStore activity and settings entries to the manifest, and sets `Application Entry Point` to `Activity`.
3. In `Window > RuStoreSDK > Settings > PayClient` enter `Console Application Id` (the number in the address of the app page in RuStore Console, between `apps/` and `/versions`) and `Deeplink Scheme` (any name unique to the game, lowercase letters and digits).
4. In the Toolkit open the `RuStore` configuration and list the product ids in `RuStorePayments > ProductsJson`:

```json
{ "Values": ["coins100", "noads"] }
```

5. Build. `MirraSDK5 > RuStore > Validate setup` prints the same check without building.

### In-Game Purchases

Products are created in RuStore Console as consumable or non-consumable. Prices come from the store: `GetProductData` returns the price in whole currency units (`PriceFloat` keeps kopecks) and the currency code. A product id missing from `ProductsJson` or from the Console returns a price of 0.

```csharp
MirraSDK.Payments.Purchase(
    productTag: "coins100",
    onSuccess: () => GiveProduct("coins100"),
    onError: () => Debug.Log("Not purchased")
);
```

Grant the product inside `onSuccess` and nowhere else. MirraSDK records that grant in the save, which is how it later knows what the player has already received.

Call `RestorePurchases` on every launch and grant each pending product:

```csharp
MirraSDK.Payments.RestorePurchases((restoreData) => {
    foreach (string productTag in restoreData.PendingProducts) {
        restoreData.RestoreProduct(productTag, () => GiveProduct(productTag));
    }
});
```

A product is pending when RuStore lists more paid purchases of it than the save has granted:

1. The game closed after payment and before the grant: the purchase is returned once on the next launch.
2. The save was deleted (reinstall, new device): every purchase RuStore still lists is returned, consumables included.
3. Refunded and unfinished payments are not counted.

`IsAlreadyPurchased` is true when the player has at least one paid purchase of the product. `MirraSDK.Payments.IsAvailable` is false when RuStore refuses payments for this game or player, for example when monetization is not enabled in RuStore Console.

A player who is not signed in to RuStore still sees prices; the purchase list stays empty until RuStore knows who the player is. Game startup never waits for RuStore longer than 10 seconds.

Subscriptions are not supported.

### Reviews

```csharp
MirraSDK.Platform.RateGame();
```

Opens the RuStore review sheet inside the game. RuStore decides whether to show it and limits how often a player sees it, so call it at a natural pause and do not rely on it appearing. `ShareGame` is not implemented on RuStore.

### Player

`MirraSDK.Player.IsLoggedIn` tells whether the player is signed in to RuStore. `InvokeLogin` opens the RuStore app for sign-in and reports the result when the player returns to the game. Without the RuStore app on the device it reports an error at once. RuStore does not expose a profile, so the name and id properties are empty.

### In-App Updates

Optional module: install `RuStore In-app Updates` from the Toolkit card. No game code is needed. The mode is set in the Toolkit, `RuStorePlatformInteractions > UpdateMode`:

| Mode | Behaviour |
| --- | --- |
| `Disabled` | Updates are never checked. |
| `Flexible` | RuStore asks the player, downloads in the background, and the update is installed on the next launch. Default. |
| `Immediate` | RuStore shows a full screen update the player has to finish. Falls back to `Flexible` when RuStore does not allow it. |
| `Silent` | The update downloads without a RuStore dialog and is installed on the next launch. |

The check runs once after the game starts. A download that ends while the player is in the game is not installed until the next launch, so gameplay is never interrupted by a restart.

### Remote Config

Optional module: install `RuStore Remote Config` from the Toolkit card and enter the Remote Config application id from RuStore Console in `RuStoreFlags > AppId`. Values are then read through [Experiments](flags):

```csharp
int reward = MirraSDK.Flags.GetInt("daily_reward", 100);
```

Game startup waits for the config up to `RuStoreFlags > InitTimeoutSeconds` (5 by default) and then continues with the default values. If the config arrives later, the next reads return the real values. An empty `AppId` turns the module off.

Boolean values are written as `true`, `false`, `1` or `0`. Fractional numbers use a dot.

### Build Check

An Android build of the `RuStore` configuration stops with a numbered report while the setup is incomplete. Without the check such a build would succeed and ship with payments turned off.

| Report | Fix |
| --- | --- |
| A RuStore package is not installed or is older than needed | Press the install or update button on the Toolkit card. |
| MirraSDK5 is older than 5.2.5 | Update MirraSDK5, then press `Install dependencies of RuStore API for MirraSDK5` on the card. |
| External Dependency Manager is not installed | Install `com.google.external-dependency-manager` and run `Force Resolve`. |
| Minimum API Level is below 24 | Raise it in Player Settings. |
| Application Entry Point is not Activity | Run `Fix setup`. |
| The manifest is missing, not patched or has no launcher activity | Run `Fix setup`. |
| RuStore Pay settings are missing | Run `Fix setup`, then enter the two values. |
| Console Application Id or Deeplink Scheme is empty | Enter it in `Window > RuStoreSDK > Settings > PayClient`. |
| Files from RuStore API 1.x remain | Run `Fix setup`. |

### Migrating from 1.x

Version 1.x bundled RuStore 10.0.x packages. Their Android libraries were hosted on a server that RuStore has shut down, so those builds no longer resolve.

1. Update MirraSDK5 to 5.2.5 or newer.
2. In the Toolkit package list select `RuStore API for MirraSDK5` and press the update button. It replaces the old RuStore packages with the current ones.
3. Run `MirraSDK5 > RuStore > Fix setup`. It deletes `Assets/Plugins/Android/RuStoreIntentFilterActivity.java`, removes the old launcher activity from the manifest and gives the launcher back to the game activity, adds the RuStore Pay 11 entries, and asks External Dependency Manager to resolve again. The previous manifest is saved to `Library/MirraSDK/RuStore/AndroidManifest.backup.xml`.
4. Check that `Console Application Id` and `Deeplink Scheme` are still filled in `Window > RuStoreSDK > Settings > PayClient`.
5. If the game called `MirraSDK.Payments.SupplyProduct` inside the `onSuccess` of a purchase, remove that call. Version 2.0.0 records the grant itself; with both in place a purchase interrupted later would not be restored.
6. The old `ExternalPackages/ru.rustore.*.tgz` files in the project folder are no longer used and can be deleted.

`ProductsJson` keeps its format, and no game code has to change apart from step 5.

A Toolkit older than 5.2.5 installs only the package without the RuStore packages. The project still compiles, and the build check asks for the MirraSDK5 update.

### Troubleshooting

1. Gradle reports `resource string/rustore_PayClientSettings_... not found` on Unity 2022.3. The folder `Assets/Plugins/Android/RuStoreSDKSettings.androidlib` was deleted while the editor was open. Run `Fix setup` or restart the editor, then build again.
2. Gradle reports `Could not resolve ru.rustore.sdk`. Run `Assets > External Dependency Manager > Android Resolver > Force Resolve`. If a template in `Assets/Plugins/Android` still mentions `artifactory-external.vkpartner.ru`, delete that line.
3. Payments are unavailable on the device. Check that the package name matches the app in RuStore Console, that `Console Application Id` belongs to this app, and that monetization is enabled for it.

### Not Supported

1. Subscriptions, promotions and coupons.
2. Push notifications.
3. Install referrer.
