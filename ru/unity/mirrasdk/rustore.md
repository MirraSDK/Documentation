---
title: RuStore
nav_order: 200
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/rustore
---

# RuStore

Пакет `RuStore API for MirraSDK5` запускает игру в RuStore через обычный API MirraSDK: внутриигровые покупки, отзывы, вход игрока, а при желании - обновления внутри приложения и удаленная конфигурация. Код игры не обращается к RuStore напрямую.

Версия 2.0.0 работает с RuStore Pay 11.2.0, Review 10.5.2 и Core 10.5.1, которые устанавливаются из реестра пакетов RuStore.

### Требования

1. Unity 2022.3.38 или новее.
2. MirraSDK5 5.2.5 или новее.
3. External Dependency Manager for Unity (`com.google.external-dependency-manager`). Он добавляет Android-библиотеки RuStore в сборку.
4. `Player Settings > Android > Other Settings > Minimum API Level` не ниже 24.
5. `Application Entry Point` в значении `Activity`. С `GameActivity` платежи RuStore не работают.
6. Игра зарегистрирована в RuStore Консоли с тем же именем пакета, что и у сборки.

### Установка

1. Откройте `MirraSDK5 > Open Toolkit` и перейдите к списку пакетов.
2. Выберите `RuStore API for MirraSDK5` и нажмите кнопку установки. Toolkit добавит в проект реестр `RuStore Nexus`, установит `ru.rustore.core`, `ru.rustore.pay` и `ru.rustore.review`, а затем сам пакет.
3. У дополнительных модулей свои кнопки на той же карточке: `Install RuStore In-app Updates` и `Install RuStore Remote Config`. Ставьте только то, что нужно игре.
4. В `Player Settings > Android > Publishing Settings` включите `Custom Main Gradle Template`, `Custom Gradle Properties Template` и `Custom Gradle Settings Template`, затем выполните `Assets > External Dependency Manager > Android Resolver > Force Resolve`.

В описании карточки перечислены все пакеты RuStore с установленными версиями. Когда выходит новая версия пакета, на той же карточке появляется кнопка обновления, которая обновляет и пакеты RuStore.

### Настройка

1. Переключите платформу на Android и выберите `RuStore` как конфигурацию сборки в Toolkit.
2. Выполните `MirraSDK5 > RuStore > Fix setup`. Команда создает свой Android-манифест, если его нет в проекте, создает настройки RuStore Pay, добавляет в манифест активность и записи настроек RuStore и ставит `Application Entry Point` в `Activity`.
3. В `Window > RuStoreSDK > Settings > PayClient` введите `Console Application Id` (число в адресе страницы приложения в RuStore Консоли, между `apps/` и `/versions`) и `Deeplink Scheme` (любое уникальное для игры имя, строчные латинские буквы и цифры).
4. В Toolkit откройте конфигурацию `RuStore` и перечислите идентификаторы товаров в `RuStorePayments > ProductsJson`:

```json
{ "Values": ["coins100", "noads"] }
```

5. Соберите игру. `MirraSDK5 > RuStore > Validate setup` выводит ту же проверку без сборки.

### Внутриигровые покупки

Товары создаются в RuStore Консоли как потребляемые или непотребляемые. Цены приходят из магазина: `GetProductData` возвращает цену в целых единицах валюты (`PriceFloat` сохраняет копейки) и код валюты. Для идентификатора, которого нет в `ProductsJson` или в Консоли, возвращается цена 0.

```csharp
MirraSDK.Payments.Purchase(
    productTag: "coins100",
    onSuccess: () => GiveProduct("coins100"),
    onError: () => Debug.Log("Покупка не состоялась")
);
```

Выдавайте товар внутри `onSuccess` и только там. MirraSDK записывает эту выдачу в сохранение и по ней позже понимает, что игрок уже получил.

Вызывайте `RestorePurchases` при каждом запуске и выдавайте каждый невыданный товар:

```csharp
MirraSDK.Payments.RestorePurchases((restoreData) => {
    foreach (string productTag in restoreData.PendingProducts) {
        restoreData.RestoreProduct(productTag, () => GiveProduct(productTag));
    }
});
```

Товар считается невыданным, когда в RuStore оплаченных покупок этого товара больше, чем выдано по сохранению:

1. Игра закрылась после оплаты, но до выдачи: покупка вернется один раз при следующем запуске.
2. Сохранение удалено (переустановка, новое устройство): вернутся все покупки, которые RuStore по-прежнему показывает, включая потребляемые.
3. Возвращенные и незавершенные платежи не учитываются.

`IsAlreadyPurchased` возвращает true, когда у игрока есть хотя бы одна оплаченная покупка товара. `MirraSDK.Payments.IsAvailable` равен false, когда RuStore не разрешает платежи для этой игры или игрока, например если в RuStore Консоли не включена монетизация.

Игрок, не вошедший в RuStore, все равно видит цены; список покупок остается пустым, пока RuStore не узнает игрока. Запуск игры никогда не ждет RuStore дольше 10 секунд.

Подписки не поддерживаются.

### Отзывы

```csharp
MirraSDK.Platform.RateGame();
```

Открывает окно отзыва RuStore внутри игры. RuStore сам решает, показывать ли окно, и ограничивает частоту показов, поэтому вызывайте метод в естественной паузе и не рассчитывайте, что окно появится каждый раз. `ShareGame` в RuStore не реализован.

### Игрок

`MirraSDK.Player.IsLoggedIn` показывает, вошел ли игрок в RuStore. `InvokeLogin` открывает приложение RuStore для входа и сообщает результат, когда игрок возвращается в игру. Если приложения RuStore на устройстве нет, метод сразу сообщает об ошибке. RuStore не отдает профиль, поэтому свойства с именем и идентификатором пустые.

