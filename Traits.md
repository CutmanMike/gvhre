# Creating Traits

Creating traits for your classes involves a little bit of DECORATE, and possibly some ACS depending on what you want to do for each trait. Simply put, traits are inventory items. The backend handles when the inventory items are distributed and manipulated, while the modder just needs to worry about what the classes need to do while they have these inventory items.

## DECORATE

The main trait actor should be in the format of **Trait_(TRAIT NAME HERE)**. It should inherit from BaseTrait, just to make it easier for modders to view. Here's an example trait:

```
actor Trait_MyCoolTrait : BaseTrait {}
```

And that's literally it! Your trait is ready to be given out by the system (assuming it has been registered, see [Classes.md](Classes.md)). Of course, this trait does nothing currently. What it does is up to the modder. You can use A_JumpIfInventory in weapons, CheckInventory in ACS etc. There are a few more actors you can create that the system will recognize if needed, but the top level **Trait_(TRAIT NAME HERE)** actor MUST exist.

### Active Trait

Creaitng a _A suffixed actor for your trait will cause the icon on the HUD to glow while the player owns it. This is useful for showing the player that the trait's ability is active, or has just triggered. It can inherit from TraitActive, which is a Powerup, or any Inventory item. Powerups are especially useful as you can set a powerup.duration to have the HUD icon to glow for a limited time.

```
actor Trait_MyCoolTrait_A : TraitActive // (can be changed to inherit from Powerup or any Inventory type)
{
powerup.duration 35
}
```

### Triggered Trait

A _T suffixed actor will automatically be given to players once they are given the trait. This is very useful for any extra setup steps, such as giving the class new weapons or executing ACS scripts. TriggeredTrait is a CustomInventory class you can inherit from with a Pickup state to perform your actions.

```
actor Trait_MyCoolTrait_T : TriggeredTrait 
{
  states
  {
  Pickup:
  TNT1 A 0 ACS_NamedExecuteWithResult("MyCoolTraitScript")
  stop
  }
}
```

### Cooldown Trait

A _CD suffixed trait must be created if you intend to use the ACS function **TraitCooldown** in **FUNC.acs**. This displays a little cooldown overlay for your trait on the HUD when called.

```
actor Trait_MyCoolTrait_CD : TraitCooldown {}
```

### Disabled Trait

A _D suffixed actor will show your trait is disabled on the HUD with a dark overlay. Give this to your player if your trait will ever enter a permanent disabled state.

```
actor Trait_MyCoolTrait_D : BaseTrait {}
```

### Destroyed Trait

A _X suffixed actor will automatically be given to players if their trait is destroyed (via another trait or game mechanic). This is important to include if your trait needs to undo or stop any scripts from running to revert the player back to normal. You can inherit from TraitRemoval, which is a CustomInventory

```
actor Trait_MyCoolTrait_X : TraitRemoval
{
  states
  {
  Pickup:
  TNT1 A 0 A_TakeInventory("CoolTraitSuperPower", 1)
  stop
  }
}
```
## LANGUAGE

There are a few strings you will need to add to a **LANGUAGE** lump to finalize your trait setup.

- **TRAIT_(TRAIT NAME HERE)_NAME** - The name of your trait. It doesn't need to match the class name exactly.
- **TRAIT_(TRAIT NAME HERE)_DESC** - The description of what your trait does.
- **TRAIT_(TRAIT NAME HERE)_ICON** - The graphic to use for your trait icon.

Try to keep your trait description within 6 lines to make it fit nicely in the trait selector box! Here's an example LANGUAGE lump:

```
TRAIT_ALPHA_NAME = "Alpha Strike";
TRAIT_ALPHA_DESC = "This is the description for\n\nthe first trait.\n\n\cv- Something -";
TRAIT_ALPHA_ICON = "T_DUMM1";
```

## ACS

Your traits need to also be registered via an ACS open script, but this is covered in [Classes.md](Classes.md) as you can register your class at the same time.

## Next Steps

Now that you've got a trait inventory item ready, it's time to decide how your class should use this trait. You can do something basic such as doing an A_JumpIfInventory to check if a player owns Trait_(TRAIT NAME HERE) to perform an alternative action, or use a TriggeredTrait to execute an ACS script that does all the work.

How many traits you design is up to you. However, as noted in [Classes.md](Classes.md), if a round starts and there are no more traits avaiable to offer any existing class, no more traits will be offered for that match. You should try to strive for at least **10** traits per class, as that covers the possibility of both teams winning two rounds, thus 5 x 2 traits being offered. GVH:RE currently has 11 traits for each of the base classes for variety, but feel free to make even more if you're feeling extra creative!
