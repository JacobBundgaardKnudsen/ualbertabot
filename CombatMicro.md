# Combat Management Logic #

Combat in UAlbertaBot is handled within `CombatCommander`:

  * `CombatCommander.update()`
    * Set a worker to scout if it is the appropriate time
    * If any of our occupied regions contain enemy units, create defense Squad to defend
    * If additional combat units are available, attack as follows:
      * If we know of a visible enemy base location, attack it
      * Otherwise, if we see visible units, attack them
      * Otherwise, if we know where an enemy building was, attack it
      * Otherwise, explore until we find a new attack target
    * `Squads.update()` - perform squad commands
      * Perform `CombatSimulation`
        * If simulation returns victory
          * Continue attacking toward current target
          * Call associated `MicroManager` sub-class for each unit type
        * If simulation returns defeat
          * Retreat Squad toward own base

If you wish to change this logic or add your own, you simply need to modify `CombatCommander.update()` to do so.

# Squad Formation & Implementation #

In the above example we see that `Squad` objects are created. A `Squad` is an object which holds the following information:

```
class Squad
{
    SquadOrder         order;                  // the order to be carried out 
    UnitVector         units;                  // the units in this squad

    MeleeManager       meleeManager;           // manager for melee units in this squad
    RangedManager      rangedManager;          // manager for ranged units in this squad
    DetectorManager    detectorManager;        // manager for detector units in this squad
    TransportManager   transportManager;       // manager for transport units in this squad

    std::map<BWAPI::Unit *, bool>   nearEnemy; // whether or not a given unit is near an enemy unit
};
```

With a `SquadOrder` consisting of

```
class SquadOrder
{
    enum { None, Attack, Defend };

    int                type;       // type of order, from enum above
    int                radius;     // the radius of action for this order
    BWAPI::Position    position;   // the position where this order is to be carried out
};
```

### Example Squad ###

  * `order = { Attack, 600, (200, 300) }`
  * `units = { Zealot, Zealot, Dragoon, Observer }`

This `Squad` has the intuitive meaning of "Take these two Zealots, Dragoon and Observer to Attack all enemy units within a radius of 600 of position (200, 300). The Zealots will have their behaviour controlled by `MeleeManager`, the Dragoons by `RangedManager`, and the `Observer` by `DetectorManager`.

Any `Squad` formed and added to `SquadData` inside `CombatCommander` will have its `update()` method called each frame, and will carry out behaviours in this fashion. If you wish to add new behaviours you need to modify `SquadOrder` and the respective `MicroManager`s.

By default, UAlbertaBot will only set one Attack squad and send it to attack the closest known enemy base.

# Unit Micro #

Unit Micro in UAlbertaBot is handled by `MicroManager` and its sub-classes. `Squad.update()` calls `Squad.setManagerUnits()` which assigns each unit in the squad to a specific `MicroManager` sub-class. For generality, UAlbertaBot contains `MicroManager` sub-classes such as `MeleeManager` and `RangedManager` rather than perform micro specific to each `UnitType`.

The default behaviour for each of these managers is as follows:

  * `MeleeManager` - Attack closest enemy unit based on a given priority
  * `RangedManager` - Attack closest enemy unit and then retreat until attack cooldown has finished (aka: kite)
  * `DetectorManager` - The first detector made will follow the primary attack `Squad` into battle. Any others will explore the map.
  * `TransportManager` - Not yet implemented.

These can be modified independently in order to implement your own unit micromanagement. You can even make your own unit-specific managers, as long as you make sure to add them to the correct managers within `Squad`.