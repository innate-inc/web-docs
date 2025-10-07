# More physical skills

You can create additional physical skills with code by using lower-level APIs such as navigating to a specific pose:

```python
from innate.skill import Skill
from innate import navigation, manipulation
import numpy as np
from typing import Tuple

class GoHome(Skill):
    """
    Returns the robot to its home/base pose safely by coordinating
    manipulation and navigation.
    """

    def __init__(self):
        super().__init__()
        navigation.init()
        manipulation.init()

    def guidelines(self) -> str:
        return """
        Use this skill when:
        - Robot needs to return to its home position
        - Robot needs to reset its configuration
        - Starting a new set of tasks

        Do not use when:
        - Robot is carrying objects
        - Path to home is blocked
        """

    def execute(self) -> Tuple[str, bool]:
        try:
            # 1) Move arm to a safe zero/park joint configuration
            home_joints = np.zeros(6)  # Adjust DOF as needed
            manipulation.set_joint_pose(home_joints)

            # 2) Navigate to absolute 'home' pose
            navigation.go_to_pose(0.0, 0.0, 0.0)

            return "Successfully returned to home position", True
        except Exception as e:
            return f"Failed to return home: {e}", False

    def interrupt(self):
        # Stop all motion channels
        navigation.interrupt()
        manipulation.interrupt()

```
