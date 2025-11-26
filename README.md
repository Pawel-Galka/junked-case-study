# JUNKED – Student Game Project (Case Study)

**JUNKED** is a student game project about metal detecting and junk hunting, created in a 6–person team at Collegium Da Vinci.  

The game is still a **work in progress** and under active development.  
This repository does not contain the full game or any source code – it is a **high–level overview of the systems I designed and implemented as the sole programmer on the project**.

---

## My role in the project

- Sole programmer / gameplay developer in a team of 6 people (design, art, audio, programming).
- Responsible for:
  - Core player controller and camera setup.
  - Metal detector behaviour and procedural signal generation.
  - Digging and uncovering items from the ground.
  - Inventory and item data (definitions, rarity, world pickups).
  - Dialogue and quest systems based on ScriptableObjects.
  - Basic world reset and rarity distribution logic.
  - Integrating all these systems into one coherent gameplay loop.
- Repository and project administration in Unity Version Control (Plastic SCM).
- Working in an **Agile** style:
  - weekly sprints,
  - monthly milestones,
  - planning and tracking tasks in tools like Jira and Trello,
  - regular builds for the team to test and review.

---

## High–level gameplay

The core fantasy of JUNKED is:

> You explore an area with a metal detector, listen to the signal, decide where to dig, and uncover items of different rarity and composition.

Key gameplay elements:

- **Exploration** – walking through zones with different loot distributions.
- **Metal detecting** – using a handheld detector to search for buried objects.
- **Digging** – deciding where to dig based on the signal and environment.
- **Collecting items** – storing discovered objects in an inventory.
- **Dialogue and quests** – talking to NPCs, taking and turning in quests that require specific items.
- **World refresh** – over time, the world can be reset so that new items can be found.

---

## Systems I implemented

### 1. Player and camera

- First person style movement built on Unity's character controller.
- Mouse–look camera with clamped vertical rotation.
- Additional camera polish:
  - head–bob effect when walking and running,
  - small FOV kick when sprinting,
  - smooth transitions to keep the movement feeling responsive but not jittery.

These systems are tuned using exposed variables so that designers can adjust speed, gravity, jump feel and camera behaviour without touching the code.

---

### 2. Metal detector and procedural signal

This is the heart of the project.

I implemented a metal detector system that:

- Samples a procedural noise field under the player and around the detector coil.
- Uses multiple layers of noise to simulate different types of metals and buried objects.
- Feeds this signal into:
  - audio tones (higher frequency and volume for stronger signals),
  - digging logic (where items can be found and what type they are).

Key elements:

- **Config data** in ScriptableObjects:
  - detector parameters (noise scales, sampling radius, update rate),
  - audio parameters for different metal types.
- **Noise sampling**:
  - multi–octave Perlin noise to create interesting patterns underground,
  - per–zone randomization for replayability.
- **Zones and dig masks**:
  - designer–defined zones with different loot behaviour,
  - dig masks that remember where the player has already dug, so items do not respawn in the same holes.
- **Procedural audio**:
  - a simple tone generator driven by detector values,
  - mixes signals for iron, copper, gold etc. into a single audio output.

---

### 3. Digging and uncovering items

The digging system connects the detector signal to actual items in the world.

Responsibilities:

- Check the ground under the player and around the detector.
- Decide, based on noise and zone configuration, what type of item should appear.
- Use dig masks so that repeated digging in the same place does not spawn infinite loot.
- Spawn a world item that the player can then pick up.

This creates a loop:

1. Detector reacts to something under the ground.
2. Player chooses a spot and digs.
3. An item is spawned with some rarity and composition.
4. The item can be picked up and stored in the inventory.

---

### 4. Inventory, items and rarity

I implemented a lightweight inventory and item data layer to support the metal detecting loop.

Key ideas:

- **Item definitions** (ScriptableObjects):
  - id, display name, icon, category (iron, copper, gold, relic, junk, tools, etc.).
- **Item instances** (runtime):
  - unique ID for each discovered object,
  - composition values (how much iron/copper/gold, quality, etc.).
- **Rarity system**:
  - rarity tiers (Common, Uncommon, Rare, Epic, Legendary, Artifact),
  - per–zone rarity distributions that influence what the player can find in a given area.
- **World pickups**:
  - world items represent physical objects lying on the ground,
  - they can be picked up and transferred into the player's inventory.

The inventory is intentionally simple but structured so it can be easily extended in future (sorting, filters, selling items, etc.).

---

### 5. Dialogue and quest system

To support NPCs and progression, I built a small dialogue and quest framework.

- **Dialogue assets**:
  - define lines, speaker names and simple operations (set a variable, start a quest, turn in a quest).
- **Dialogue variables**:
  - ScriptableObject key–value store for bool, float and string values,
  - used by dialogue logic and other systems.
- **Dialogue runtime**:
  - manages the current line, choices and state,
  - can temporarily lock player movement while a conversation is active,
  - integrates with quests and variables.
- **Quests**:
  - item–collection quests defined as assets,
  - each quest has objectives like "bring X pieces of copper junk" or "find a specific relic",
  - quest log tracks states (not taken, in progress, ready to turn in, turned in),
  - a small HUD shows progress for active quests.

This allows designers to quickly script NPCs that give and complete quests based on items found with the detector.

---

### 6. World reset and replayability

Finally, I implemented a world reset system to keep the game replayable without manually reloading scenes:

- After a configurable amount of time, the system:
  - clears dig masks,
  - randomizes noise seeds or uses a specific seed for a given run,
  - invokes events that other systems can listen to (e.g. to respawn props or update UI).

This makes the world feel dynamic: after some time, new places become interesting to explore again.

---

## Work in progress

The underlying JUNKED project is still:

- **in active development**,
- evolving as the team adds new features and content,
- being iterated on based on feedback from teachers and team members.

The systems described here are **not final** – in a future version I would like to:

- refactor some of the subsystems for cleaner separation of concerns,
- extract more generic components (for example for other detector types or quest categories),
- improve tools for designers (better in–editor visualizations and inspectors).

---

## Why this repo exists

This repository is meant to serve as a **case study** for my work on JUNKED:

- It explains what I was responsible for as the sole programmer.
- It shows how I think about structuring gameplay systems around:
  - ScriptableObjects,
  - noise and procedural logic,
  - data–driven configuration,
  - simple but expandable architecture.

No source code or game assets are included here because the full project is team–owned and still in development.
