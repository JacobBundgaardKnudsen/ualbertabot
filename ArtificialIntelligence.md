# Artificial Intelligence #

UAlbertaBot contains a mix of dynamic artificial intelligence systems and scripted rule-based decision making. Here I will give an overview of each main function of the bot and how it is performed.

## Build-order Planning (Search-Based AI) ##
  * All build-orders in UAlbertaBot are planned by our heuristic search based [build order planning system](StarcraftBuildOrderSearch.md). Build-order goals are given by `StrategyManager` to `ProductionManager` which then uses this system to dynamically generate near-optimal build order plans. The only two exceptions to this rule are:
    * A supply producer is build immediately if a supply defficiency is detected
    * A detector is made if enemy cloaked units are detected
  * Details of this method can be found in our AIIDE 2011 paper: [Build-order Optimization in StarCraft](http://scholar.google.com/citations?view_op=view_citation&hl=en&user=teRseTgAAAAJ&citation_for_view=teRseTgAAAAJ:XiVPGOgt02cC).

<a href='http://www.youtube.com/watch?feature=player_embedded&v=-8IycRLEBok' target='_blank'><img src='http://img.youtube.com/vi/-8IycRLEBok/0.jpg' width='640' height=480 /></a>

## Combat Scenario Decisions (Real-time Simulation) ##
  * All battles UAlbertaBot encounters are first run through our [combat simulation system](AdversarialSearch.md). If the outcome of this simulation is positive we continue toward the enmy base. If the outcome is negative, we retreat toward our own base.
  * Details of the this method can be found in our 2012 AIIDE paper: [Fast Heuristic Search for RTS Game Combat Scenarios](http://scholar.google.com/citations?view_op=view_citation&hl=en&user=teRseTgAAAAJ&citation_for_view=teRseTgAAAAJ:olpn-zPbct0C).


<a href='http://www.youtube.com/watch?feature=player_embedded&v=9441p6uTIVU' target='_blank'><img src='http://img.youtube.com/vi/9441p6uTIVU/0.jpg' width='640' height=360 /></a>

## Multi-Unit Path Finding (Search-Based AI) ##
  * A flood-fill based multi-agent path finding system is used to determine all ground paths to a given destination on a given map. The results of these are cached efficiently for later re-use if necessary.
## Strategy Selection (Learning AI) ##
  * UAlbertaBot uses the UCB algorithm for selecting a strategy to use against a given opponent, based on the results of previous matches. If no match history is available, we use a random selection from given strategies. The following graph shows how UAlbertaBot's win rate went up over time as it learned which strategies to implement in the 2012 AIIDE StarCraft AI Competition:

![http://www.cs.ualberta.ca/~cdavid/win_percentage.png](http://www.cs.ualberta.ca/~cdavid/win_percentage.png)

## Unit Micromanagement (Rule-Based) ##
  * Currently, all unit micromanagement follows a rule-based scripted policy found in its respective `MicroManager` subclass. We hope to soon extend this to use our full [alpha-beta based micro controller](AdversarialSearch.md).
## Worker-Resource Allocation (Rule-Based) ##
  * Workers are allocated to resources by rule-based scripts using expert knowledge obtained from professional StarCraft players.
## Race Selection ##
  * Currently UAlbertaBot only plays the Protoss race, however its architecture allows for any race to be played. The only limiting factor is a fix needed to the build order system which currently has a bug for the zerg race. In the future we hope to play random race.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=LwWQy7MxT-Q' target='_blank'><img src='http://img.youtube.com/vi/LwWQy7MxT-Q/0.jpg' width='640' height=480 /></a>