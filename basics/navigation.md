---
icon: compass
---

# Navigation

## Overview

Maurice operates using a dual-system navigation approach:

#### **Core Navigation System**

* Uses RGBD+LiDAR SLAM for localization
* Creates and maintains an occupancy map for obstacle-free path planning
* Continuously stores and updates a pose-graph representation of the environment for semantic navigation

#### **Navigation Methods**

&#x20;Maurice can navigate in three ways:

* By coordinates
* Through text prompts to stored locations
* Through text prompts to visible locations (in-sight navigation)

For successful navigation, Maurice requires both:

* An occupancy map (for understanding free/occupied space)
* A pose-graph (for understanding the environment's structure)

The system continuously updates its environmental understanding through the pose-graph representation, allowing Maurice to maintain an accurate model of its surroundings for navigation purposes.

## Setup

### Slam Occupancy Map

* **Initial Position**
  * Place Maurice in a repeatable starting position
  * This position will serve as the map's origin
  * Note: Choose this position carefully as it will be your reference point
* **Start Mapping**
  * Execute command: `maurice-sdk map new`
  * Enter your desired map name when prompted
  * A visualization window will appear showing:
    * Current map generation
    * Sensor data window
* **Mapping Process**
  * Drive Maurice through the environment using either:
    * The mobile app
    * WASD keys on your keyboard
  * Mapping best practices:
    * Cover all areas where Maurice will operate
    * Perform regular 360-degree turns to avoid sensor blind spots
    * Revisit key regions multiple times to improve map accuracy
* **Save Map**
  * Press escape to finish mapping
  * The map will automatically save to `~/maps`
* **View Map**
  *   To view your created map, use command:

      ```bash
      maurice-sdk map view map_name
      ```

### Pose Graph Map

1. **Initial Setup**
   * Place Maurice in the same origin position used for the occupancy map
   *   Load your previously created map using:

       ```
       maurice-sdk map load map_name
       ```
2. **Create New Pose-Graph**
   *   Start the pose-graph creation process:

       ```
       maurice-sdk pose-graph new pg_name
       ```
3. **Environment Coverage**
   * Drive Maurice through the environment
   * Ensure the robot observes:
     * All key rooms
     * Important objects relevant to planned tasks
   * The robot will automatically:
     * Build its proprietary pose-graph representation
     * Create a navigable understanding of the environment
4.  **Managing the Pose-Graph**

    *   Save the pose-graph:

        ```
        maurice-sdk pose-graph save pg_name
        ```
    *   View the pose-graph:

        ```
        maurice-sdk pose-graph view pg_name
        ```
    * Load a pose-graph for use:

    Note: The `-dynamic` argument controls whether the pose-graph:

    ```sh
     maurice-sdk pose-graph load pg_name --dynamic False
     
     #`True`: Continuously updates with new environmental information
     #`False`: Remains static using only saved information
    ```

## Using Navigation

### Python SDK

**Setup**

```python

from maurice_sdk import navigation
# Initialize navigation module
navigation.init()
```

**Get Current Pose**

```python
current_pose = navigation.get_current_pose()
# Returns: tuple (x, y, theta)# Example: (1.5, -0.3, 0.785)
```

**Go to Pose**

```python
navigation.go_to_pose(x, y, theta)
# Parameters:#   x: float - x coordinate in meters#   y: float - y coordinate in meters#   theta: float - orientation in radians
```

**Go to Memory**

```python
navigation.go_to_memory("text prompt")
# Parameter:#   text prompt: str - description of saved location# Example: navigation.go_to_memory("kitchen counter")
```

**Go to In Sight**

```python
navigation.go_to_in_sight("text prompt", distance)
# Parameters:#   text prompt: str - description of visible location/object#   distance: float - desired distance from object in meters# Example: navigation.go_to_in_sight("blue chair", 0.5)
```

**Interrupt Navigation**

```python
navigation.interrupt()
# Immediately stops current navigation command# Can be used to halt any ongoing navigation task
```

### CLI

**Get Current Pose**

```bash
maurice-sdk navigation get-current-pose
# Returns: x y theta# Example output: 1.5 -0.3 0.785
```

**Go to Pose**

```bash
maurice-sdk navigation go-to-pose <x> <y> <theta>
# Parameters:#   x: x coordinate in meters#   y: y coordinate in meters#   theta: orientation in radians# Example: maurice-sdk navigation go-to-pose 1.5 -0.3 0.785
```

**Go to Memory**

```bash
maurice-sdk navigation go-to-memory "text prompt"
# Parameter: text description of saved location# Example: maurice-sdk navigation go-to-memory "kitchen counter"
```

**Go to In Sight**

```bash
maurice-sdk navigation go-to-in-sight "text prompt" <distance>
# Parameters:#   text prompt: description of visible location/object#   distance: desired distance from object in meters# Example: maurice-sdk navigation go-to-in-sight "blue chair" 0.5
```

**Interrupt Navigation**

```bash
maurice-sdk navigation interrupt
# Immediately stops current navigation command
```
