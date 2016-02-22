# StarcraftBuildOrderSearch #

This folder contains the source code and VS project file for the implementation of our heuristic search based build order optimization.

The details of this method can be found in our AIIDE 2011 paper: [Build-order Optimization in StarCraft](http://scholar.google.com/citations?view_op=view_citation&hl=en&user=teRseTgAAAAJ&citation_for_view=teRseTgAAAAJ:XiVPGOgt02cC).

All build-orders used by UAlbertaBot's [ProductionManager](https://code.google.com/p/ualbertabot/source/browse/trunk/UAlbertaBot/Source/base/ProductionManager.cpp) are planned by this system. If you wish to implement custom build-orders, you must replace calls to this module by your own custom system.

<a href='http://www.youtube.com/watch?feature=player_embedded&v=-8IycRLEBok' target='_blank'><img src='http://img.youtube.com/vi/-8IycRLEBok/0.jpg' width='640' height=480 /></a>