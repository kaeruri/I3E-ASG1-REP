# I3E ASSIGNMENT 1 README: 3D VIRTUAL PLAYER EXPERIENCE


## Game Overview
This project is an interactive, first-person 3D virtual puzzle-corridor experience built entirely in Unity using C#. The player must navigate a secure testing facility by overcoming hazardous obstacle systems, collecting hidden keycards, and utilizing a laser-pointer interaction mechanism to bypass security protocols and reach the final exit.


## Controls & Instructions

| Action | Control (Input) |
| :--- | :--- |
| **Movement** | W, A, S, D / Arrow Keys |
| **Look / Aim** | Mouse Movement |
| **Interact / Fire Pointer** | E Key |


## System Requirements & Desired Settings
* **Target Platform:** Windows PC (Desktop executable or Unity Editor Play Mode)

* **Hardware Requirements:** Standard keyboard and mouse input. Any DirectX 11 or DirectX 12 compatible graphics card.

* **Desired Project Settings:**
  * Resolution: 1920x1080 (Aspect Ratio: 16:9 widescreen)
  * Input System: Built using Unity's standard Input Manager.
  * Audio: System volume enabled to hear interactive spatial audio feedback loops.


## Official Walkthrough & Puzzle Answer Key
The level functions as a sequential logic puzzle that must be completed in the exact order below:

1. **The Laser Corridor:** Navigate past the initial red security lasers. Standard horizontal beams will drain player health on impact, while the fast high-voltage grids will trigger an instant kill.

2. **Retrieving Key 1 (Sensor Key):** Follow the branching path to locate and collect the physical Sensor Key card item floating in the room.

3. **Deactivating the Grid:** Return to the main corridor. Face the purple wall-mounted Security Button. Point your player's built-in raycast laser pointer directly at the button and press E. Because you have the Sensor Key, the button will verify the identity, turn green, and completely shut down the active laser grid.

4. **Retrieving Key 2 (Access Key):** Walk safely through the deactivated grid path to find and collect the final Access Key card.

5. **The Final Escape:** Approach the locked laboratory exit door. With the Access Key in your inventory, crossing the door's proximity trigger will automatically swing it open permanently to complete the experience.


## Known Limitations & Application Bugs
* **Character Controller Velocity:** If the player walks into a hazard while mid-air or sliding down a collider slope, a slight physics stutter may occur right as the teleportation logic sets in. This is handled gracefully in the code by completely arresting all active Rigidbody velocity vectors before changing the transform position.

* **Raycast Distance:** The interaction laser pointer has a maximum range restriction. If the player stands too far away from the purple Security Button, pressing E will not trigger it. The player must step into proper proximity range for the raycast to register a valid hit.


## References, Assets & Credits
* **Audio Sound Effects:** UI Chimes, Door Servos, and Laser Zap assets sourced via the Unity Asset Store (Free Casual Game SFX Packs / Freesound.org open-source library).

* **Textures & Materials:** Custom Unity Emission shaders utilized to create the glowing red visual look for the physical laser hazard boundaries.

* **3D Models:** Level design architecture composed entirely using Unity's built-in 3D Primitive shapes (Cubes, Capsules, and Cylinders) to maintain a clean, high-performance prototyping test-chamber environment.


## Implemented Systems & C# Script Logic

### 1. Physics & Interaction System (PlayerInteract.cs)
* **Logic:** Utilizes Unity's physics engine to cast an invisible Physics.Raycast straight out from the center of the player's camera view. 

* **Mechanics:** When looking at interactive objects within a specific distance, a visual laser-pointer ray line is rendered using a LineRenderer. Pressing the interaction key (E) sends a message to the target object to trigger its activation logic.

### 2. Player Inventory & Respawn Persistence (PlayerInventory.cs & PlayerHealth.cs)
* **Logic:** Tracks boolean states for collected keys (hasAccessKey, hasSensorKey). 

* **Checkpoint Logic:** If the player takes too much damage or hits an instant-kill hazard, the script temporarily disables the CharacterController, clears all physical Rigidbody velocity vectors to prevent physics glitches, and teleports the player's transform back to the Spawn_Point.

* **Persistence:** Because the level does not reset via scene reloads, all collected inventory items remain intact upon death, preventing game breaking soft-locks.

### 3. Hazard Grid Mechanics (LaserHazard.cs & SecurityButton.cs)
* **Logic:** Laser hazards utilize Box Colliders set to Is Trigger. When a player's collider crosses the boundary, it reads the script components to determine damage severity.

* **Deactivation:** Firing the interaction pointer at the purple security button checks the player's inventory for the hasSensorKey. If verified, it changes the mesh material color to green and sets the entire Laser_Grid parent GameObject to inactive, safely silencing the trap.

### 4. Animation & Audio Feedback Logic (DoorController.cs)
* **Logic:** The security exit door handles proximity entry via physics triggers. When a valid player with the Access Key enters the trigger, it sets the Animator state variable isOpen to true.

* **Permanent State:** The script completely bypasses the OnTriggerExit loop once unlocked, ensuring the door remains locked open forever.

* **Audio Integration:** Dedicated AudioSource.PlayOneShot() nodes are injected across the door, button, and laser scripts to execute real-time audio chimes for Access Granted, laser zaps, and power-downs.


## Project Architecture & Organization
The Unity Project hierarchy has been cleanly modularized for clean architecture and version control deployment:

* _Project/Animations/ — Contains custom animation controllers and door override clips.

* _Project/Audio/ — Contains audio interface and hazard sound effects.

* _Project/Materials/ — Holds visual indicators like red laser emissions and security button status colors.

* _Project/Prefabs/ — Houses modular level obstacles and item instances.

* _Project/Scripts/ — Contains cleanly documented C# runtime behaviors.
