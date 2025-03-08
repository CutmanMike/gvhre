# Creating Classes

Creating classes for GVH:RE is straight forward, assuming you already know how to create and add a playerclass already. There are a few additional steps to set up your class so it is available in the class selector, and to have traits available for it.

## ACS

You will need at least one ACS script loaded via **LOADACS**. That script needs to #include **GVHADD.acs** to access the trait and class registering functions. If you're doing this in slade, copy **GVHADD.acs** from the GVH:RE pk3 file into the same directory you will have your other .acs files. You *do not* need to compile this file, it just needs to be available for your script compiler. Once you've set that up, create your new acs text file. It should have the following at the top:

```
#library "MYCLASS"
#include "zcommon.acs"
#include "GVHADD.acs"
```

### Registering your class

You need an OPEN script that registers your class and the traits it will get. You can use the functions **RegisterClass** and **RegisterTrait** that we get access to by including **GVHADD.acs** to do this. 

- **RegisterClass(str playeractor, int team)** - The player actor name is the actor of the playerclass. Team is which team they belong to, which should be either GVHADD_HUMAN_TEAM or GVHADD_GHOUL_TEAM.
- **RegisterTrait(str playeractor, str traitactor)** - The player actor name is the actor of the playerclass. Traitactor is the trait inventory item (see [Traits.md](Traits.md)).

You will also need a copy of this script to execute for the clients using an OPEN CLIENTSIDE script. Here is an example the scripts that registers a class:

```
script "MyRegisterScript" OPEN
{
  // Safety delay. Ensures the base classes have the correct order.
  Delay(1);
  
  // Register the class and what team they represent.
  // GVHADD_HUMAN_TEAM = Huamns, GVHADD_GHOUL_TEAM = Ghouls
  RegisterClass("DummyPlayer", GVHADD_HUMAN_TEAM);
  
  // Now to register the traits with the RegisterTrait function.
  // The first argument is the player class you are assigning the trait to. The second argument is the trait inventory item.
  
  RegisterTrait("DummyPlayer", "Trait_Alpha");
  RegisterTrait("DummyPlayer", "Trait_Beta");
  RegisterTrait("DummyPlayer", "Trait_Gamma");
  RegisterTrait("DummyPlayer", "Trait_Delta");
  RegisterTrait("DummyPlayer", "Trait_Epsilon");
  RegisterTrait("DummyPlayer", "Trait_Zeta");
  RegisterTrait("DummyPlayer", "Trait_Eta");
  RegisterTrait("DummyPlayer", "Trait_Theta");
  RegisterTrait("DummyPlayer", "Trait_Iota");
  RegisterTrait("DummyPlayer", "Trait_Kappa");
  RegisterTrait("DummyPlayer", "Trait_Lambda");
}

// IMPORTANT! This script also needs to be run for the client!

script "MyRegisterScript_Client" OPEN CLIENTSIDE
{
    if(IsNetworkGame())
    {
        ACS_NamedExecuteWithResult("MyRegisterScript");
    }
}
```

### Setting up the HUD

In ACS again, you will need to define what GVH:RE will display on the HUD. You may notice the absense of SBARINFO in this mod. This is because the entire HUD is handled via ACS and user cvars. You can use another acs file if you wish, as long as it #includes **GVHADD.acs** again. This time we will be using GVHADD's **UpdateHUD** function to maniuplate the HUD.

Your HUD script MUST be an named script, and must use the pattern "GVH_LoadHud_(CLASSNAME HERE)" and take one argument. **UpdateHud** takes a string element to choose which element of the HUD to manipulate, and a string for what to put in that element. The HUD elements are listed below:

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
  UpdateHud("gvh_hud_wep_1", "DummyChaingun");
  
  // Regular Fire
  UpdateHud("gvh_hud_wepicon_1", "DUMWEPI");
  UpdateHud("gvh_hud_wepammotype_1", "DummyClip");
  
  // Altfire
  UpdateHud("gvh_hud_wepalticon_1", "DUMWEPA");
  UpdateHud("gvh_hud_wepaltammotype_1", "DummyRocket");
  
  // Inventory use item, and what to display as the counter for it.
  UpdateHud("gvh_hud_useitem", "");
  UpdateHud("gvh_hud_itemcounter", "");
  
  // By default, weapon slots on the hud (the icons next to 1, 2, 3 etc)
  // Will use the wepicon defined above. You can override them here if needed.
  // For example, the Creeper's ventriloquism trait uses this to show what
  // sounds you will make for each weapon slot.
  UpdateHud("gvh_hud_sloticon_1", "DUMWEPA");
  
  
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

## DECORATE

You will need to crate an automap marker for your class. It should be a GVH_MapMarker class that stops after 2 tics. You can copy the example below to easily create one for your class. The sprite should be a representation of your class to appear on the automap.

```
actor MapMarker_DummyPlayer : GVH_MapMarker
{
  states
  {
  Spawn:
  DUMM A 2
  stop
  }
}
```

## Next steps

Assuming you've set everything up correctly, your class is ready to be inserted into the game.

If you run the game and find that in TLMS the class selector isn't showing at all anymore, it is because you have not defined any traits for it yet. In the current version of GVH:RE, it will only offer traits if at least 2 are available for **every** class each round. If you defined one with no traits, there won't be any traits to offer anyone!

Check the [Traits.md](Traits.md) page to learn how to create traits for your class.
