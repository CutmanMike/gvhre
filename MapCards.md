# Creating Map Cards

Map cards are little information cards that appear at the start of a level, showing who created the map and where the map's music is from. Creating a map card for your (or someone elses) map is very simple.

## LANGUAGE

All you need to do to set up a map card is add a few entries to a **LANGUAGE** lump. The game will handle the rest. Replace MAPNAME with the map code of your map (i.e MAP01).

- **MAPCARD_NAME_MAPNAME** - The actual name of your map, i.e "Claw Fist".
- **MAPCARD_SUBTEXT_MAPNAME** - Smaller text that appears under the name of your map, can be anything.
- **MAPCARD_NAMEFONT_MAPNAME** - The big font used for the map name. Default is BIGFONT.
- **MAPCARD_AUTHOR_MAPNAME** - The name of the person who made the map.
- **MAPCARD_MUSIC_MAPNAME** - The name of the music that plays in this map.
- **MAPCARD_MUSICSOURCE_MAPNAME** - The name of the composer or where the music came from.
- **MAPCARD_ICON_MAPNAME** - The graphic for the icon that appaers next to the map name. If used, should be a 128x128 image.
- **MAPCARD_BACKGROUND_MAPNAME** - The background texture to use under the map card's text. Can be an existing texture or a custom graphic. Default size is 64x64.
- **MAPCARD_BACKGROUNDHEIGHT_MAPNAME** - The height for the map card background texture. Default is 64.
- **MAPCARD_BACKGROUNDWIDTH_MAPNAME** - The width for the map card background texture. Default is 64.
- **MAPCARD_BACKGROUNDYOFF_MAPNAME** - The vertical offset for the map card background. Useful if the background image chosen doesn't look quite right (when using a sky texture for example).
- **MAPCARD_SCROLL_MAPNAME** - The scroll speed of the background. Any higher than 1 will make it scroll quite fast, and 0 disabled scrolling. Default is 1.
- **MAPCARD_FRAME_MAPNAME** - The frame graphic to be used for the map card. You can use one of the frames in GVH:RE's graphic/mapcard folder, or make your own. Default is "MAPCRB00".

Here is an example entry of a map card:

```
MAPCARD_NAME_GVH30 = "\ctStrange Aeons";
MAPCARD_SUBTEXT_GVH30 = "\cjGhouls vs Humans: \cvCold Demise";
MAPCARD_NAMEFONT_GVH30 = "GRGW_LWR";
MAPCARD_AUTHOR_GVH30 = "DBThanatos & ds201";
MAPCARD_MUSIC_GVH30 = "Dangerous Adventures";
MAPCARD_MUSICSOURCE_GVH30 = "Cazok";
MAPCARD_BACKGROUND_GVH30 = "RW23_1";
MAPCARD_BACKGROUNDHEIGHT_GVH30 = "128";
MAPCARD_FRAME_GVH30 = "MAPCRB07";
```

## Fonts

GVH:RE has the following fonts to use for map cards, though you can always import your own:

- **BIGFONT** - Standard Doom2 big font, used by default.
- **GRGW_LWR** - Gargoyle Wing. The font used for many GVH:RE elements.
- **GRGW_BO** - Same as above but with a black border.
- **ZBIGFONT** - ZDoom logo font.
