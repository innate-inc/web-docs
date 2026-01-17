# Navigation

MARS supports multiple navigation modes to suit different use cases. You can control navigation through the Innate Controller App.

## Navigation Modes

### Map-Free Mode (Default)

In map-free mode, the robot uses its local costmap (obstacle detection from sensors) to navigate without a pre-built map. This is ideal for:

- Quick deployment in new environments
- Spaces that change frequently
- Simple point-to-point navigation

The app displays a local view around the robot showing detected obstacles in real-time.

### Mapping Mode

Before using full autonomous navigation, you can create a map of your space:

1. Open the Innate Controller App
2. Go to **Configuration** → **Mapping** tab
3. Tap **Create New Map**
4. Follow the on-screen instructions

**Safety tips during mapping:**

- Place the robot's arm in a safe resting position before starting
- Drive slowly and cover each room thoroughly
- Avoid sudden movements that could affect sensor readings

### Navigation with Maps

Once you have a saved map, the robot can navigate autonomously to saved locations or positions you specify.

## Go-To Mode

The app includes a **Go-To Mode** that lets you visually select a destination:

1. Open the map view in the app
2. Enable Go-To Mode
3. Tap and drag on the map to set a goal position and orientation
4. The robot will plan a path and navigate to the goal

The app displays the planned trajectory as a green line on the map, updating in real-time as the robot moves.

## Switching Navigation Modes

You can switch between navigation modes through the app. The current mode is displayed in the app interface, and the robot will automatically adjust its behavior based on the selected mode.

## For Developers

Navigation modes are managed via ROS 2 topics. The current navigation mode is published on `/nav/current_mode` with values:

- `mapfree` - Map-free local navigation
- `mapping` - Creating a new map
- `navigation` - Navigating with a saved map

See [ros2-reference.md](../../software/advanced/ros2-reference.md "mention") for more details on the navigation interfaces.
