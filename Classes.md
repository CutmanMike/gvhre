# Creating Classes

Creating classes for GVH:RE is straight forward, assuming you already know how to create and add a playerclass already. There are a few additional steps to set up your class so it is available in the class selector, and to have traits available for it.

## ACS

You will need at least one ACS script loaded via **LOADACS** that imports **TRAITSDB**. If you're doing this in slade, copy **TRAITSDB.acs** from the GVH:RE pk3 file into the same directory you will have your other .acs files. You do not need to compile this file, it just needs to be available for your script compiler. Once you've set that up, create your new acs text file. It should have the following at the top:

```
#library "MYCLASS"
#include "zcommon.acs"
#import "TraitsDB.acs"
```

### Registering your class

You need an OPEN script that registers your class and the traits it will get. You can use the functions **RegisterClass** and **RegisterTrait** that we import from TraitsDB to do this. 

- **RegisterClass(str playeractor, int team)** - The player actor name is the actor of the playerclass. Team is which team they belong to (0 humans, 1 ghouls).
- **RegisterTrait(str playeractor, str traitactor, str unused)** - The player actor name is the actor of the playerclass. Traitactor is the trait inventory item (see Traits.md). The last argument is currently unused.

Here is an example of an OPEN script that registers a class:

```
script "MyRegisterScript" OPEN
{
// Safety delay. Ensures the base classes have the correct order.
Delay(1);

// Register the class and what team they represent.
// 0 = Huamns, 1 = Ghouls
RegisterClass("DummyPlayer", 0);

// Now register the traits.
// The second argument is the trait inventory item. The third is currently unused, so leave it as an empty string.
RegisterTrait("DummyPlayer", "Trait_Alpha", "");
RegisterTrait("DummyPlayer", "Trait_Beta", "");
RegisterTrait("DummyPlayer", "Trait_Gamma", "");
RegisterTrait("DummyPlayer", "Trait_Delta", "");
RegisterTrait("DummyPlayer", "Trait_Epsilon", "");
RegisterTrait("DummyPlayer", "Trait_Zeta", "");
RegisterTrait("DummyPlayer", "Trait_Eta", "");
RegisterTrait("DummyPlayer", "Trait_Theta", "");
RegisterTrait("DummyPlayer", "Trait_Iota", "");
RegisterTrait("DummyPlayer", "Trait_Kappa", "");
RegisterTrait("DummyPlayer", "Trait_Lambda", "");
}
```

### Setting up the HUD

In ACS again, you will need to define what GVH:RE will display on the HUD. You may notice the absense of SBARINFO in this mod. This is because the entire HUD is handled via ACS and user cvars.

Your HUD script MUST be an named script, and must use the pattern "GVH_LoadHud_(CLASSNAME HERE)" and take one argument. The rest of the script will be setting user cvars to maniuplate the HUD. The following CVARs must be set using SetUserCvarString, affecting the ```ConsolePlayerNumber()```.

- **gvh_hud_wep_1** - The weapon actor used for slot 1.
- **gvh_hud_wepicon_1** - The graphic displayed for the weapon in slot 1's primary fire.
- **gvh_hud_wepammotype_1** - The ammo actor used for the weapon in slot 1's primary fire.
- **gvh_hud_wepalticon_1** - The graphic displayed for the weapon in slot 1's alt fire.
- **gvh_hud_wepaltammotype_1** - The ammo actor used for the weapon in slot 1's alt fire.
- **gvh_hud_sloticon_1** - The graphic displayed for the weapon in slot 1 in the weapon slot list. If blank, will use the same as gvh_hud_wepicon_1.

You can change the number for the above to affect different weapon slots.

- **gvh_hud_useitem** - The grahpic displayed for the inventory (use item) actor.
- **gvh_hud_itemcounter** - The actor for the inventory (use item) actor's counter. Leave blank to imply infinite use.

Here is a full example of script.

```
script "GVH_LoadHud_DummyPlayer" (int tid)
{
// Slot 1 weapon setup.
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_wep_1", "DummyChaingun");

// Regular Fire
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_wepicon_1", "DUMWEPI");
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_wepammotype_1", "DummyClip");

// Altfire
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_wepalticon_1", "DUMWEPA");
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_wepaltammotype_1", "DummyRocket");

// Inventory use item, and what to display as the counter for it.
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_useitem", "");
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_itemcounter", "");

// By default, weapon slots on the hud (the icons next to 1, 2, 3 etc)
// Will use the wepicon defined above. You can override them here if needed.
// For example, the Creeper's ventriloquism trait uses this to show what
// sounds you will make for each weapon slot.
SetUserCvarString(ConsolePlayerNumber(), "gvh_hud_sloticon_1", "CWEAP11");


// You can use conditional statements to check for inventory (traits)
// to modify the hud for different traits.
// For more examples see HUDREG.acs in the base GVHRE pk3.
}
```
## LANGUAGE

There are a few entries you will need to provide in a **LANGUAGE** lump.

- **CLASS_NAME_(CLASS NAME HERE)** - The name of your class.
- **CLASS_SELECT_(CLASS NAME HERE)** - The graphic of your actor displayed on the class selector while it is selected.
- **CLASS_UNSELECT_(CLASS NAME HERE)** - The graphic of your actor displayed on the class selector while it is not selected.

Here is an example of a LANGUAGE lump:

```
[enu default]

CLASS_NAME_DUMMYPLAYER = "DUMMY";

CLASS_SELECT_DUMMYPLAYER = "SELIDUM";
CLASS_UNSELECT_DUMMYPLAYER = "SELUDUM";
```

There are more items to add to the LANGUAGE lump, but that will be covered in the traits guide!

## Next steps

Assuming you've set everything up correctly, your class is ready to be inserted into the game.

If you run the game and find that in TLMS the class selector isn't showing at all anymore, it is because you have not defined any traits for it yet. In the current version of GVH:RE, it will only offer traits if at least 2 are available for **every** class. If you defined one with no traits, there won't be any traits to offer anyone!

Check the Traits.md page to learn how to create traits for your class.
