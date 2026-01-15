# Code-Based Physical Skills

Some physical capabilities are better expressed as code than learned from demonstration. Skills that coordinate multiple systems, follow explicit logic, or require deterministic behavior benefit from programmatic implementation.

## Injected Interfaces

Every skill receives three interfaces at runtime for controlling robot hardware:

| Interface | Purpose | Example |
|-----------|---------|---------|
| `self.mobility` | Base/wheel control | `send_cmd_vel(linear_x, angular_z, duration)` |
| `self.manipulation` | Arm control | `goto_joint_state([j1, j2, j3, j4, j5, j6])` |
| `self.head` | Camera tilt | `set_position(angle)` |

These interfaces abstract ROS 2 complexity—no publishers, message types, or timing management required.

```python
# Move the base
self.mobility.send_cmd_vel(linear_x=0.1, angular_z=0.0, duration=2.0)

# Move the arm
self.manipulation.goto_joint_state([0, -0.5, 1.5, -1.0, 0, 0])

# Tilt the head
self.head.set_position(angle=-10)
```

## Example: GoHome

A utility skill that returns the robot to a known safe state:

```python
from brain_client.skill_types import Skill, SkillResult

class GoHome(Skill):
    """Return the robot to its home position safely."""

    def __init__(self, logger):
        super().__init__(logger)
        self._cancelled = False
        self.home_position = (0.0, 0.0, 0.0)  # x, y, theta
        self.home_arm_pose = [0, -0.5, 1.5, -1.0, 0, 0]

    @property
    def name(self):
        return "go_home"

    def guidelines(self):
        return """
        Use this skill when:
        - Robot needs to return to home/charging position
        - Robot needs to reset to known configuration
        - Starting new task sequence from clean state

        Do not use when:
        - Robot is carrying objects
        - Obstacle blocking home position
        """

    def execute(self):
        # Move arm first (avoid collisions during navigation)
        self.logger.info("Moving arm to safe position")
        self.manipulation.goto_joint_state(self.home_arm_pose)
        
        if self._cancelled:
            return "Cancelled while moving arm", SkillResult.CANCELLED

        # Then navigate
        self.logger.info("Navigating to home position")
        x, y, theta = self.home_position
        # Navigation logic here
        
        return "Successfully returned home", SkillResult.SUCCESS

    def cancel(self):
        self._cancelled = True
        self.mobility.send_cmd_vel(0, 0)
        return "Go home cancelled"
```

This skill demonstrates **sequenced coordination**—arm repositioning before navigation to avoid collisions. This logic is explicit and cannot be learned from demonstration.

## Example: LookAround

A survey skill that rotates the robot to observe surroundings:

```python
class LookAround(Skill):
    """Rotate to survey surroundings."""

    def __init__(self, logger):
        super().__init__(logger)
        self._cancelled = False

    @property
    def name(self):
        return "look_around"

    def guidelines(self):
        return (
            "Use when robot needs to survey surroundings. "
            "Rotates in place to look in multiple directions."
        )

    def execute(self, angles: list[float] = None):
        if angles is None:
            angles = [-90, 0, 90, 180]
        
        for angle in angles:
            if self._cancelled:
                return "Survey cancelled", SkillResult.CANCELLED
            
            self.mobility.send_cmd_vel(
                linear_x=0.0,
                angular_z=0.5 if angle > 0 else -0.5,
                duration=abs(angle) / 30
            )
            time.sleep(1.0)  # Pause to observe
            self._send_feedback(f"Surveyed {angle} degrees")
        
        return "Survey complete", SkillResult.SUCCESS

    def cancel(self):
        self._cancelled = True
        self.mobility.send_cmd_vel(0, 0)
        return "Survey cancelled"
```

## Accessing Robot State

Skills can request sensor data by declaring dependencies:

```python
from brain_client.skill_types import RobotStateType

class InspectArea(Skill):
    def get_required_robot_states(self) -> list[RobotStateType]:
        return [
            RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64,
            RobotStateType.LAST_ODOM
        ]

    def update_robot_state(self, **kwargs):
        self.image = kwargs.get(RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64.value)
        self.odom = kwargs.get(RobotStateType.LAST_ODOM.value)
```

The system updates these at 50Hz while the skill runs. Always check for `None` on first access.

### Available State Types

| Type | Description |
|------|-------------|
| `LAST_MAIN_CAMERA_IMAGE_B64` | Camera frame (base64 JPEG) |
| `LAST_ODOM` | Position, orientation, velocity |
| `LAST_MAP` | Occupancy grid |
| `LAST_HEAD_POSITION` | Head tilt angle |

## Skill Type Selection

| Use Case | Recommended Type |
|----------|------------------|
| Explicit sequencing logic | Code-based |
| Coordinating multiple systems | Code-based |
| Deterministic, reproducible behavior | Code-based |
| Visual perception and adaptation | Learned |
| High environmental variation | Learned |
| Fixed, repeatable motion | Replay |

## Composing Skills

The skill system supports composition—higher-level behaviors built from skill chains:

1. `look_around` (code) - Survey environment
2. `navigate_to_position` (code) - Approach target
3. `pick_socks` (learned) - Grasp object
4. `send_email` (digital) - Report completion

Each skill focuses on one capability. BASIC orchestrates them into coherent behavior.
