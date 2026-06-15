# I3E ASSIGNMENT 1 README: 3D VIRTUAL PLAYER EXPERIENCE


## Game Overview
This project is an interactive, first-person 3D virtual puzzle-corridor experience built entirely in Unity using C#. The player must navigate a secure testing facility by overcoming hazardous obstacle systems, collecting hidden keycards, and utilizing a laser-pointer interaction mechanism to bypass security protocols and reach the final exit.


## Controls & Instructions

| Action | Control (Input) |
| **Movement** | W, A, S, D / Arrow Keys |
| **Look / Aim** | Mouse Movement |
| **Interact / Fire Pointer** | E Key |

### **Objective:**
1. **Survive the Hazards:** Navigate past the security lasers. Red beam corridors will damage your health, while high-voltage grids will trigger an instant kill.

2. **Find the Sensor Key:** Locate and collect the hidden Sensor Key card. 

3. **Deactivate the Grid:** Use your player's built-in raycast laser pointer to aim at the purple wall-mounted Security Button and press E to shut down the main laser defenses.

4. **Acquire the Access Key:** Grab the Access Key card located past the defenses.

5. **Escape:** Approach the locked laboratory door with the Access Key to permanently open the exit and complete the level.


## Implemented Systems & C# Script Logic

The project perfectly meets all core technical parameters outlined in the assignment brief through four core custom C# scripts:

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
