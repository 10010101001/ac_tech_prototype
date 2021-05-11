# AssaultCube Tech Prototype #

## Purpose ##

<img src="/doc/screenshot_ac_complex_800.png" width="50%" alt="Tech Prototype: ac_complex"><img src="/doc/screenshot_ac_complex2_800.png" width="50%" alt="Tech Prototype: ac_complex">

Development of the first-person-shooter game AssaultCube ("AC") began in 2004 and was first released in 2006. It is based on the [Cube (1) Engine](http://cubeengine.com/). One of the current challenges is the fact that AssaultCube can no longer live up to the expectations of todays players in terms of visual appearance. It may not be possible to make AssaultCube look like a 2021 AAA game and compete with commercial games but it is realistic to catch up significantly.

It is safe to say that all the potential of the Cube Engine has been fully exploitet. It is not possible to make the game look better with this engine. The artists have done everything possible to use the available tools to the max and they achieved outstanding results considering the technological limits. 

The natural step forward is to renew the technology stack from the ground up and to provide new tools to the artists so that they can make existing content look much better plus can
produce fresh, modern content for the game.

The Cube Engine has meanwhile been succeeded by [Cube 2: Sauerbraten Engine](http://sauerbraten.org/) which in turn has been succeeded by [Tesseract Engine](http://tesseract.gg/). The basic assumption is that the artists and programmers in the AssaultCube community like the philosophy of the Cube engine(s) and want to move forward with that approach thus want to explore the capabilities of the Tesseract Engine.

The purpose of this tech prototype is to explore how the technology of AssaultCube could be renewed by using the Tesseract Engine.

## Disclaimer ##

This prototype has been put together by a programmer and not by an artists and so the game assest (maps, models, textures) are not of sufficiently good quality to showcase the full potential of the engine. For example, textures are currently of poor quality because the normalmaps and specularmaps are cheaply autogenerated and because the textures are generally of low resolution. The final result will look better once artists improve the assets.

## Quick Tour ##

Follow the steps of this quick tour to get an impression of the technology.

- Start assaultcube.bat / assaultcube.sh
- Navigate to "Options" 
  - Click the "Resolution" tab and make sure the proper resolution is set
  - Click the "Display" tab, tick "Fullscreen", set Anisotropic Filtering and Multisample AA to "16x" and set Morphological AA/Temporal AA/FXAA to "ultra"
  - Click the "Graphics" tab and set all options to "high"
  - In case you get a poor frame rate then lower some of these options.
- Navigate to "Map Browser" and click the map "ac_complex"
- Walk around and look at the red bricks (parallax mapping)
- Walk inside the buildings and notice the illusion of depths at walls and floors (normal mapping) 
- Look for metallice surfaces and note the reflections (specular mapping)
- Walk to the center square to see a few static player models 
- Press T and type `/thirdperson 2` to see an animated player model in action
- Press T and type `/addbot` to spawn a bot, then shoot the bot to see ragdoll physics in action
- Press E to enter edit mode and drag some lights around to see the dynamic lighting
- Select a few cubes from a wall surface then press G and scroll to change selection gridsize
- Release G and use the scrollwheel to push/pull a few cubes out/in the wall
- Try to build a simple building with three floors
- Press F2 and F3 to apply some textures and place some mapmodels into the scene
- Run a server instance with server.bat / server.sh
- Play with a friend on your server and figure out where the physics/movement feel like AC v1 and where it deviates

These steps should give you a first impression. If you would like to gain a deeper understanding please consult the following resources:

1. [Changes](#changes)
2. [Asset Migration Path](#asset-migration-path)
3. [Findings](#findings)

## Changes ##

This tech prototype contains the following changes compared to vanilla Tesseract:

- 1 map from AC v1
- 49 textures from AC v1 with cheaply generated normalmaps/specmaps
- 5 hudgun models from AC v1 - the sniper rifle has a cheaply generated normalmap/specmap and the other guns have none
- 1 skeleton-animated player model with ragdoll physics support (some say they look more like Seaman than terrorists)
- 3 test map models (static player models)
- 1 game mode (insta i.e. OSOK with crosshair)
- hudgun sway and hudgun position from AC v1
- movement speed, crouch speed, straferunning from AC v1 (not perfect yet)
- disabled colouring of player models
- a little bit of AC theming for the UI
- import command for AC v1 maps 

## Asset Migration Path ##

### Map Migration ###

<img src="/doc/editmode_800.png" width="50%">

#### Guidelines ####

- The lighting, specularity and normalmapping features of the engine **should be used wisely**. We should not create technology driven content. AC should retain its theme and should not get a metallic, overbright, overspecced look. The reason why things currently look that way is because it is a simple prototype. Please note that even the very first AC v1 maps (ac_complex and ac_desert) used zero to no colored lights probably for exactly that reason - technology should serve art and not vice versa.
- There should be an approach for official maps: Existing official maps should be migrated in a way that preserves the original textures (with normal/specmaps) but textures should not be replaced. Lighting should be done in a way that comes close to the v1 look. If the author of a map wishes to further improve the map (geometry/textures) it may be better to create a new version (ac_complex and ac_complex_classic). This is important to remain true to the original look as described in [community split](#community-split).

#### Import ####
- The tech prototype supports AssaultCube map format 10 from v1.3. Older formats are not supported.
- Place your map in the folder *\media\map\legacyformat*
  - To get started you should try to import ac_complex since its textures do already have some sort of normalmaps/specmaps.
- Start the tech prototype and type `/importassaultcube <yourmap> <newmapsize>`
- The map will be migrated and saved to */media/map/yourmap_imported.ogz* and */media/map/yourmap_imported.cfg*
- To prevent accidential overrides by succint imports you should save it with a new name. Do not forget to manually copy the .cfg as well.

#### Geometry ####
- After import, type E to enter edit mode, see also [basic editing](http://sauerbraten.org/docs/editing.html) and [editing binds](https://github.com/drian0/ac_tech_prototype/blob/master/config/default.cfg#L94)
- You will notice that certain textures on corner cubes may be broken - you you will need to fix this manually
- You will notice that certain heightfields that span more than one cube may be misaligned - you will need to fix this manually

#### Lighting ####
- After improving geometry, review the lights in the map
- Tesseract comes with a dynamic lighting system whereas AssaultCube v1 used a 2D lighting system. This means the imported lights do not make much sense - delete them with the command `/clearents light` 
- The imported map will have ceiling cubes with a skybox texture - remove these cubes so that the sunlight reaches into the map
- Place new [lights](http://sauerbraten.org/docs/editref.html#_light_) and [spotlights](http://sauerbraten.org/docs/editref.html#_spotlight_) in the map
- Tweak the sunlight with the commands [sunlight](http://sauerbraten.org/docs/editref.html#sunlight), [sunlightyaw](http://sauerbraten.org/docs/editref.html#sunlightyaw), [sunlightpitch](http://sauerbraten.org/docs/editref.html#sunlightpitch) and [sunlightscale](http://sauerbraten.org/docs/editref.html#sunlightscale)
- Tweak the [ambient](http://sauerbraten.org/docs/editref.html#ambient) lighting and [skylight](sauerbraten.org/docs/editref.html#skylight)
- Tweak diffuse global illumination with giscale, gidist, giaoscale as documented [here](http://tesseract.gg/README)
- Hint: Please note that some commands of the [Cube2:Sauerbraten editing reference](http://sauerbraten.org/docs/editref.html) have been advanced by Tesseract, check out the [Tesseract README](http://tesseract.gg/README) and [Tesseract Rending Pipeline Documentation](http://tesseract.gg/renderer.txt)

#### Textures ####
- After improving the lighting review the textures in the map
- You will notice that textures that are not power-of-two might have a wrong offset
  - Example: After importing ac_complex check out the 3x3 wooden boxes on the streets with misfit textures (texture *makke/box_3.jpg*)
  - This can be fixed my changing the texture offset on the given surface/cube in editmode by pressing O or P and scrolling. Alternatively put [texscale](sauerbraten.org/docs/editref.html#texscale) in the *\*.tex* configuration of the given texture. 
- Review the */media/map/yourmap_imported.cfg*
  - Look for the *texload* commands; `texload <yourtex>` will load the texture configured in *media/texture/yourtex.tex*
  - Note that if the *importassaultcube* command does not find a given *.tex file it will fallback to the legacy texture definition instead. Example:
    - New: `texload "makke/rock"; texscale 2.0; setshader "stdworld";`
    - Legacy: `texture 0 "makke/rock"; texscale %.1f`
  - The cfg may contain some broken lines such as "0 0 0 xyz" due to a bug in the importer, clear those lines 
  - If the cfg contains a *fog* command, fix the fog level by multiplying the value by 976 e.g. `fog 488292 // fog 500 in ac v1`
  - If the cfg contains a *shadowyaw* command, remove it since it is not supported anymore

### Texture Migration ###

<img src="/doc/normalmap.png" width="50%">

AC v1 supported diffuse textures only whereas Tesseract supports textures slots with eight different types: primary diffuse, secondary diffuse, decals, normal map, glow map, specularity map, depth map and environment map. There should be a *\*.tex* file per texture slot to define the types and that shader that is applied.

Examples:
- [Wood texture with Parallax Mapping](/media/texture/noctua/wood/planks02.tex)
- [Bricks texture with Parallax Mapping and Specular Mapping](/media/texture/mayang/bricks_2.tex)
- [Iron texture with Specular Mapping](/media/texture/noctua/metal/iron02.tex)

See also [texture types](http://sauerbraten.org/docs/editref.html#texture) and [shaders table](http://sauerbraten.org/docs/editref.html#setuniformparam).

All existing AC v1 textures should be migrated to this new configuration and should have *at least have a normalmap*. Most likely there is no point in having some texture remain diffuse-only.

### Hudgunmodel Migration ###

<img src="/doc/hudgun.PNG" width="50%" alt="Tech Prototype: ac_complex">

The weapons shown in the head-up-display should have at least a good *normalmap* and a *specmap*. The hands must not have any specularity. The look of the weapons should generally be very close to v1 due to [tradition](#Balancing-Innovation-and-Tradition).

On top of that the following optimizations could be applied as well:
 - models could have more polygons so that the round surfaces like barrel and scope are smoothened.
 - the existing skin textures contain highlighted areas to simulate specularity, this could be removed because this should now be done via spemap instead
 - the skins textures could be re-created in high-res

Example:
- [Sniper Rifle md3 config file](/media/model/hudgun/sniper/md3.cfg)

Notes:
- `md3load <model>` loads a given md3 file
- `md3skin <mesh> <skin> <mask>` applies a diffuse texture *skin* to the *mesh* with a given *mask*. The *mask* is an ordinary RGB picture where the R(ed) channel stores a specular map, the G(reen) channel stores glow map and the B(blue) channel stores environment map.
- `md3bumpmap <mesh> <normalmap>` applies a *normalmap* to the *mesh*
- `md3spec <mesh> -1` disables specularity for the given *mesh*. This is a useful shortcut so that you do not need to provide a *mask* with disabled specularity.

### Playermodel Migration ###

### Mapmodel Migration ###

## Findings ##

### Feasibility ###

Taking into account a) the capabilities of the new technology b) the capabilities of the development team and c) support of the AC community it is safe to say that renewing the technology stack of AssaultCube with Tesseract is feasible. The mission could be structured as follows:

- *People.* There should be a group of *at least* one developer, one artist and one tester than can focus solely on the first release with Tesseract without distraction. Maintenance of AC v1.x and AC mobile should be shouldered by other members of the development team for the duration of the first release.
- *Methodology.* The first release (*minimum playable product*) should have a very, very limited scope. This ensures that it get's done and that it creates a sense of achievement which is important for momentum. The scope could include 1 game mode, 1 weapon, 1 player model and 1 map. For example: instagib (OSOK with crosshair), sniper rifle, seaman-terrorist-guy and ac_complex.

### Hardware Support ###

AC v1 was marketed with the promise to run well on old computers. Most of the computers that were considered "old" back in 2004 do not exist anymore today. If we look at old computers by today's standards then Tessearct will support those when gfx settings are reduced. If the assets are too expensive computationally we may provide alternative assets for older computers. For example the default player model could have 10k polys while the alternate player model has 2k polys.

### Balancing Innovation and Tradition ###

Renewing the tech stack of a game poses the inherent risk of a community split. Some will keep playing the old version while others move on to the new version. If too many people stick to the old version the endeavour fails. Therefore it is important to 'worship tradition'. This is especially crucial in the first releases or first months/years. There needs to be a plan in which areas innovation is allowed an in which tradition is ensured.

