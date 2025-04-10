# Creating a GVH map

Creating a GVH compatible map is very simple if you already have experience in making Doom maps. I recommend using [Ultimate Doom Builder](https://ultimatedoombuilder.github.io/) for your map editor. The map format can be any compatible Zandronum map formats, but I recommend UDMF as it grants the mapper a lot of flexibility not found in other formats. There are many guides on the internet for making Doom maps, so only the basics of setting up your map for GVH play will be covered here.

## Map Specific Things

Ghouls vs Humans has no map specific objects needed to be playable, although I highly recommend adding both Human (Blue) and Ghoul (Red) team starts, preferably at opposite ends of the map with no sight lines to the enemy team:
```
5080 - Blue Team Start (Humans)
5081 - Red Team Start (Ghouls)
```
While GVH:RE functions without team starts, it can help balance your map. If no team starts are available, random Deathmatch Player Starts will be used instead. It is generally good practice to add Deathmatch AND Player 1 starts in case your map happens to get played in other game modes and mods.

Map Cards are also an optional feature that can display information about your map when players enter a level. See [Creating Map Cards](MapCards.md) for more details.

## Map Design

While it is still debatable what makes a good GVH map, there are a few points you can keep in mind while planning your layout.

* Humans thrive in open areas with lots of room to retreat. If your map is too open, Ghouls will have trouble getting kills without great teamplay. Humans will often flock to areas of the map that are either open or have a single choke point that can be defended. Try to give your map several routes for Ghouls to attack from when making open areas.
* Ghouls love cramped areas and tricky terrain that regular Human classes can't easily navigate. If your map is too cramped or has a lot of closed off areas, Ghouls will barrel in and wipe out the Humans with ease. If your map has lots of elevation or tall ledges that Humans can't easily get over, Jitterskull and Sjas will have an advantage.
* While Ghouls vs Humans is horror themed, too much darkness can hinder gameplay. No Human is going to walk into an overly dark or foggy area if they can help it. Try to save darkness/fog for limited sections of the map that provide a risky retreat, instead of going all in on atmosphere.
* Try to think about where your teams spawn, and what areas players will most likely head in to get an advantage. Good GVH maps start both teams with many options to take, instead of a single choke point or doorway heading to the enemy team. While the "defend the base" style of map can work, it can be hard to pull off without the gameplay always devolving into the same strategy.

