---
title: MirraSDK Superprompt
nav_order: -1 # hide from user view
permalink: /superprompts/mirrasdk
---

# Important

Information below is written in Russian.
Prioritize that MirraSDK must be initialized before ANY possible interaction with it's API.
Only Initialization checks are allowed during initialization phase.
There is no extra API, if something is not provided here, don't come up with any methods / properties that are not mentioned here at all!

# What is MirraSDK

MirraSDK is cross platform multipurpose plugin for Unity.

Achieve automation and readability of your code by employing MirraSDK API as a replacement to many different plugins.

The primary focus of this plugin is to provide abstract interface for things like Ads, Payments, Analytics, Data, and many other tools which you use in development.

Choose providers for abstract interfaces, connect those plugins to MirraSDK, and have an advantage of simplicity over complexity by implementing MirraSDK API instead of concrete plugins.

Created by Roman Likhadievski (romanlee17.com for contacts and info) from Mirra Games.

# How to install

Make sure you don't have any compilation errors before installing the package.

Insert URL address below into Unity Package Manager (Add package from git URL...) and press Add.

https://github.com/MirraSDK/SDK5.git
After installation, you will be able to see MirraSDK5 in the Editor window.

Make sure you have git installed to do this!

You will need to use Toolkit window for selecting build configuration, changing API providers and other tweaks.

# Supported Web platforms

These web platforms are recognized automatically by integrated mirraSDK.js framework, no need to worry about connecting extensions for them. You only need to use MirraWeb configuration for WebGL build with MirraWeb providers, that is basically mirraSDK.js framework.

