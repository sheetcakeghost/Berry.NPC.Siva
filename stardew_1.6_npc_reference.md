# Stardew Valley 1.6 Custom NPC Reference Guide

## Core Requirements

### Manifest.json
```json
{
  "Name": "[Mod Name]",
  "Author": "[Author]",
  "Version": "1.0.0",
  "Description": "[Description]",
  "UniqueID": "[Author].[ModName]",
  "MinimumApiVersion": "3.18.0",
  "UpdateKeys": ["GitHub:[Username]/[Repository]"],
  "ContentPackFor": {
    "UniqueID": "Pathoschild.ContentPatcher"
  }
}
```

### Content.json Format
- Must use `"Format": "2.0.0"` for SDV 1.6
- NPC data structure updated from 1.5 to use object notation

## NPC Data Structure (1.6 Format)

```json
{
  "Format": "2.0.0",
  "Action": "EditData",
  "Target": "Data/Characters",
  "Entries": {
    "NPCName": {
      "Personality": {
        "Optimism": 0, // -1 to 1 range
        "Gender": "feminine/masculine/undefined",
        "Manners": "polite/rude/neutral", 
        "SocialAnxiety": "outgoing/shy/neutral",
        "Outlook": "positive/negative/neutral",
        "SelfAbsorption": "self_centered/altruistic/neutral"
      },
      "Home": "MapName",
      "Birthday": "season day",
      "DisplayName": "Display Name",
      "DefaultMap": "MapName",
      "DefaultPosition": [x, y],
      "Orientation": 2, // 0=up, 1=right, 2=down, 3=left
      "Occupation": "Occupation",
      "CanSocialize": true,
      "Age": "adult/teen/child",
      "Love": ["NPC1", "NPC2"],
      "Relationships": {
        "NPC1": "friend",
        "NPC2": "love",
        "NPC3": "disliked"
      },
      "RandomDialogue": [
        "Random dialogue 1.",
        "Random dialogue 2."
      ],
      "NameOfTodaysSpecialDialogue": "Hi, today is a special day!",
      "SeasonDialogue": {
        "spring": ["Spring dialogue 1.", "Spring dialogue 2."],
        "summer": ["Summer dialogue 1.", "Summer dialogue 2."],
        "fall": ["Fall dialogue 1.", "Fall dialogue 2."],
        "winter": ["Winter dialogue 1.", "Winter dialogue 2."]
      },
      "FestivalDialogue": {
        "spring1": "Spring egg festival dialogue!",
        "summer11": "Luau dialogue!"
      },
      "DayOfWeekDialogue": {
        "Monday": ["Monday dialogue 1.", "Monday dialogue 2."],
        "Friday": ["Friday dialogue 1.", "Friday dialogue 2."]
      },
      "WeatherDialogue": {
        "sun": ["Sunny dialogue 1.", "Sunny dialogue 2."],
        "rain": ["Rainy dialogue 1.", "Rainy dialogue 2."],
        "snow": ["Snowy dialogue 1.", "Snowy dialogue 2."]
      }
    }
  }
}
```

## Gift Tastes (1.6 Format)

```json
{
  "Format": "2.0.0",
  "Action": "EditData",
  "Target": "Data/GiftTastes",
  "Entries": {
    "NPCName": {
      "Love": [68, 64, 254], // Item IDs or category names
      "Like": [216, 221, "Vegetable"],
      "Neutral": [18, 20, 22],
      "Dislike": [300, 304, 306],
      "Hate": [168, 170, 180]
    }
  }
}
```

## Events Format

```json
{
  "Format": "2.0.0",
  "Action": "EditData",
  "Target": "Data/Events/[LocationName]",
  "Entries": {
    "[eventID]/f [NPCName] [heartLevel]/t [timeOfDay]/w [weather]/c [condition]": "speak NPCName \"Dialogue text.\"#pause 500#emote NPCName 20#move farmer 0 1 3"
  }
}
```

## Schedule Format

```json
{
  "Format": "2.0.0",
  "Action": "EditData",
  "Target": "Data/Schedules/[NPCName]",
  "Entries": {
    "spring": "800 [location] [x] [y] [direction] [animation]/1200 [location] [x] [y] [direction] \"[dialogue]\"",
    "spring_[day]": "800 [location] [x] [y] [direction]/1200 [location] [x] [y] [direction]",
    "rain": "STAY_IN_BED",
    "summer": "GOTO spring"
  }
}
```

## Asset Requirements

- **Portraits**: 128×192 pixels (8 expressions per row, typically 32 total)
- **Character sprites**: 16×32 pixels per frame
  - Rows: down, right, up, left
  - Need idle and walking animations for each direction
- **Required file structure**:
  - `assets/[NPCName]/portrait.png`
  - `assets/[NPCName]/sprite.png`

## Dialogue Tokens

