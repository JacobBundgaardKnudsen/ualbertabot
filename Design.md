# UAlbertaBot Design & Architecture #

![http://webdocs.cs.ualberta.ca/~cdavid/ualbertabot.png](http://webdocs.cs.ualberta.ca/~cdavid/ualbertabot.png)

## Logic Flow ##

Here is the complete sequential logic flow of UAlbertaBot:

  * `UAlbertaBot.onFrame()`
    * `GameCommander.update()`
      * `WorkerManager.update()`
        * Re-allocate workers to new mineral patches if mined out
        * Send idle workers to gather minerals
        * Allocate gas workers until 3 in each refinery
        * Move build workers to building location if necessary
      * `ProductionManager.update()`
        * If event occurs which triggers a new build order
          * Clear current build order
          * Get new build order goal from `StrategyManager`
          * Set new build order from `StarcraftBuildOrderSearch`
        * If current build order contains items
          * If the highest priority item can be built
            * Build the highest priority item
      * `BuildingManager.update()`
        * Check to see if assigned workers have died en route or while constructing
        * Assign workers to the unassigned buildings and label them _planned_
        * For each _planned_ building, if the worker isn't constructing, send the command
        * Update status of buildings which have started construction
        * If we are terran and a building is under construction without a worker, assign a new one
        * Check if any buildings have completed and update data structures
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
      * `ScoutManager.update()`
        * If we know where enemy base is
          * If we are outside enemy base, go toward it
          * If we are inside enemy base, attack closest worker
            * If enemy worker attacks back, retreat
        * If we don't know where enemy base is
          * Visit base location least recently visited until found
      * `InformationManager.update()`
        * For each enemy unit we can see, update its last known position and unit details
        * For each enemy building we see, update it as a known enemy base location
        * If we see enemy detectors, make note
        * If we see enemy cloaked units, make note
      * `MapGrid.update()`
        * For each occupied grid space, update its time last visited
      * `MapTools.update()`
        * N/A - `MapTools` is only polled for path finding details
      * `StarcraftBuildOrderSearchManager.update()`
        * If a goal is given, perform search for a given time limit

## Motivation ##

UAlbertaBot was designed to have an intuitive hierarchical structure based on proven military command systems. This structure is beneficial for several reasons:
  * Intuitive classes for ease of interpretation
    * ex: `ProductionManager` handles every task related to production of buildingd and units
  * Modular design allows for easy modifications and updates over time
    * ex: To change unit micro behaviour simply replace the `MicroManager` sub-classes
  * Vertical communication reduces code complexity
    * ex: Only `CombatCommander` can talk to `Squad`, passing it orders
  * Information is on a need-to-know basis, reducing data transfer between modules
    * ex: Only `WorkerManager` knows which workers are assigned to which jobs
  * Vertical structure avoids code duplication


## Global Communication ##

There are a few cases of global communication in UAlberaBot, which are implemented via `Singleton` classes. Computationally expensive tasks such as path finding and strategy logic are performed inside these classes, and the results are then made globally available. These classes are:
  * `InformationManager`
    * Computes and stores important information about units which each other manager may want access to (ie: determine if opponent has a detector)
  * `MapTools`
    * Performs ground-distance path finding and caches results for later retrieval
  * `MapGrid`
    * Performs efficient nearest-neighbour calculations as well as stores information relating to when an area of the map was last visited
  * `StrategyManager`
    * Makes decisions about build-order goals, when to expand, and other game policies