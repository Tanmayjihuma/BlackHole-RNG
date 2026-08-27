# 🎮 Game Systems & World Expansion Documentation

---

# 1. DATA STORAGE SAFE LIMITS

### The 9 Quadrillion Cap

> **Important:** Do keep it in mind in **Scrap** and **XP**.

---

# 2. WORKSPACE FOLDER MAPPING

### How to Add World Items

## `[AREAS_ITEMS]` — Atomic

**Destructibles (Trees, Cars)**

1. Add a **Model**.
2. Inside Model: Add an invisible **BasePart** named `HitBox`.
3. Inside `HitBox`: Add **String Attribute** `ItemType` (e.g., `"Tree"`).
4. Inside `HitBox`: Add **Attachments** named `Damage` and `Health`.

---

## `[PLANETS]` — Atomic

**Multiplier Earths**

1. Add a **Model** named exactly `Earth X`.
2. Inside Model: Add **BasePart** named `HitBox`.
3. Inside `HitBox`: Add **Attachment** named `Damage`.

---

## `[LUCK MACHINES]` — Persistent

**Gacha Rollers**

1. Add a **Model** named `LuckMachine[ZoneNumber]`.
2. Inside Model: Add **Attachment** `PrompDisplay` and **BasePart** `Spawn`.
3. Inside Model: Add Attributes:

   * `Cost` — Number
   * `ConfigName` — String
   * `LuckDisplay` — String

---

## `[GLOBAL LEADERBOARDS]` — Persistent

1. Duplicate existing board.
2. Rename Model to the exact **DataStore key** (e.g., `TotalKillsData`).

---

## `[SPACESHIP]` — Persistent

**Shop Prompts**

1. Add a **Model** named `Spaceship_Stage[X]`.
2. Inside Model: Add **Attachment** named `Attachment`.

---

## `[BASESPAWNS & ZONESPAWNS]` — Persistent

**Teleporters**

1. Add an invisible, anchored **BasePart** named `Spawn_[ZoneNumber]`.

---

## `[DOORS]` — Persistent

**Zone Barriers**

1. Add a **Model** named `Door[ZoneNumber]`.
2. Inside Model: Add a physical **BasePart** named `Part` to block the path.

---

## `[INVISIBLE WALLS, MAP, RANDOMSTUFF]` — Default

> No scripting required. Drop raw meshes, parts, or models here.

---

# 3. PROGRESSION SYSTEMS

## 3.1 PROGRESSION: HOW TO EXPAND LEVELS & XP

**Config File:**
`ReplicatedStorage.Config.LevelConfig`

**Action:**
Scroll to the `XPRequirements` table and add new level numbers as indices.

**Code Example:**

```lua
[150] = 180000000000000,
[151] = 220000000000000,
```

**Note:**
The server and UI automatically read this; no extra scripting is needed.

---

## 3.2 PROGRESSION: HOW TO ADD NEW REBIRTH TIERS

**Config File:**
`ReplicatedStorage.Config.RebirthData`

**Action:**
Add the next tier number, its level requirement, and the new multiplier.

**Code Example:**

```lua
[7] = {LevelReq = 200, Multiplier = 4.5},
```

**Note:**
The UI and Rebirth server logic automatically update based on this table.

---

## 3.3 ITEMS: HOW TO ADD A NEW BLACKHOLE

**Workspace/Storage:**

1. Duplicate an existing Blackhole model in `ServerStorage.Models.Blackhole`.
2. Rename it to your new item (e.g., `"Galactic Rift"`).
3. Keep `StreamingMode` set to `Persistent`.
4. Ensure it still has the `HitBox` and `Main` base parts inside.

**Config File:**
`ReplicatedStorage.Config.BlackholeData`

**Action:**
Add the exact name and its stats to the table.

**Code Example:**

```lua
["Galactic Rift"] = {
    Damage = 15000000,
    OrbitDistance = 7,
    OrbitSpeed = 3,
    Scale = 1,
    Image = "rbxassetid://123456",
    Rarity = "Mythic"
},
```

---

## 3.4 WORLD: HOW TO ADD NEW DESTRUCTIBLE ITEMS

### Trees, Cars, etc.

**Workspace:**

1. Group your meshes into a Model in `Workspace.Areas_Items`.
2. Set the Model's `StreamingMode` to `Atomic`.
3. Create an invisible **BasePart** named `HitBox` inside the Model.
4. Add a **String Attribute** to `HitBox` named `ItemType` (e.g., `"Giant Robot"`).
5. Add two **Attachments** inside `HitBox` named `Damage` and `Health`.

**Config File:**
`ReplicatedStorage.Config.DestructiblesConfig`

**Action:**
Register the exact `ItemType` string.

**Code Example:**

```lua
["Giant Robot"] = {
    Health = 500000000,
    Scrap = 5000000,
    XP = 25000000,
    RespawnTime = 25
},
```