### Обновления внутри приложения

Дополнительный модуль: установите `RuStore In-app Updates` с карточки в Toolkit. Код в игре не нужен. Режим задается в Toolkit, `RuStorePlatformInteractions > UpdateMode`:

| Режим | Поведение |
| --- | --- |
| `Disabled` | Обновления не проверяются. |
| `Flexible` | RuStore спрашивает игрока, скачивает в фоне, обновление ставится при следующем запуске. Значение по умолчанию. |
| `Immediate` | RuStore показывает полноэкранное обновление, которое игрок должен завершить. Если RuStore его не разрешает, используется `Flexible`. |
| `Silent` | Обновление скачивается без окна RuStore и ставится при следующем запуске. |

Проверка выполняется один раз после запуска игры. Загрузка, которая закончилась, пока игрок в игре, устанавливается только при следующем запуске, поэтому игра никогда не прерывается перезапуском.

### Удаленная конфигурация

Дополнительный модуль: установите `RuStore Remote Config` с карточки в Toolkit и введите идентификатор приложения Remote Config из RuStore Консоли в `RuStoreFlags > AppId`. Значения читаются через [Эксперименты](flags):

```csharp
int reward = MirraSDK.Flags.GetInt("daily_reward", 100);
```

Запуск игры ждет конфигурацию не дольше `RuStoreFlags > InitTimeoutSeconds` (по умолчанию 5) и затем продолжается со значениями по умолчанию. Если конфигурация придет позже, следующие чтения вернут настоящие значения. Пустой `AppId` выключает модуль.

Логические значения записываются как `true`, `false`, `1` или `0`. В дробных числах используется точка.

### Проверка перед сборкой

Android-сборка конфигурации `RuStore` останавливается с пронумерованным отчетом, пока настройка не завершена. Без проверки такая сборка прошла бы успешно и вышла с выключенными платежами.

| Отчет | Что сделать |
| --- | --- |
| Пакет RuStore не установлен или старее нужного | Нажмите кнопку установки или обновления на карточке в Toolkit. |
| MirraSDK5 старее 5.2.5 | Обновите MirraSDK5, затем нажмите `Install dependencies of RuStore API for MirraSDK5` на карточке. |
| Не установлен External Dependency Manager | Установите `com.google.external-dependency-manager` и выполните `Force Resolve`. |
| Minimum API Level ниже 24 | Поднимите его в Player Settings. |
| Application Entry Point не Activity | Выполните `Fix setup`. |
| Манифеста нет, он не дополнен или в нем нет стартовой активности | Выполните `Fix setup`. |
| Нет настроек RuStore Pay | Выполните `Fix setup`, затем введите два значения. |
| Пустой Console Application Id или Deeplink Scheme | Введите значение в `Window > RuStoreSDK > Settings > PayClient`. |
| Остались файлы RuStore API 1.x | Выполните `Fix setup`. |

### Переход с версии 1.x

Версия 1.x включала пакеты RuStore 10.0.x. Их Android-библиотеки лежали на сервере, который RuStore отключил, поэтому такие сборки больше не собираются.

1. Обновите MirraSDK5 до 5.2.5 или новее.
2. В списке пакетов Toolkit выберите `RuStore API for MirraSDK5` и нажмите кнопку обновления. Она заменит старые пакеты RuStore на актуальные.
3. Выполните `MirraSDK5 > RuStore > Fix setup`. Команда удаляет `Assets/Plugins/Android/RuStoreIntentFilterActivity.java`, убирает из манифеста старую стартовую активность и возвращает запуск игровой активности, добавляет записи RuStore Pay 11 и просит External Dependency Manager заново разрешить зависимости. Прежний манифест сохраняется в `Library/MirraSDK/RuStore/AndroidManifest.backup.xml`.
4. Проверьте, что `Console Application Id` и `Deeplink Scheme` по-прежнему заполнены в `Window > RuStoreSDK > Settings > PayClient`.
5. Если игра вызывала `MirraSDK.Payments.SupplyProduct` внутри `onSuccess` покупки, уберите этот вызов. Версия 2.0.0 записывает выдачу сама; если оставить оба вызова, прерванная позже покупка не восстановится.
6. Старые файлы `ExternalPackages/ru.rustore.*.tgz` в папке проекта больше не используются, их можно удалить.

Формат `ProductsJson` не меняется, и кроме шага 5 код игры менять не нужно.

Toolkit старее 5.2.5 устанавливает только сам пакет, без пакетов RuStore. Проект при этом компилируется, а проверка перед сборкой попросит обновить MirraSDK5.

### Решение проблем

1. Gradle сообщает `resource string/rustore_PayClientSettings_... not found` в Unity 2022.3. Папка `Assets/Plugins/Android/RuStoreSDKSettings.androidlib` была удалена при открытом редакторе. Выполните `Fix setup` или перезапустите редактор и соберите снова.
2. Gradle сообщает `Could not resolve ru.rustore.sdk`. Выполните `Assets > External Dependency Manager > Android Resolver > Force Resolve`. Если шаблон в `Assets/Plugins/Android` все еще упоминает `artifactory-external.vkpartner.ru`, удалите эту строку.
3. Платежи недоступны на устройстве. Проверьте, что имя пакета совпадает с приложением в RuStore Консоли, что `Console Application Id` относится к этому приложению и что для него включена монетизация.

### Не поддерживается

1. Подписки, акции и купоны.
2. Push-уведомления.
3. Install referrer.