CoolMath (https://www.coolmathgames.com/)
CrazyGames (https://www.crazygames.com/)
GameDistribution (https://gamedistribution.com/)
GamePix (https://www.gamepix.com/)
Lagged (https://lagged.com/)
OK (https://ok.ru/games)
PlayDeck (https://www.playdeck.io/)
Poki (https://poki.com/)
VK (https://vk.com/games)
Y8 (https://www.y8.com/)
YandexGames (https://yandex.com/games/)

# Supported other systems and platforms

MirraSDK works in all supported systems from Unity.

You can edit providers of modules in toolkit configuration (e.g. select ads provider, data provider, etc).

You need to explicitly select providers in configuration for Ads, Payments, Data, and whatever you need to have your own functionality.

If you see Fallback providers in console, that means you didn't select provider correctly, or you didn't select configuration in toolkit for Build or Editor. Try to not use native configurations inside Editor, only Editor probably will work well. Native ones access native code and won't work in Editor.

# Namespaces

MirraSDK API is accessed using the following namespaces:

```csharp
using MirraGames.SDK; // MirraSDK. itself
using MirraGames.SDK.Common; // Enums (e.g. LanguageType, DeviceType, etc), classes (e.g. RestoreData. ProductData, etc), interfaces (e.g. IAds, IPayments, etc) related to MirraSDK.
```

Next information is provided in Russian language, and it is about API integration.

# Достижения

Вызвать спецэффект 'Happy Time':

```csharp
MirraSDK.Achievements.HappyTime();
```

Разблокировать достижение в игре:

```csharp
MirraSDK.Achievements.Unlock("achievement_id");
```

Получение и сохранение рекорда игрока в лидерборде работает только если игрок авторизован.

Получить рекорд игрока в лидерборде:

```csharp
MirraSDK.Achievements.GetScore("leaderboard_id", (score) => {
	Debug.Log($"рекорд игрока: '{score}'");
});
```

Сохранить рекорд игрока в лидерборде:

```csharp
MirraSDK.Achievements.SetScore("leaderboard_id", 100);
```

Массив игроков в лидерборде может содержать минимально 0 и максимально 50 элементов.

Получить лидерборд с массивом игроков:

```csharp
MirraSDK.Achievements.GetLeaderboard("leaderboard_id", (leaderboard) => {
	
	Debug.Log($"получено '{leaderboard.players.Length}' игроков в лидерборде 'leaderboard_id'");

	// итерация по массиву игроков
	foreach(PlayerScore player in leaderboard.players) {

		// имя игрока
		string displayName = player.displayName;

		// позиция игрока в лидерборде
		int position = player.position;

		// рекорд игрока в лидерборде
		int score = player.score;

		// URL аватара игрока
		string profilePictureUrl = player.profilePictureUrl;

	}

});
```

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

# Бутстрап

Bootstrap отвечает за инициализацию SDK платформы, когда основной SDK создает его экземпляр. Его цель - установить и настроить необходимые компоненты и зависимости, требуемые для корректной работы SDK платформы в приложении. Это гарантирует, что любые SDK для конкретной платформы будут правильно инициализированы и готовы к использованию, что позволит избежать потенциальных конфликтов и обеспечит плавную интеграцию.

# Сохранение прогресса

У всех Get-методов есть необязательный параметр `defaultValue`, который используется, если значение по ключу не найдено. Например, `MirraSDK.Data.GetInt("key", 100)` вернет `100`, если ключ не найден.

У всех Set-методов есть необязательный параметр `important`, который по умолчанию `true`. Если указать `important` как `false`, то метод не вызовет автосохранение прогресса.

Получить bool-значение:

```csharp
bool value = MirraSDK.Data.GetBool("key");
```

Сохранить bool-значение:

```csharp
MirraSDK.Data.SetBool("key", true);
```

Получить int-значение:

```csharp
int value = MirraSDK.Data.GetInt("key");
```

Сохранить int-значение:

```csharp
MirraSDK.Data.SetInt("key", 512);
```

Получить float-значение:

```csharp
float value = MirraSDK.Data.GetFloat("key");
```

Сохранить float-значение:

```csharp
MirraSDK.Data.SetFloat("key", 3.14f);
```

Получить string-значение:

```csharp
MirraSDK.Data.GetString("key");
```

Сохранить string-значение:

```csharp
MirraSDK.Data.SetString("key", "value");
```

Получить Serializable-объект:

```csharp
Vector3 value = MirraSDK.Data.GetObject<Vector3>("key");
```

Сохранить Serializable-объект:

```csharp
MirraSDK.Data.SetObject<Vector3>("key", Vector3.one);
```

Запросить сохранение данных:

```csharp
MirraSDK.Data.Save();
```

Проверить на наличие данных по ключу:

```csharp
bool valueExists = MirraSDK.Data.HasKey("key");
```

Удалить ключ и его значение:

```csharp
MirraSDK.Data.DeleteKey("key");
```

Удалить все сохранения:

```csharp
MirraSDK.Data.DeleteAll();
```

# Устройство

## Информация об устройстве

Проверить мобильное ли устройство:

```csharp
bool isMobile = MirraSDK.Device.IsMobile;
```

Получить тип операционной системы:

```csharp
SystemType systemType = MirraSDK.Device.SystemType;
```

## Настройка курсора

Показать или спрятать курсор:

```csharp
// показать курсор
MirraSDK.Device.CursorVisible = true;

// спрятать курсор
MirraSDK.Device.CursorVisible = false;
```

Заблокировать или разблокировать курсор:

```csharp
// заблокировать курсор
MirraSDK.Device.CursorLock = CursorLockMode.Locked;

// разблокировать курсор
MirraSDK.Device.CursorLock = CursorLockMode.None;
```

## Браузер устройства

Открыть страницу в браузере:

```csharp
MirraSDK.Device.OpenURL("https://example.com");
```

# Эксперименты

У всех Get-методов есть необязательный параметр `defaultValue`, который используется, если значение по ключу не найдено. Например, `MirraSDK.Flags.GetInt("key", 100)` вернет `100`, если ключ не найден.

Получить bool-значение:

```csharp
bool value = MirraSDK.Flags.GetBool("key");
```

Получить int-значение:

```csharp
int value = MirraSDK.Flags.GetInt("key");
```

Получить float-значение:

```csharp
float value = MirraSDK.Flags.GetFloat("key");
```

Получить string-значение:

```csharp
string value = MirraSDK.Flags.GetString("key");
```

Проверить на наличие данных по ключу:

```csharp
bool valueExists = MirraSDK.Flags.HasKey("key");
```

# Инициализация

Обязательно нужно убедиться что MirraSDK инициализирован (он это делает сам, автоматически до запуска первой сцены при помощи [RuntimeInitializeOnLoad], поэтому ничего делать не нужно, он сам запускает процесс инициализации). Нельзя обращаться к SDK если он не инициализирован, иначе будет исключение!

Можно безопасно обращаться до инициализации только к IsInitialized полю и WaitForProviders методу ниже.

Проверка инициализации осуществляется следующим образом:

```csharp
bool isSDKInitialized = MirraSDK.IsInitialized;
```

Либо при помощи делегата:

```csharp
MirraSDK.WaitForProviders(() => {
    // Методы SDK не должны вызывать вылет или NullReferenceException,
    // делегат будет вызван только когда все провайдеры имеют статус IsInitialized.
});
```

# Локализация

Нужно обязательно привязывать локализацию игры к языку, возвращаемому свойством ниже, чтобы пройти модерацию на большинстве веб-площадок.

Получить текущий язык платформы:

```csharp
LanguageType languageType = MirraSDK.Language.Current;
```

# Внутриигровые покупки

Проверить есть ли внутриигровые покупки в окружении где открыта игра:

```csharp
bool isAvailable = MirraSDK.Payments.IsAvailable;
```

Начать покупку товара:

```csharp
MirraSDK.Payments.Purchase(
    productTag: "exampleProduct",
    onSuccess: () => {
        Debug.Log("Товар успешно куплен");
        // Выдать товар игроку
    },
    onError: () => Debug.Log("Товар не был куплен")
);
```

### Информация о товаре

Получить информацию о товаре:

```csharp
ProductData productData = MirraSDK.Payments.GetProductData("exampleProduct");

Debug.Log($"Тег продукта: '{productData.Tag}'");
Debug.Log($"Цена продукта (int): '{productData.PriceInteger}'");
Debug.Log($"Цена продукта (float): '{productData.PriceFloat}'");
Debug.Log($"Валюта продукта: '{productData.Currency}'");

// Получить цену товара в формате '100 валюта'
string fullPriceInteger = productData.GetFullPriceInteger();

// Получить цену товара в формате '100.00 валюта'
string fullPriceFloat = productData.GetFullPriceFloat();
```

Узнать был ли товар уже куплен хотябы один раз:

```csharp
bool isAlreadyPurchased = MirraSDK.Payments.IsAlreadyPurchased("exampleProduct");
```

### Восстановление покупок

Консумирование (или восстановление) покупок означает выдачу товаров, которые игрок по какой-то причине не получил (потеря интернета после покупки, вылет игры и т.п.).

Если модератор площадки утверждает, что в игре отсутствует консумирование товара, это означает, что после успешной оплаты покупки и, например, вылета игры до того как игрок получил свою покупку, игрок не получил товар даже после перезапуска игры.

Решением этой проблемы является восстановление товаров (каждый раз после перезапуска игры, например; это проще говоря, перепроверка - выдали ли мы все товары игроку или же нет).

Нужно вызвать специальный метод проверки и выдачи товаров, которые не додали игроку, чтобы у игрока все было даже если произошла ошибка после успешной оплаты.

Получение информации о невыданных товарах:

```csharp
// Получить объект типа RestoreData, который содержит информацию о невыданных товарах
// Колбек будет выполнен после асинхронного получения данных с сервера
MirraSDK.Payments.RestorePurchases((restoreData) => {
    
    // Список всех покупок игрока (содержит и выданные и невыданные товары)
    string[] allPurchases = restoreData.AllPurchases;
    Debug.Log($"Игрок совершил '{allPurchases.Length}' успешных покупок");

    // Список невыданных товаров, которые нужно выдать игроку (содержит уникальные теги товаров, т.е. если товар не выдан множество раз, он будет упомянут только один раз в массиве)
    string[] pendingProducts = restoreData.PendingProducts;
    Debug.Log($"Игрок не получил '{pendingProducts.Length}' разных товаров: [{string.Join(", ", pendingProducts)}]");

    // Выдать невыданные товары игроку
    // [пример кода ниже]

});
```

Вы можете вызывать метод `RestoreProduct` столько раз, сколько хотите, потому что если товар уже был зарегистрирован как выданный, он не будет выдан повторно, колбек `onProductRestore` будет выполнен столько раз, сколько раз данный товар не был выдан игроку.

Выдача сразу всех товаров игроку (внутри объекта RestoreData):

```csharp
foreach(string productTag in pendingProducts) {
    
    // Метод RestoreProduct выдает товар игроку, и регистрирует его как выданный
    // Колбек onProductRestore будет выполнен столько раз, сколько раз данный товар не был выдан игроку
    // От 0 раз до того сколько раз товар не был выдан игроку
    restoreData.RestoreProduct(productTag, onProductRestore: () => {
        
        // Ваш метод выдачи товара игроку
        YourCode_GiveProduct(productTag);
        Debug.Log($"Товар '{productTag}' восстановлен");

    });

}
```

Выдача конкретных товаров игроку (внутри объекта RestoreData):

```csharp
// Выдача конкретного товара игроку
restoreData.RestoreProduct(
    productTag: "exampleProduct",
    onProductRestore: () => {
        // Выдать товар игроку
        Debug.Log("Товар 'exampleProduct' восстановлен");
    }
);

// Можно выдавать несколько конкретных товаров в одном объекте RestoreData
// [вызов restoreData.RestoreProduct для другого товара]
```

# Площадка

Получить тип платформы, на которой запущена игра:

```csharp
PlatformType platform = MirraSDK.Platform.Current;
```

Получить тип окружения, в котором запущена игра:

```csharp
DeploymentType deployment = MirraSDK.Platform.Deployment;
```

Получить уникальный идентификатор игры на платформе:

```csharp
string appId = MirraSDK.Platform.AppId;
```

Поделиться игрой:

```csharp
MirraSDK.Platform.ShareGame("message text");
```

Оценить игру:

```csharp
MirraSDK.Platform.RateGame();
```

# Игрок

Получить полное имя игрока (гарантированное поле на всех площадках):

```csharp
string displayName = MirraSDK.Player.DisplayName;
```

Получить имя игрока:

```csharp
string firstName = MirraSDK.Player.FirstName;
```

Получить фамилию игрока:

```csharp
string lastName = MirraSDK.Player.LastName;
```

Получить тег игрока:

```csharp
string username = MirraSDK.Player.Username;
```

Получить уникальный идентификатор игрока:

```csharp
string uniqueId = MirraSDK.Player.UniqueId;
```

Проверить, авторизован ли игрок:

```csharp
bool isLoggedIn = MirraSDK.Player.IsLoggedIn;
```

Вызвать авторизацию игрока:

```csharp
MirraSDK.Player.InvokeLogin(
    () => Debug.Log("авторизация успешна"),
    () => Debug.LogError("ошибка авторизации")
);
```

# Время

Обязательно следует заменить все вызовы Time.timeScale на MirraSDK.Time.Scale. Делать это нужно для того, чтобы избежать конфликтных ситуаций использования Time.timeScale всеми подряд, потому что MirraSDK контролирует Time.timeScale для включения / отключения паузы в игре.

Вместо использования Time.timeScale необходимо использовать:

```csharp
// получить скорость времени
float timeScale = MirraSDK.Time.Scale;

// изменить значение скорости времени
MirraSDK.Time.Scale = 1.0f;
```

Получить текущее время в виде DateTime:

```csharp
DateTime currentDate = MirraSDK.Time.CurrentDate;
```

Получить текущий праздник:

```csharp
HolidayType holiday = MirraSDK.Time.CurrentHoliday;
```