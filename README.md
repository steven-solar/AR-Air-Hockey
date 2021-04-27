# README for AR Air Hockey: Final Project by Marc Di Meo and Steven Solar

This unity project builds and runs 3D AR Air Hockey on macOS and iOS to be played by one player against an AI, or two players on macOS. 

###GameObjects:
* Table: This is the flat game table, where the air hockey game lives. This holds the majority of the game objects and is mapped to the table marker. This holds the AI, striker, goals, and all the walls. 
* Player_Puck: This is the player's "handheld" paddle used to strike the Striker and score, consisting of the base of the paddle and the mallet on top. It contains a RigidBody component, and a Capsule Collider, and is tagged as "Player". Upon colliding with the Striker, it causes it to bounce and pick up speed. This is mapped to the Player target. Utilizes Player.cs script.  
* AI: AI defines the AI or Player 2's blue "handheld" paddle used to strike the striker, consisting of the base of the paddle and the mallet on top. It is tagged as "AI" and given a Capsule Collider to define collision behavior with the puck in AI.cs. A RigidBody component is attached to handle the physics of puck collision. It allows for user input of isAI to determine whether it should be controlled by an AI or WASD keys (only for MacOS). AI.cs is attached as a script component, which will be explained in greater detail
* Striker: The Striker object is what the players hit to try to score on their opponent. It contains a RigidBody component, and a Capsule Collider. It is given a Capsule Collider and tagged as "Puck" in order to define collision behavior with players, goals, and walls in the various game scripts. Utilizes Striker.cs script
* Edges: The four walls define the boundaries of the table, and are given BoxColliders that cause the puck to bounce upon striking a wall. The four edges define the boundaries of the playing space, upon colliding with these the puck will bounce back. Each edge is tagged "EDGE_LEFT", "EDGE_RIGHT", "EDGE_FRONT" and "EDGE_BACK". Puck collision behavior is defined in Striker.cs.
* Goals: There are two goals on each side of the board, one for each player to defend. The goals are tagged AI_GOAL and PLAYER_GOAL, with the AI defending AI_GOAL and the player defending PLAYER_GOAL. These tags, along with the goals' Box Collider components, are used to define collision behavior with the puck in Striker.cs. When the Striker collides with the goal's BoxCollider, it increases the other player's score and resets the Striker position. 
* Scores: The Score objects P_Score (player score) and E_Score (enemy score) keep track of the goals scored by both players. The objects are tagged as "P_Score" and "E_Score" respectively in order to define scoring in GameManager.cs 
* AR Camera: Built in object from Vuforia but converts the scene into augmented reality. Utilizes GameManager.cs script. 

###Scripts:
* GameManager.cs: Defines high level game functionality such as behavior on-score and tracking each player's score. 
* Player.cs: Defines player movement and behavior on colliding with the Striker. Player.cs is attached to the Player GameObject. 
  * Update(): Upon pressing an arrow key, Player moves in that direction at PlayerSpeed speed, Player is kept within the board boundaries via edge detection. 
  * onCollisionEnter(): Sets up the AI to counter. Causes the striker to bounce appropriately.
* AI.cs: Defines AI movement and behavior on colliding with the Striker. AI.cs is attached to the AI GameObject.
  * Awake(): Grabs Difficulty and isAI from player preferences
  * Start(): Defines "basePoint" if isAI depending on difficulty (higher difficulty -> player closer to own goal)
  * Update(): 
    * If !isAI: Moves with arrow keys the same way Player does (WASD instead of arrows) 
    * If isAI: AI splits behavior into two cases for when puck is in AI half or Player half. In AI half, tries to hit the puck towards player's goal, in player half tries to defend the puck from scoring on its own goal
  * onCollisionEnter(): Causes the puck to bounce appropriately
* Striker.cs: Triggers goal scoring and bouncing on appropriate collisions. 
 * OnTriggerEnter(): Calls Reset function from GameManager.cs for goal scored on Player or AI. Calls bounceSideWall() and bounceGoalWall() from GameManager.cs when colliding with edges of the table


### Image Targets:
The project utilized Vuforia Engine to create two AR markers used for the table and the player striker. The Vuforia package was implemented into the Unity through package manager and is the only package that was used for development. The license for the markers is “ARAirHocker” with license key “AbGoQrv/////AAABmU9BErH4cUg1nN1ZGZ4PbctipWM04jhI6bhYXauffhn+M2PZJJb6d97HytnDTk04vp7Dz6cBVS8NoMID1eLroFSX8ogAaSqH7ErpYU49nkIB9Z6bmnM0Tdiz8dbaDZFQxUUP6QKaY+3aHPx8ezgC46z2+vlwvlppWLGh19N8TLmnJOg6PUs10Q6wUNIBY6X+ynTL7cNjEMWtk9AO+i4Cxkat5zc6yizc2sLJ2fEhesPFxaWQHVV/LOvoNAXLjAE2xi8eEE4+dQHExQ2X+OscimBPr6MXymmtjDBkkLckqyzAB9gXNw0PXg6hSBxZaR+5zckfJ1Pm+DfrqovsrWrh0FpHls64nU3FiHwVBYTj1lbB” 
Vuforia markers work by mapping objects to the image targets in real time. When the image targets move so do the objects attached to the image targets. 
* Table_Target: A QR code that acts as the parent to the Table GameObject
* Player_Target: A QR code that acts as the parent to the Player_Puck GameObject

### How to play MacOS:
1. Load unity project into unity
2. Select AI puck object and determine whether you want to play with an AI or two player through the script input (isAi == 0 for player input, isAi == 1 for AI)
3. Press play when you are ready, control Player with Up, Down, Left, Right arrow keys and if not AI, control Player 2 with WASD keys

### How to download for iOS:
1. Open up unity on your laptop or computer
2. Make sure you are on the default scene for iOS
3. Press File > Build Settings 
4. Specify iOS and select your local version of Xcode (you need Xcode to run the game)
5. Select build and create a folder to create the game. 
6. Once the build is complete open up the XCode file titled “Unity-iPhone.xcodeproj”
7. The file will open up in XCode
8. Plug your iOS device into your computer
9. At the top of XCode you will specify where you want to build the device (this should indicate the name of your phone)
10. On the left hand side select “Unity-iPhone”
11. Select on the “Signing & Capabilities” tab and check off automatically manage signing
12. Change the team to your name in the drop down menu
13. Change the bundle identifier to anything you want ensuring it is unique from how you originally got it
14. Press the play button on the top left of Xcode
15. This should download the game onto your iOS device
16. On your iOS device allow access to the game in General>Device Management in settings
17. The game is ready to be played.

### How to play iOS:
1. Place the table Vuforia tracker on a surface of your choice. This can be a table, the floor, or any flat surface. 
2. Place the striker Vuforia tracker on the same surface. 
3. Open up the unity game app and hold your device above both the trackers
4. Play around with the placement of the trackers until you can see both of the objects in the AR environment. 
5. You may need to utilize a stick of some sort but move the striker tracker and hit the puck. 



