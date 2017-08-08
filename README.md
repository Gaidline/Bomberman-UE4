# Bomberman-UE4

**3D, top-down, multiplayer game created with Unreal Engine 4.15.3.**


### About the game

To play right away, go to *Bomberman Release* and run *Bomberman_UE4.exe*. 

**In order to use multiplayer feature, make sure you allowed the game to communicate through the Windows firewall (popup window after first launch) and switch off the *VirtualBox Host-Only Network* in Network Connections as it may cause problems with finding game session by others.**

First Player who clicks 'Play' in the main menu, creates game session. Other Players can join this session by clicking 'Find games' and selecting the adequate session from list. Up to 4 users can play in one session. When there are at least two Players logged in, the game starts and the timer begins to countdown. During the game, a new Players can join if there are free slots for them.

At the beginning each Player has one Bomb to plant at a time. The Bomb returns to the Player inventory after explosion. A number of Bombs, as well as its blast length, can be upgraded by a Pickup. Bomb blasts can destroy destructible walls, other Pickups and Players.

Pickups are spawned in the map by destroying destructible wall. Each wall may spawn one of three different Pickups: Bomb Pack (+1 Bomb to inventory), Longer Bomb Blast (+2 squares to blast length) or Speedup (x2 running speed for the Player within 10sec).

A Player wins when he is the last one standing in map. If there is more than one Player when game time is up, or if all Players are dead, then there is a draw. When the game round is over, the main menu will be loaded in a few seconds. Players can create another game session from there.


### Source code

Entire game is made in Blueprints which are stored in Content -> Blueprints. Text below describes the most important assets:
* **Camera_Pawn** - A static Camera used by all Players to see the map. Provides information about world forward and right vectors.
* **Bomberman_GameMode** - Handles Player login and logout. Spawns new Player in proper slot. Updates game time. Handles end game conditions (victory, draw). Sends HUD update requests to all Players.
* **Bomberman_GameState** - Stores several variables that should be replicated between all Players.
* **Bomberman_GameInstance** - Handles networking events: creating, destroying and joining game session. Handles networking errors.
* **MapConstructor** - An Actor that uses Blutility (function to run in Editor) to build a map by using *DestructibleWalls* and *ImmortalWalls* (blocks). You can specify width and height of the map, offset between blocks, array of indexes of places where should not be any wall there (i.e. Player start locations). You can also specify the probability of placing *DestructibleWall* in given place (default is 50%) which results in number of *DestructibleWalls* in the map. In order to use this Actor to build new map, place *MapConstructor* in the level, select it and go to details, setup variables as you like, scroll down and select blutility function (*Build Map Geometry*), click run.
* **DestructibleWall** - Spawns random Pickup when destroyed (with probability).
* **Bomberman** - Main playable Character. Stores logic for: movement, Bomb planting, target view changing, picking up the Pickup, Player color setting, HUD updating.
* **Spectator** - A Character used by the Player when he is dead. Allows him to continue watching the game but disables all interaction.
* **Bomb** - Used as a main weapon by the Players. Handles blast tracing. Sends Destroy event to whatever should be destroyed.


### Further development

* Connect *Spectator* with *Bomberman* (i.e. through base player) as it contains almost the same methods as the *Bomberman* (not all methods though).
* Add new templates to *MapConstructor* to build new maps with different composition of *ImmortalWalls*.
* Add zoom in / out feature to *Camera_Pawn*, which will be useful for bigger maps.
* New Pickup: temporal protection against Bomb blast.
* Kill counter and playing time in Player HUD.
* Create AI to use when no Player has joined game session.