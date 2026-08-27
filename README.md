# 1. DATA STORAGE SAFE LIMITS (THE 9 QUADRILLION CAP) Do keep it in mid in scrap and xp 

# 2. WORKSPACE FOLDER MAPPING (HOW TO ADD WORLD ITEMS)

[AREAS_ITEMS] (Atomic) - Destructibles (Trees, Cars)
1. Add a Model. 
2. Inside Model: Add an invisible BasePart named `HitBox`.
3. Inside HitBox: Add String Attribute `ItemType` (e.g., "Tree").
4. Inside HitBox: Add Attachments named `Damage` and `Health`.

[PLANETS] (Atomic) - Multiplier Earths
1. Add a Model named exactly `Earth X`.
2. Inside Model: Add BasePart named `HitBox`.
3. Inside HitBox: Add Attachment named `Damage`.

[LUCK MACHINES] (Persistent) - Gacha Rollers
1. Add a Model named `LuckMachine[ZoneNumber]`.
2. Inside Model: Add Attachment `PrompDisplay` and BasePart `Spawn`.
3. Inside Model: Add Attributes: `Cost` (Number), `ConfigName` (String), `LuckDisplay` (String).

[GLOBAL LEADERBOARDS] (Persistent)
1. Duplicate existing board.
2. Rename Model to the exact DataStore key (e.g., `TotalKillsData`).

[SPACESHIP] (Persistent) - Shop Prompts
1. Add a Model named `Spaceship_Stage[X]`.
2. Inside Model: Add Attachment named `Attachment`.

[BASESPAWNS & ZONESPAWNS] (Persistent) - Teleporters
1. Add an invisible, anchored BasePart named `Spawn_[ZoneNumber]`.

[DOORS] (Persistent) - Zone Barriers
1. Add a Model named `Door[ZoneNumber]`.
2. Inside Model: Add a physical BasePart named `Part` to block the path.

[INVISIBLE WALLS, MAP, RANDOMSTUFF] (Default)
* No scripting required. Drop raw meshes, parts, or models here.

# 3. PROGRESSION SYSTEMS

[LEVELS & XP]
* File: ReplicatedStorage.Config.LevelConfig
* Action: Add new lines to the `XPRequirements` table.
* Code: 
  [150] = 180000000000000,

[REBIRTH TIERS]
* File: ReplicatedStorage.Config.RebirthData
* Action: Add the next sequential index.
* Code:
  [7] = {LevelReq = 200, Multiplier = 4.5},

[INDEX / POKEDEX LOGIC]
* Setup: Add `Discovered_BlackHoles = ""` to `DefaultData.Items`.
* Server Logic (In FunctionHandler.AddOwned):
  local discovered = player:GetAttribute("Discovered_BlackHoles")
  if not string.find(discovered, blackHoleName, 1, true) then
      player:SetAttribute("Discovered_BlackHoles", discovered .. blackHoleName .. ",")
  end
* UI Logic: Split the `Discovered_BlackHoles` string and highlight UI frames for items found.

[SKILLS / UPGRADES LOGIC]
* Setup: Add `SkillPoints = 0` to `DefaultData.Attributes`. Add `Unlocked_Skills = ""` to `DefaultData.Items`.
* Config: Create `ReplicatedStorage.Config.SkillsData`.
  return {
      ["Double_Strike"] = {Cost = 5, Multiplier = 2, Type = "Damage"}
  }
* Logic: In `StatesService`, check the `Unlocked_Skills` string. If they own "Double_Strike", multiply damage by 2.

# 4. ECONOMY & ITEMS

[ADDING NEW BLACKHOLES]
1. Model: Duplicate a Blackhole in `ServerStorage.Models.Blackhole`. Rename it. Keep `HitBox` and `Main` parts.
2. Config: Open `ReplicatedStorage.Config.BlackholeData` and add stats:
   ["Galactic Rift"] = {Damage = 15000000, OrbitDistance = 7, OrbitSpeed = 3, Scale = 1, Rarity = "Mythic"},

[ADDING NEW LUCK MACHINE CONFIGS]
* File: ReplicatedStorage.Config.LuckMachineData
* Action: Create a config matching the `ConfigName` Attribute on the workspace machine.
* Code:
  ["Zone9_LuckMachine"] = {
      Cost = 5000000,
      Items = {
          {Name = "Nebula Core", BaseChance = 10},
          {Name = "Galactic Rift", BaseChance = 50}
      }
  }

[ADDING NEW DEVIP PRODUCTS & GAMEPASSES]
1. IDs: Open `ReplicatedStorage.Config.MonetizationData`. Add IDs to `DeveloperProducts` or `Gamepasses`.
2. Product Rewards: Define yield in the `ProductRewards` table:
   [987654321] = { RewardType = "Scrap", Amount = 5000000 },
3. Gamepass Rewards: Open `MonetizationService`. Add an `elseif` branch in `grantPassReward()` to apply a permanent Attribute (e.g., `Pass_NewPass`).
4. UI: Bind the button in your ShopUI using `MarketplaceService`.
