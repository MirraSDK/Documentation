---
title: Achievements
nav_order: 100
nav_group: mirrasdk-en
permalink: /en/unity/mirrasdk/achievements
---

# Achievements

Trigger the 'Happy Time' special effect:

```csharp
MirraSDK.Achievements.HappyTime();
```

Unlock an in-game achievement:

```csharp
MirraSDK.Achievements.Unlock("achievement_id");
```

Getting and saving a player's score in a leaderboard only works if the player is authorized.

Get the player's score in a leaderboard:

```csharp
MirraSDK.Achievements.GetScore("leaderboard_id", (score) => {
	Debug.Log($"player's score: '{score}'");
});
```

Save the player's score in a leaderboard:

```csharp
MirraSDK.Achievements.SetScore("leaderboard_id", 100);
```

The player array in a leaderboard can contain a minimum of 0 and a maximum of 50 elements.

Get a leaderboard with a player array:

```csharp
MirraSDK.Achievements.GetLeaderboard("leaderboard_id", (leaderboard) => {
	
	Debug.Log($"received '{leaderboard.players.Length}' players in leaderboard 'leaderboard_id'");

	// iterate through the player array
	foreach(PlayerScore player in leaderboard.players) {

		// player's name
		string displayName = player.displayName;

		// player's position in the leaderboard
		int position = player.position;

		// player's score in the leaderboard
		int score = player.score;

		// player's avatar URL
		string profilePictureUrl = player.profilePictureUrl;

	}

});
```
