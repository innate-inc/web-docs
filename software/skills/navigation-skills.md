# Navigation Skills

Navigation skills handle robot locomotion—moving from one location to another while avoiding obstacles. These skills interface with Nav2, the ROS 2 navigation stack, providing path planning, obstacle avoidance, and smooth trajectory execution.

## Architecture

Navigation skills bridge BASIC's high-level intentions and Nav2's low-level execution:

```
BASIC (goal: "go to kitchen")
       |
       v
NavigateToPosition skill
       |
       v
Nav2 (path planning, costmaps, velocity control)
       |
       v
Robot base (/cmd_vel)
```

## NavigateToPosition

The primary navigation skill. Accepts map-frame coordinates and navigates the robot to that position.

```python
class NavigateToPosition(Skill):
    @property
    def name(self):
        return "navigate_to_position"

    def guidelines(self):
        return (
            "Use when you need to navigate the robot to the specified position "
            "using provided x, y coordinates, and theta (yaw) angle IN RADIANS. "
            "Can be used to navigate to a specific point in the map."
        )

    def execute(self, x: float, y: float, theta: float):
        result = self.nav2_controller.go_to_position(x, y, theta)
        
        if result == TaskResult.CANCELED:
            return "Navigation canceled", SkillResult.CANCELLED
        elif result == TaskResult.SUCCEEDED:
            return f"Reached position ({x}, {y}, {theta})", SkillResult.SUCCESS
        else:
            return f"Navigation failed with result: {result}", SkillResult.FAILURE
```

### Parameters

| Parameter | Type | Description |
|-----------|------|-------------|
| `x` | float | X coordinate in map frame (meters) |
| `y` | float | Y coordinate in map frame (meters) |
| `theta` | float | Final orientation (radians) |

### Execution Flow

1. **Path Planning**: Nav2 computes a collision-free path from current position to goal. If no valid path exists, navigation fails immediately.

2. **Execution**: Robot follows the path, replanning dynamically as new obstacles appear.

3. **Progress Feedback**: Skill reports completion percentage to BASIC. At 85% completion, it signals readiness for the next action.

4. **Completion**: Robot arrives at goal and rotates to target orientation.

### Cancellation

Navigation supports immediate cancellation with smooth deceleration:

```python
def cancel(self):
    self.nav2_controller.cancel_navigation()
    return "Navigation canceled"
```

## TurnAndMove

A lightweight skill for simple movements without full path planning overhead.

```python
class TurnAndMove(Skill):
    LINEAR_SPEED = 0.1   # m/s
    ANGULAR_SPEED = 0.5  # rad/s
    
    def execute(self, action: str, value: float = None):
        if action == "turn_left":
            return self._turn(angle_degrees=value or 90, direction="left")
        elif action == "turn_right":
            return self._turn(angle_degrees=value or 90, direction="right")
        elif action == "move_forward":
            return self._move(distance=value or 0.2, direction="forward")
        elif action == "move_backward":
            return self._move(distance=value or 0.2, direction="backward")
        elif action == "move_head":
            return self._move_head(angle=value or 0)
```

### Supported Actions

| Action | Default | Description |
|--------|---------|-------------|
| `turn_left` | 90 degrees | Rotate counter-clockwise |
| `turn_right` | 90 degrees | Rotate clockwise |
| `move_forward` | 0.2m | Move forward |
| `move_backward` | 0.2m | Move backward |
| `move_head` | 0 degrees | Tilt camera (-25 to +15 degrees) |

### Head Control

The `move_head` action adjusts camera tilt:

```python
def _move_head(self, angle: float):
    angle = max(-25, min(15, angle))  # Clamp to safe range
    self.head.set_position(int(angle))
    return f"Head moved to {angle} degrees", SkillResult.SUCCESS
```

| Angle | View |
|-------|------|
| -25 degrees | Floor/objects |
| 0 degrees | Straight ahead |
| +15 degrees | Faces/shelves |

## Skill Selection Guide

| Scenario | Skill |
|----------|-------|
| Navigate to map location | `NavigateToPosition` |
| Path with obstacles | `NavigateToPosition` |
| Distance > 1-2 meters | `NavigateToPosition` |
| Voice command ("turn left") | `TurnAndMove` |
| Fine positioning | `TurnAndMove` |
| Camera adjustment | `TurnAndMove` |

## Coordinate System

All coordinates use the **map frame**—absolute positions from SLAM. This enables:

- Named locations ("kitchen at 3.2, 1.5")
- Consistent positioning across sessions
- BASIC can reference locations by name

For relative navigation ("2 meters ahead"), transform to map coordinates using odometry.
