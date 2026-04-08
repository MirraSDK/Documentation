---
title: Внутриигровые покупки
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/payments
---

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
