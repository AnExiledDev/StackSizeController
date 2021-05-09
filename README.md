## Feature Highlights
* Allows setting stack sizes for nearly every item in Rust.
* Items are catagorized and automatically populated in the data file.
* Stacks can easily be modified globally, by category or individually in the configuration file. If you want complete control over every item you can modify each item individually in the data file.
* Several quality of life commands allowing you to **search all items** and list categories.
* Item search displays vanilla stack rate as well as custom stack rate after multipliers.


## Quick Important Notes
* If stack sizes are modified in the data file (at /oxide/data/StackSizeController.json) defined hard limits will override them, whereas defined multipliers will multiply that base value.
* Stacking an item over 2,147,483,647 will cause an error when loaded and will not stack the item at that number. 2,147,483,647 is the max for stack sizes for all stack size plugins.


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

### **stacksizecontroller.regendatafile**
##### **Permission:** `stacksizecontroller.regendatafile` (Only needed if used in-game)
##### **Usage:** `stacksizecontroller.regendatafile`
##### **Parameters:** `No Parameters`
##### **Usage Example:** `stacksizecontroller.regendatafile`

##### Wipes the data file and regenerates the item cache. (Note: Item cache is automatically maintained on plugin initialization.)


## Configuration

### Default Configuration
```json
{
  "RevertStackSizesToVanillaOnUnload": true,
  "AllowStackingItemsWithDurability": true,
  "HidePrefixWithPluginNameInMessages": false,
  "DisableDupeFixAndLeaveWeaponMagsAlone": false,
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
  "CategoryStackHardLimits": {
    "Weapon": 0,
    "Construction": 0,
    "Items": 0,
    "Resources": 0,
    "Attire": 0,
    "Tool": 0,
    "Medical": 0,
    "Food": 0,
    "Ammunition": 0,
    "Traps": 0,
    "Misc": 0,
    "All": 0,
    "Common": 0,
    "Component": 0,
    "Search": 0,
    "Favourite": 0,
    "Electrical": 0,
    "Fun": 0
  },
  "IndividualItemStackHardLimits": {},
  "VersionNumber": {
    "Major": 3,
    "Minor": 2,
    "Patch": 0
  }
}
```

### Configuration Example
```json
{
  "RevertStackSizesToVanillaOnUnload": true,
  "AllowStackingItemsWithDurability": true,
  "HidePrefixWithPluginNameInMessages": false,
  "DisableDupeFixAndLeaveWeaponMagsAlone": false,
  "GlobalStackMultiplier": 1,
  "CategoryStackMultipliers": {
    "Weapon": 10.0,
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
  "IndividualItemStackMultipliers": {
    "-566907190": 10
  },
  "CategoryStackHardLimits": {
    "Weapon": 1,
    "Construction": 0,
    "Items": 0,
    "Resources": 50000,
    "Attire": 0,
    "Tool": 0,
    "Medical": 15,
    "Food": 0,
    "Ammunition": 0,
    "Traps": 0,
    "Misc": 0,
    "All": 0,
    "Common": 0,
    "Component": 0,
    "Search": 0,
    "Favourite": 0,
    "Electrical": 0,
    "Fun": 0
  },
  "IndividualItemStackHardLimits": {
    "-586342290": 3
  },
  "VersionNumber": {
    "Major": 3,
    "Minor": 2,
    "Patch": 0
  }
}
```

- `RevertStackSizesToVanillaOnUnload` - If true; item stacksizes are returned to vanilla defaults on plugin unload.
- `AllowStackingItemsWithDurability` - If enabled, items with durability such as weapons can be stacked if they are at full durability. If disabled items with durability can't be stacked at all. (Contents, attachments and ammo are all returned to the player)
- `HidePrefixWithPluginNameInMessages` - Currently does nothing. Future version will hide the prefix from chat messages in-game.
- `DisableDupeFixAndLeaveWeaponMagsAlone` - Disables the dupe fix, which removes ammo from weapons when stacking, with this disabled players can dupe any ammo slowly. 
- `GlobalStackMultiplier` - Multiplies all item stacks by this value.
- `CategoryStackMultipliers` - Each category will multiply stacks for those items by the defined amount.
- `IndividualItemStackMultipliers` - Accepts "item_id": multiplier. Use stacksizecontroller.itemsearch to find the item id easily.
- `CategoryStackHardLimits` - Each item in this category will be set to this hard stack limit, if the value is above 0.
- `IndividualItemStackHardLimits` - Accepts "item_id": hard limit. Use stacksizecontroller.itemsearch to find the item id easily.

## Data

### StackSizeController.json
- Each item is separated into different categories which are generated by the game. Everything in this file is auto generated and automatically maintained.
- The only value in here not overriden are the CustomStackSize values. You can modify these for fine-tuning stack sizes of every item in the inventory or storages.
- You do not need to modify this file unless you need more control than the configuration gives you. Modifying this value overrides every config file definition EXCEPT IndividualItemStackHardLimits.
- **Setting "CustomStackSize" to any value other than 0 will override vanilla defaults. (As of v3.1.3)**

#### Datafile Example
```json    
"Resources": [
   {
     "ItemId": 996293980,
     "Shortname": "skull.human",
     "HasDurability": false,
     "VanillaStackSize": 1,
     "CustomStackSize": 0
   },
   {
     "ItemId": 204391461,
     "Shortname": "coal",
     "HasDurability": false,
     "VanillaStackSize": 1,
     "CustomStackSize": 0
   },
   {
     "ItemId": -1018587433,
     "Shortname": "fat.animal",
     "HasDurability": false,
     "VanillaStackSize": 1000,
     "CustomStackSize": 0
   },
   {
     "ItemId": 609049394,
     "Shortname": "battery.small",
     "HasDurability": false,
     "VanillaStackSize": 1,
     "CustomStackSize": 0
   },
   {
     "ItemId": 1719978075,
     "Shortname": "bone.fragments",
     "HasDurability": false,
     "VanillaStackSize": 1000,
     "CustomStackSize": 0
   },
   {
     "ItemId": 634478325,
     "Shortname": "cctv.camera",
     "HasDurability": false,
     "VanillaStackSize": 64,
     "CustomStackSize": 0
   },
   {
     "ItemId": -1938052175,
     "Shortname": "charcoal",
     "HasDurability": false,
     "VanillaStackSize": 1000,
     "CustomStackSize": 0
   },
   (... continued)
]
```

## Developer Hooks

#### OnVendorHeliFuelAdjust

- Called when a heli has spawned at a vendor, and this plugin is about to reset its fuel amount to 100
- Returning `false` will prevent the fuel from being adjusted
- Returning `null` will result in the default behavior

```csharp
bool? OnVendorHeliFuelAdjust(MiniCopter heli)
```
