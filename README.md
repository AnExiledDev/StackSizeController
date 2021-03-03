**Description looked better in my notepad. Improvements to readability to come soon.**
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
##### **Usage:** `stacksizecontroller.itemsearch <full or partial item name>`
##### **Parameter #1:** `full or partial item name` Can be any length, however I suggest you use 2, 3 or more characters to avoid potential slowdowns.
##### **Usage Example:** `stacksizecontroller.itemsearch pic` (Result pictured below)

##### Searches item display names, descriptions, and shortnames for all or partial results and returns them in an organized list.

![itemsearch example](https://i.imgur.com/yOUagsF.png)

----

### **stacksizecontroller.listcategories**
##### **Usage:** `stacksizecontroller.listcategories`
##### **Parameters:** `No Parameters`
##### **Usage Example:** `stacksizecontroller.listcategories` (Result pictured below)

![listcategories example](https://i.imgur.com/CsbGcSB.png)

----

### **stacksizecontroller.listcategoryitems**
##### **Usage:** `stacksizecontroller.listcategoryitems <exact category name>`
##### **Parameter #1:** `exact category name` Must be an exact category name. Use stacksizecontroller.listcategories for help.
##### **Usage Example:** `stacksizecontroller.listcategoryitems Weapons`

##### Output matches `stacksizecontroller.listcategoryitems`

----

### **stacksizecontroller.setstack**
##### **Usage:** `stacksizecontroller.setstack <item shortname or id> <stack limit or multiplier>`
##### **Parameter #1:** `Shortname or ID` Use stacksizecontroller.itemsearch if you need help.
##### **Parameter #2:** `Stack limit or multiplier` Supplying just a number like "2000" sets that as the max stack limit. Supplying a number immediately followed by an x sets a multiplier, like "20x". Entering "20 x" would cause an error.
##### **Usage Example:** `stacksizecontroller.setstack generator.wind.scrap 5` (Hard limit) or `stacksizecontroller.setstack wood 20x` (Multiplier)

##### Updates configuration file, adding or updating the hard limit list or the multiplier list.

----

### **stacksizecontroller.setstackcat**
##### **Usage:** `stacksizecontroller.setstackcat <category name> <stack multiplier>`
##### **Parameter #1:** `category name` Use stacksizecontroller.listcategories if you need help. (Not case sensitive)
##### **Parameter #2:** `Stack multiplier` Unlike setstack this only accepts a multiplier, it does not require and will error if it's provided with a non-numeric character.
##### **Usage Example:** `stacksizecontroller.setstackcat resources 20`

##### Updates configuration file, changing the specified categories multiplier.

----

### **stacksizecontroller.setallstacks**
##### **Usage:** `stacksizecontroller.setallstacks <stack multiplier>`
##### **Parameter #1:** `Stack multiplier` Unlike setstack this only accepts a multiplier, it does not require and will error if it's provided with a non-numeric character.
##### **Usage Example:** `stacksizecontroller.setallstacks 10`

##### Updates configuration file, changing every category to the defined multiplier.

----

### **stacksizecontroller.regendatafile**
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
  "GlobalStackMultiplier": 1,
  "CategoryStackMultipliers": {
    "Weapon": 1,
    "Construction": 1,
    "Items": 1,
    "Resources": 1,
    "Attire": 1,
    "Tool": 1,
    "Medical": 1,
    "Food": 1,
    "Ammunition": 1,
    "Traps": 1,
    "Misc": 1,
    "All": 1,
    "Common": 1,
    "Component": 1,
    "Search": 1,
    "Favourite": 1,
    "Electrical": 1,
    "Fun": 1
  },
  "IndividualItemStackMultipliers": {},
  "IndividualItemStackHardLimits": {},
  "VersionNumber": {
    "Major": 3,
    "Minor": 0,
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
  "GlobalStackMultiplier": 1,
  "CategoryStackMultipliers": {
    "Weapon": 1,
    "Construction": 1,
    "Items": 5,
    "Resources": 12,
    "Attire": 1,
    "Tool": 1,
    "Medical": 1,
    "Food": 1,
    "Ammunition": 1,
    "Traps": 1,
    "Misc": 1,
    "All": 1,
    "Common": 1,
    "Component": 1,
    "Search": 1,
    "Favourite": 1,
    "Electrical": 1,
    "Fun": 1
  },
  "IndividualItemStackMultipliers": {
    "-566907190": 10
  },
  "IndividualItemStackHardLimits": {
    "-586342290": 3
  },
  "VersionNumber": {
    "Major": 3,
    "Minor": 0,
    "Patch": 0
  }
}
```

- `RevertStackSizesToVanillaOnUnload` - If true; item stacksizes are returned to vanilla defaults on plugin unload.
- `AllowStackingItemsWithDurability` - If enabled, items with durability such as weapons can be stacked if they are at full durability. If disabled items with durability can't be stacked at all. (Contents, attachments and ammo are all returned to the player)
- `HidePrefixWithPluginNameInMessages` - Currently does nothing. Future version will hide the prefix from chat messages in-game.
- `GlobalStackMultiplier` - Multiplies all item stacks by this value.
- `CategoryStackMultipliers` - Each category will multiply stacks for those items by the defined amount.
- `IndividualItemStackMultipliers` - Accepts "item_id": multiplier. Use stacksizecontroller.itemsearch to find the item id easily.
- `IndividualItemStackHardLimits` - Accepts "item_id": hard limit. Use stacksizecontroller.itemsearch to find the item id easily.