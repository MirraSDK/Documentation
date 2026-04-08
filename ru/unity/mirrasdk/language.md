---
title: Локализация
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/language
---

# Локализация

Нужно обязательно привязывать локализацию игры к языку, возвращаемому свойством ниже, чтобы пройти модерацию на большинстве веб-площадок.

Получить текущий язык платформы:

```csharp
LanguageType languageType = MirraSDK.Language.Current;
```