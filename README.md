## Feature Highlights
* Allows setting stack sizes for nearly every item in Rust.
* Items are automatically populated in the configuration file.
* Stacks can easily be modified globally, by category or individually in the configuration file.
* Several quality of life commands allowing you to **search all items** and list categories.
* Item search displays vanilla stack rate as well as custom stack rate after multipliers.
* Stateful/container items (water containers, etc.) are protected from resizing so they don't dupe or lose their contents.


## Quick Important Notes
* Running the plugin once generates items in configuration for IndividualItemStackSize from vanilla defaults. This list is automatically updated when new items are detected, and a notification is put in the console.
* Multipliers multiply IndividualItemStackSize definitions not vanilla stack size. Individual multipliers take priority over category stack multipliers.
* This fork uses a local **vanilla defaults datafile** (`oxide/data/StackSizeController_vanilla-defaults.json`) as the authoritative source of vanilla stack sizes. By default (`UpdateVanillaDefaultsFromRepo: false`) this file is **not** overwritten from the GitHub repo on load — the repo copy is stale and missing items added by newer Rust updates, and overwriting it caused those items to lose their vanilla anchor. The repo is only fetched once to bootstrap when no local datafile exists, or on every load if you set `UpdateVanillaDefaultsFromRepo: true`.
* Some items are **never resized** regardless of multipliers (see "Protected Items" below).
* Stacking an item over 2,147,483,647 will cause an error when loaded and will not stack the item at that number. 2,147,483,647 is the max for stack sizes for all stack size plugins as it is a hardcoded limitation of Rust.


## Protected Items (never resized)
Certain items hold variable sub-state (a liquid amount, condition/attachments, etc.). Stacking them merges or duplicates that state, causing item dupes or content loss, so they are always kept at their vanilla stack size:

* `water`, `water.salt` — the liquid unit items.
* `waterjug`, `botabag`, `smallwaterbottle`, `bucket.water`, `gun.water`, `pistol.water` — liquid **containers** that hold a variable amount of liquid.
* `cardtable`, `hat.bunnyhat`, `rustige_egg_e` — misc items that misbehave when resized.

In addition, if `AllowStackingItemsWithDurability` is `false`, every item with durability (weapons, armor, tools — anything with condition/attachment sub-state) is forced back to its vanilla stack size. Set this to `false` if you are seeing armor/weapons dupe their attachments or slotted items when stacked.


## Installation Instructions
* Put plugin in oxide/plugins.
* Start server and wait for StackSizeController to be loaded.
* Open the configuration and modify settings as needed. Setting individual stack sizes is done in the configuration NOT datafile in value IndividualItemStackSize which is generated on plugin load.
* Run o.reload StackSizeController in console to set configured stack sizes.

### Keeping vanilla defaults current after a Rust update
Because the repo defaults file is stale, the recommended way to pick up items added by a game update is to regenerate the local datafile from your own server:

1. Temporarily set `GlobalStackMultiplier` and every `CategoryStackMultipliers` entry to `1` (so nothing is modified away from vanilla), then reload the plugin.
2. Run `stacksizecontroller.vd` in the console. This reverts stacks to vanilla, writes every current item definition's stack size to `StackSizeController_vanilla-defaults.json`, then restores your custom sizes.
3. Restore your multipliers.

With `UpdateVanillaDefaultsFromRepo` left at `false`, that generated file stays authoritative and won't be overwritten. Re-run after each wipe/game update to capture newly added items.


## Console Commands
### **stacksizecontroller.itemsearch**
##### **Permission:** `stacksizecontroller.itemsearch` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.itemsearch <full or partial item name>`
##### **Parameter #1:** `full or partial item name` Can be any length, however I suggest you use 2, 3 or more characters to avoid potential slowdowns.
##### **Usage Example:** `stacksizecontroller.itemsearch pic` (Result pictured below)

##### Searches item display names, descriptions, and shortnames for all or partial results and returns them in an organized list.

