---
title: Localization
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/language
---

# Localization

It is mandatory to bind the game's localization to the language returned by the property below in order to pass moderation on most web platforms.

Get the current platform language:

```csharp
LanguageType languageType = MirraSDK.Language.Current;
```
