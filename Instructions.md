# Download #

To download the bot source code, do one of the following:

  * Click [Downloads](http://code.google.com/p/ualbertabot/downloads/list) above. Download the UAlbertaBot zip file and extract to a directory.
  * Click [Source](https://code.google.com/p/ualbertabot/source/checkout) above. Check out the source code via an SVN client such as [Tortoise SVN](http://tortoisesvn.net/).

# Prerequisites #

UAlbertaBot is currently being maintained using Visual Studio 2012. Any version of Visual Studio can be used to compile the source, however the only VS Project files I will be actively maintaining are in VS 2012.

List of Prerequisites for Bot:
  * [Visual Studio 2010](http://www.microsoft.com/visualstudio/eng/downloads) OR [Windows SDK v7.1](http://www.microsoft.com/en-us/download/details.aspx?id=8279) (required for VS2012 to get v90 support)
  * [Visual Studio 2012](http://www.microsoft.com/visualstudio/11/en-us) (any version)
  * [BWAPI 3.7.4](http://code.google.com/p/bwapi/)
  * [BOOST 1.47.0](http://www.boost.org/) (or higher)

The UAlbertaBot project files use Windows Environment Variables to point to their required libraries.

![http://webdocs.cs.ualberta.ca/~cdavid/starcraftaicomp/project.jpg](http://webdocs.cs.ualberta.ca/~cdavid/starcraftaicomp/project.jpg)

List of Required Windows System Environment Variables:
  * BWAPI\_DIR - BWAPI root directory
  * BOOST\_DIR - BOOST root directory

To edit these in Windows 7 right click "My Computer" > Properties > Advanced System Settings > Environment Variables:

![http://webdocs.cs.ualberta.ca/~cdavid/starcraftaicomp/environment.jpg](http://webdocs.cs.ualberta.ca/~cdavid/starcraftaicomp/environment.jpg)

NOTE: These variables CANNOT CONTAIN SPACES! (VS limitation)

# Compile #

To compile the bot:
  1. Open `UAlbertaBot/VisualStudio/UAlbertaBot.sln` in VS 2012
  1. Choose not to upgrade the projects (bwapi requires v90)
  1. Select Release mode
  1. Ensure that the "Configuration Properties" > "General" > "Platform Toolset" for all projects is set to "v90"
  1. Ensure [USING\_VISUALIZATION\_LIBRARIES](https://code.google.com/p/ualbertabot/source/browse/trunk/AdversarialSearch/source/Common.h#12) is not defined. This #define is used to launch our OpenGL visualization tool and is not supported at this time.
  1. Build "AdversarialSearch" project
  1. Build "StarcraftBuildOrderSearch" project
  1. Build "UAlbertaBot" project

# Run #

To run the bot:
  1. Install [BWAPI tools and libraries](http://code.google.com/p/bwapi/wiki/UsingBWAPI)
  1. Run Chaoslauncher.exe (obtained from BWAPI project)
  1. Modify Chaoslauncher config to point to compiled UAlbertaBot.dll
  1. Run StarCraft using Chaoslauncher