---

## 3.5 WORLD: HOW TO ADD A NEW ZONE

### Doors & Spawns

**Workspace (Spawns):**

1. In `Workspace.ZoneSpawns`, add a Part named `Spawn_9` (`StreamingMode: Persistent`).
2. In `Workspace.BaseSpawns`, add a Part named `Spawn_9` (`StreamingMode: Persistent`).

**Workspace (Doors):**

1. In `Workspace.Doors`, create a Model named `Door9` (`StreamingMode: Persistent`).
2. Add a physical barrier **BasePart** named `Part` inside the Model.

**Config File:**
`ReplicatedStorage.Config.ZoneData`

**Action:**
Map the new Zone number to a Level Requirement.

**Code Example:**

```lua
[9] = 100, -- Requires Level 100 to unlock Zone 9
```

---

## 3.6 WORLD: HOW TO ADD A NEW PLANET / EARTH

**Workspace:**

1. In `Workspace.Planets`, create a Model named `Earth 6`.
2. Set `StreamingMode` to `Atomic`.
3. Add a **BasePart** named `HitBox` inside the Model.
4. Add an **Attachment** named `Damage` to the `HitBox`.

**Config File:**
`ReplicatedStorage.Config.EarthConfig`

**Action:**
Add its gamepass lock and multiplier.

**Code Example:**

```lua
["Earth 6"] = {
    Multiplier = 100,
    Requirement = "Pass_Pad100x",
    PassId = 123456789
},
```

---

## 3.7 ECONOMY: HOW TO ADD A NEW LUCK MACHINE

### Gacha

**Workspace:**

1. Create a Model in `Workspace.LuckMachines` named `LuckMachine9` (`StreamingMode: Persistent`).
2. Add an **Attachment** named `PrompDisplay`.
3. Add a **Part** named `Spawn` (where the player teleports to).
4. Add 3 Attributes to the Model:

   * `Cost` — Number
   * `ConfigName` — String: `"Zone9_LuckMachine"`
   * `LuckDisplay` — String: `"1M - 50M"`

**Config File:**
`ReplicatedStorage.Config.LuckMachineData`

**Action:**
Define the drops for the new machine's `ConfigName`.

**Code Example:**

```lua
["Zone9_LuckMachine"] = {
    Cost = 1000000,
    Items = {
        {Name = "Omega Blackhole", BaseChance = 50},
        {Name = "Galactic Rift", BaseChance = 5}
    }
}
```

---

## 3.8 MONETIZATION: HOW TO ADD NEW DEV PRODUCTS & GAMEPASSES

**Config File:**
`ReplicatedStorage.Config.MonetizationData`

### Action 1 — Add the ID

Add the ID to the `DeveloperProducts` or `Gamepasses` table.

### Action 2 — Product Reward

**If Product:** Define the specific reward in the `ProductRewards` table.

**Code Example:**

```lua
[12345678] = {
    RewardType = "Level",
    Amount = 100
},
```

### Action 3 — Gamepass Reward

**If Gamepass:** Open the `MonetizationService` ServerScript and update `grantPassReward` to give the player the specific Attribute (e.g., `Pass_NewPass`).

### Action 4 — UI

Open the `ShopAndBoostController` LocalScript and bind the UI button to prompt the purchase using:

```lua
bindBuyButton("ButtonName", ID, true/false)
```

---

# 📌 Quick Reference

| System                              | Location / Config                              | Streaming    |
| ----------------------------------- | ---------------------------------------------- | ------------ |
| Destructibles                       | `Workspace.Areas_Items`                        | `Atomic`     |
| Planets                             | `Workspace.Planets`                            | `Atomic`     |
| Luck Machines                       | `Workspace.LuckMachines`                       | `Persistent` |
| Global Leaderboards                 | Workspace                                      | `Persistent` |
| Spaceship                           | Workspace                                      | `Persistent` |
| Base/Zone Spawns                    | Workspace                                      | `Persistent` |
| Doors                               | `Workspace.Doors`                              | `Persistent` |
| Invisible Walls / Map / RandomStuff | Workspace                                      | `Default`    |
| Level & XP                          | `ReplicatedStorage.Config.LevelConfig`         | —            |
| Rebirths                            | `ReplicatedStorage.Config.RebirthData`         | —            |
| Blackholes                          | `ReplicatedStorage.Config.BlackholeData`       | —            |
| Destructibles Config                | `ReplicatedStorage.Config.DestructiblesConfig` | —            |
| Zones                               | `ReplicatedStorage.Config.ZoneData`            | —            |
| Earths                              | `ReplicatedStorage.Config.EarthConfig`         | —            |
| Luck Machines                       | `ReplicatedStorage.Config.LuckMachineData`     | —            |
| Monetization                        | `ReplicatedStorage.Config.MonetizationData`    | —            |