![itemsearch example](https://i.imgur.com/yOUagsF.png)

----

### **stacksizecontroller.listcategories**
##### **Permission:** `stacksizecontroller.listcategories` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.listcategories`
##### **Parameters:** `No Parameters`
##### **Usage Example:** `stacksizecontroller.listcategories` (Result pictured below)

![listcategories example](https://i.imgur.com/CsbGcSB.png)

----

### **stacksizecontroller.listcategoryitems**
##### **Permission:** `stacksizecontroller.listcategoryitems` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.listcategoryitems <exact category name>`
##### **Parameter #1:** `exact category name` Must be an exact category name. Use stacksizecontroller.listcategories for help.
##### **Usage Example:** `stacksizecontroller.listcategoryitems Weapons`

##### Output matches `stacksizecontroller.listcategoryitems`

----

### **stacksizecontroller.setstack**
##### **Permission:** `stacksizecontroller.setstack` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.setstack <item shortname or id> <stack limit or multiplier>`
##### **Parameter #1:** `Shortname or ID` Use stacksizecontroller.itemsearch if you need help.
##### **Parameter #2:** `Stack limit or multiplier` Supplying just a number like "2000" sets that as the max stack limit. Supplying a number immediately followed by an x sets a multiplier, like "20x". Entering "20 x" would cause an error.
##### **Usage Example:** `stacksizecontroller.setstack generator.wind.scrap 5` (Hard limit) or `stacksizecontroller.setstack wood 20x` (Multiplier)

##### Updates configuration file, adding or updating the hard limit list or the multiplier list.

----

### **stacksizecontroller.setstackcat**
##### **Permission:** `stacksizecontroller.setstackcat` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.setstackcat <category name> <stack multiplier>`
##### **Parameter #1:** `category name` Use stacksizecontroller.listcategories if you need help. (Not case sensitive)
##### **Parameter #2:** `Stack multiplier` Unlike setstack this only accepts a multiplier, it does not require and will error if it's provided with a non-numeric character.
##### **Usage Example:** `stacksizecontroller.setstackcat resources 20`

##### Updates configuration file, changing the specified categories multiplier.

----

### **stacksizecontroller.setallstacks**
##### **Permission:** `stacksizecontroller.setallstacks` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.setallstacks <stack multiplier>`
##### **Parameter #1:** `Stack multiplier` Unlike setstack this only accepts a multiplier, it does not require and will error if it's provided with a non-numeric character.
##### **Usage Example:** `stacksizecontroller.setallstacks 10`

##### Updates configuration file, changing every category to the defined multiplier.

----

### **stacksizecontroller.vd**
##### **Permission:** `stacksizecontroller.vd`
##### **Usage:** `stacksizecontroller.vd`
##### **Parameters:** `No Parameters`

##### Regenerates the local `StackSizeController_vanilla-defaults.json` datafile from the current server's item definitions, then restores your custom stack sizes. Run it (with all multipliers temporarily set to `1`) after a Rust update to capture newly added items. See "Keeping vanilla defaults current after a Rust update" above.


## Configuration

### Default Configuration
```json
{
  "RevertStackSizesToVanillaOnUnload": true,
  "AllowStackingItemsWithDurability": true,
  "HidePrefixWithPluginNameInMessages": false,
  "UpdateVanillaDefaultsFromRepo": false,
  "GlobalStackMultiplier": 1.0,
  "CategoryStackMultipliers": {
    "Weapon": 1.0,
    "Construction": 1.0,
    "Items": 1.0,
    "Resources": 1.0,
    "Attire": 1.0,
    "Tool": 1.0,
    "Medical": 1.0,
    "Food": 1.0,
    "Ammunition": 1.0,
    "Traps": 1.0,
    "Misc": 1.0,
    "All": 1.0,
    "Common": 1.0,
    "Component": 1.0,
    "Search": 1.0,
    "Favourite": 1.0,
    "Electrical": 1.0,
    "Fun": 1.0
  },
  "IndividualItemStackMultipliers": {},
  "IndividualItemStackSize": {},
  "VersionNumber": {
    "Major": 4,
    "Minor": 1,
    "Patch": 4
  }
}
```

### Configuration Example
```json
{
  "RevertStackSizesToVanillaOnUnload": true,
  "AllowStackingItemsWithDurability": true,
  "HidePrefixWithPluginNameInMessages": false,
  "UpdateVanillaDefaultsFromRepo": false,
  "GlobalStackMultiplier": 1.0,
  "CategoryStackMultipliers": {
    "Weapon": 1.0,
    "Construction": 5.0,
    "Items": 1.0,
    "Resources": 1.0,
    "Attire": 1.0,
    "Tool": 1.0,
    "Medical": 1.0,
    "Food": 1.0,
    "Ammunition": 1.0,
    "Traps": 1.0,
    "Misc": 1.0,
    "All": 1.0,
    "Common": 1.0,
    "Component": 1.0,
    "Search": 1.0,
    "Favourite": 1.0,
    "Electrical": 1.0,
    "Fun": 1.0
  },
  "IndividualItemStackMultipliers": 
  {
    "-586342290": 10,
    "ammo.pistol": 20
  },
  "IndividualItemStackSize": {
    "abovegroundpool": 1,
    "aiming.module.mlrs": 1,
    "ammo.grenadelauncher.buckshot": 24,
    "ammo.grenadelauncher.he": 12,
    "ammo.grenadelauncher.smoke": 12,
    "ammo.handmade.shell": 64,
    "ammo.nailgun.nails": 64,
    "ammo.pistol": 128,
    "ammo.pistol.fire": 128,
    "ammo.pistol.hv": 128,
    "ammo.rifle": 128,
    "ammo.rifle.explosive": 128,
    (... Continued)
  },
  "VersionNumber": {
    "Major": 4,
    "Minor": 1,
    "Patch": 4
  }
}
```

- `RevertStackSizesToVanillaOnUnload` - If true; item stacksizes are returned to vanilla defaults on plugin unload.
- `AllowStackingItemsWithDurability` - If enabled, items with durability such as weapons can be stacked if they are at full durability. If disabled items with durability can't be stacked at all. (Contents, attachments and ammo are all returned to the player.) Set to `false` if armor/weapons are duping slotted items or attachments when stacked.
- `HidePrefixWithPluginNameInMessages` - Currently does nothing. Future version will hide the prefix from chat messages in-game.
- `UpdateVanillaDefaultsFromRepo` - If `false` (default), the local `StackSizeController_vanilla-defaults.json` datafile is authoritative and is not overwritten from the GitHub repo on load (the repo copy is stale). The repo is still fetched once to bootstrap if no local datafile exists. Set to `true` to restore the old always-download-from-repo behavior.
- `GlobalStackMultiplier` - Multiplies all item stacks by this value.
- `CategoryStackMultipliers` - Each category will multiply stacks for those items by the defined amount.
- `IndividualItemStackMultipliers` - Accepts "item_id": multiplier. Use stacksizecontroller.itemsearch to find the item id easily.
- `IndividualItemStackSize` - Where you define specific stack sizes for each individual item.

## Developer Hooks

#### OnVendorHeliFuelAdjust

- Called when a heli has spawned at a vendor, and this plugin is about to reset its fuel amount to 100
- Returning `false` will prevent the fuel from being adjusted
- Returning `null` will result in the default behavior

```csharp
bool? OnVendorHeliFuelAdjust(MiniCopter heli)
```

## Changelog

### 4.1.4
- **Protected liquid containers from resizing.** Added `waterjug`, `botabag`, `smallwaterbottle`, `bucket.water`, `gun.water`, and `pistol.water` to the ignore list. Previously only the liquid unit items (`water`, `water.salt`) were protected, so the containers holding those liquids were still being resized and could dupe or lose their contents when stacked.
- **Stopped the stale repo defaults from clobbering local edits.** The local `StackSizeController_vanilla-defaults.json` datafile is now authoritative and is no longer overwritten from GitHub on every load. Added the `UpdateVanillaDefaultsFromRepo` config option (default `false`). This fixes newer items (e.g. `hazmatsuit.pilot`) that were missing from the stale repo file losing their vanilla anchor, falling back to their live (already-modified) stack size, and compounding on reload.
- **Fixed `itemsearch` / `listcategoryitems` crashing.** These commands indexed the defaults dictionary directly and threw `KeyNotFoundException` for any item missing from the file; they now use `GetVanillaStackSize()`. Incorporates [PR #27](https://github.com/AnExiledDev/StackSizeController/pull/27) by IsaiahPetrichor: null-guards on `displayName`/`displayDescription` in `itemsearch` (null on some item definitions) and a `ContainsKey` guard on the category multiplier lookup in `listcategoryitems`.
