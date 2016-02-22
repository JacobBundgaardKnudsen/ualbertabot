# AdversarialSearch #

This folder contains the source code and VS project files for the implementation of our alpha-beta search algorithm for StarCraft combat micro decisions.

The details of the algorithms used can be found in our 2012 AIIDE paper: [Fast Heuristic Search for RTS Game Combat Scenarios](http://scholar.google.com/citations?view_op=view_citation&hl=en&user=teRseTgAAAAJ&citation_for_view=teRseTgAAAAJ:olpn-zPbct0C).

As the search algorithm can only handle battles of sizes up to 8 vs. 8 units, it cannot be used in whole in its current form. Instead, it is used for its combat simulation system, which uses script-based micromanagement to play out StarCraft battles.

In UAlbertaBot, it is called by [Squad](https://code.google.com/p/ualbertabot/source/browse/trunk/UAlbertaBot/Source/Squad.cpp#179) to determine whether or not a Squad of units should retreat from a battle or continue to press onward. To implement your own retreat code, simply edit this section to remove all calls to CombatSimulation.

Here is a video of the search code in action, see youtube video for description:

<a href='http://www.youtube.com/watch?feature=player_embedded&v=9441p6uTIVU' target='_blank'><img src='http://img.youtube.com/vi/9441p6uTIVU/0.jpg' width='640' height=360 /></a>