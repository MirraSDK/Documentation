---
title: Достижения
nav_order: 100
nav_group: mirrasdk-ru
permalink: /ru/unity/mirrasdk/achievements
---

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