- **Emotions**: `$e` followed by a number (0-11) - Example: `$e0`
- **Randomization**: `{{Random:option1|option2|option3}}`
- **Conditional**: `{{if:condition|result_if_true|result_if_false}}`
- **Time**: `{{Time}}` shows current time
- **Location**: `{{location}}` shows current location
- **Player name**: `@` or `{{farmer}}`
- **Hearts**: `{{hearts:[NPCname]}}` shows friendship level
- **Season**: `{{season}}` shows current season

## Character Registration

### Using Content Patcher:
```json
{
  "Format": "2.0.0",
  "Action": "Load",
  "Target": "Characters/[NPCName]",
  "FromFile": "assets/[NPCName]/sprite.png"
}
```

```json
{
  "Format": "2.0.0",
  "Action": "Load",
  "Target": "Portraits/[NPCName]",
  "FromFile": "assets/[NPCName]/portrait.png"
}
```

### Using SpaceCore:
```csharp
// In a C# mod with SpaceCore:
SpaceCore.APIs.CreateCharacter("NPCName", "Assets/portrait.png", "Assets/sprite.png");
```

## Siva Specific Information

- **Name**: Siva
- **Type**: Alien NPC
- **Default Location**: Calico Desert
- **Occupation**: Mr. Qi's personal chef
- **Birthday**: Spring 8
- **Diet**: Consumes light filtered through minerals/gems
- **Pet**: Blik (alien pet resembling a black cat)
- **Background**: Crashed ship in Calico Desert
- **Appearance**: Blue skin, long pink hair, green eyes, masculine features
- **Personality**: 
  - Optimism: 0.8
  - Gender: masculine
  - Manners: polite
  - SocialAnxiety: shy
  - Outlook: positive
  - SelfAbsorption: neutral

### Siva's Background Story
Siva is an alien chef who crash-landed in the Calico Desert three years ago. He was on a culinary exploration mission when his ship malfunctioned near Earth. Mr. Qi discovered him and offered protection in exchange for becoming his personal chef, preparing exotic dishes from recipes across the galaxy.

### Speech Pattern
- Speaks formally with occasional alien terminology
- Sometimes adds "~glimmer" at the end of sentences when excited
- Uses food metaphors frequently
- Has difficulty with Earth idioms and often misinterprets them

### Relationships
- **Mr. Qi**: Employer and protector, relationship of mutual respect
- **Sandy**: Friendly neighbor who helps keep his alien identity secret
- **Wizard**: Occasional visitor who shares interest in the unusual and exotic
- **Dwarf**: Bonded over both being non-human entities on Earth
- **Emily**: Potential friend who appreciates his unique perspective and colorful appearance

### Daily Routine
- Mornings: Tends to his small garden of exotic plants behind his living quarters
- Afternoons: Experiments with cooking or visits the desert to collect mineral light
- Evenings: Often serves dinner for Mr. Qi or studies Earth culture

### Character Arc
As friendship increases with the player:
1. Initially secretive about alien identity
2. Gradually shares recipes from his home planet
3. Requests help finding special ingredients from around Stardew Valley
4. Eventually reveals his homesickness and desire to repair his ship
5. At max hearts, decides whether to stay on Earth or prepare for journey home

### Gift Preferences
- **Loves**: Prismatic Shard, Fire Quartz, Gemstones (absorbs their light)
- **Likes**: Cooked dishes, especially exotic ones
- **Dislikes**: Raw fish, clay, basic minerals
- **Hates**: Void essence, strange bun, battery packs (reminds him of ship malfunction)

### Events
- 2 Hearts: Shows player his small ship wreckage hidden in the desert
- 4 Hearts: Cooking lesson using exotic ingredients
- 6 Hearts: Stargazing event where he points out his home system
- 8 Hearts: Confrontation with someone who discovered his alien identity
- 10 Hearts: Reveals technology to communicate with his home planet

## Common Pitfalls

- Using Format 1.25.0 instead of 2.0.0
- Using deprecated `Data/NPCs` and `Data/NPCGiftTastes` targets (now `Data/Characters` and `Data/GiftTastes`)
- Using string format for NPCDispositions (1.5) instead of object notation (1.6)
- Incorrect pathing for assets
- Missing schedule entries
- Using 64×64 portrait size instead of 128×192
- Missing proper event preconditions
- Not including required Personality properties

## Important Links

- Tutorial: https://stardewmodding.wiki.gg/wiki/Tutorial:_Making_a_Custom_NPC
- Template: https://stardewmodding.wiki.gg/wiki/Npc_template
- Migration Guide: https://stardewvalleywiki.com/Modding:Migrate_to_Stardew_Valley_1.6
- Content Patcher: https://github.com/Pathoschild/StardewMods/tree/stable/ContentPatcher/docs/author-guide
- SpaceCore: https://github.com/spacechase0/StardewValleyMods/tree/develop/SpaceCore/docs