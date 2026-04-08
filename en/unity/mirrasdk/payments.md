---
title: In-Game Purchases
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/payments
---

# In-Game Purchases

Check if in-game purchases are available in the environment where the game is running:

```csharp
bool isAvailable = MirraSDK.Payments.IsAvailable;
```

Start a product purchase:

```csharp
MirraSDK.Payments.Purchase(
    productTag: "exampleProduct",
    onSuccess: () => {
        Debug.Log("Product successfully purchased");
        // Grant the product to the player
    },
    onError: () => Debug.Log("Product was not purchased")
);
```

### Product Information

Get product information:

```csharp
ProductData productData = MirraSDK.Payments.GetProductData("exampleProduct");

Debug.Log($"Product tag: '{productData.Tag}'");
Debug.Log($"Product price (int): '{productData.PriceInteger}'");
Debug.Log($"Product price (float): '{productData.PriceFloat}'");
Debug.Log($"Product currency: '{productData.Currency}'");

// Get the product price in '100 currency' format
string fullPriceInteger = productData.GetFullPriceInteger();

// Get the product price in '100.00 currency' format
string fullPriceFloat = productData.GetFullPriceFloat();
```

Check if the product has already been purchased at least once:

```csharp
bool isAlreadyPurchased = MirraSDK.Payments.IsAlreadyPurchased("exampleProduct");
```

### Purchase Restoration

Consuming (or restoring) purchases means granting products that the player did not receive for some reason (internet loss after purchase, game crash, etc.).

If a platform moderator states that the game lacks product consumption, it means that after a successful purchase payment and, for example, the game crashing before the player received their purchase, the player did not receive the product even after restarting the game.

The solution to this problem is restoring products (every time after restarting the game, for example; this is essentially a re-check — whether we have granted all products to the player or not).

You need to call a special method to check and grant products that were not delivered to the player, so that the player has everything even if an error occurred after successful payment.

Getting information about undelivered products:

```csharp
// Get a RestoreData object that contains information about undelivered products
// The callback will be executed after asynchronously receiving data from the server
MirraSDK.Payments.RestorePurchases((restoreData) => {
    
    // List of all player purchases (contains both delivered and undelivered products)
    string[] allPurchases = restoreData.AllPurchases;
    Debug.Log($"The player has made '{allPurchases.Length}' successful purchases");

    // List of undelivered products that need to be granted to the player (contains unique product tags, i.e., if a product was not delivered multiple times, it will be mentioned only once in the array)
    string[] pendingProducts = restoreData.PendingProducts;
    Debug.Log($"The player did not receive '{pendingProducts.Length}' different products: [{string.Join(", ", pendingProducts)}]");

    // Grant undelivered products to the player
    // [example code below]

});
```

You can call the `RestoreProduct` method as many times as you want, because if a product has already been registered as delivered, it will not be delivered again. The `onProductRestore` callback will be executed as many times as the given product was not delivered to the player.

Granting all products to the player at once (inside the RestoreData object):

```csharp
foreach(string productTag in pendingProducts) {
    
    // The RestoreProduct method grants the product to the player and registers it as delivered
    // The onProductRestore callback will be executed as many times as the given product was not delivered to the player
    // From 0 times up to however many times the product was not delivered to the player
    restoreData.RestoreProduct(productTag, onProductRestore: () => {
        
        // Your method for granting the product to the player
        YourCode_GiveProduct(productTag);
        Debug.Log($"Product '{productTag}' restored");

    });

}
```

Granting specific products to the player (inside the RestoreData object):

```csharp
// Granting a specific product to the player
restoreData.RestoreProduct(
    productTag: "exampleProduct",
    onProductRestore: () => {
        // Grant the product to the player
        Debug.Log("Product 'exampleProduct' restored");
    }
);

// You can grant multiple specific products in a single RestoreData object
// [call restoreData.RestoreProduct for another product]
```
