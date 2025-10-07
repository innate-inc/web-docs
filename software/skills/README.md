# Skills

Skills are atomic robot capabilities that are combined and chained by BASIC to enable complex long horizon behaviors. Skills can code for physical capabilities ( Manipulation, Navigation etc), digital capabilities (emails, data processing, API calls) or both.

Defining a skill requires three main components which BASIC can interact with

1. Guidelines: Provides BASIC with language instruction for when and how to use the capabilities encoded in the Skill
2. Execution: Provides a method for BASIC to call to execute the capability encoded
3. Cancellation: Provides a method for BASIC safely and reactively cancel execution when appropriate.

## Defining Skills

Below is a sample skill.

```python
from innate.skill import Skill
from typing import Tuple

class MySkill(Skill):
    def __init__(self):
        """
        Initialize the skill
        """
        super().__init__()  # Required: initialize parent class

    def guidelines(self) -> str:
        """
        Define usage guidelines for when this skill should be used
        Returns: string describing use cases and restrictions
        """
        return """
        Use this skill when:
        - [Describe when to use this skill]
        - [List relevant conditions]

        Do not use when:
        - [Describe when not to use this skill]
        - [List restrictions or limitations]
        """

    def execute(self) -> Tuple[str, bool]:
        """
        Main execution logic
        Returns: (feedback string, success boolean)
        """
        try:
            # Implement your skill’s logic here
            return "Task completed successfully", True
        except:
            return "Task failed", False

    def interrupt(self):
        """
        Define how to safely stop execution
        """
        # Implement safe stopping behavior here

```

## Examples

### Go Home Skill

**\~/skills/go\_home.py**

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

### Pick Trash Skill

**\~/skills/pick\_trash.py**

```python
from innate.skill import Skill
from innate import navigation, manipulation
import time
from typing import Tuple

class PickTrash(Skill):
    """
    Vision-guided picking of a described trash item.
    Combines in-sight navigation with a grasping policy.
    """

    def __init__(self):
        super().__init__()
        navigation.init()
        manipulation.init()

    def guidelines(self) -> str:
        return """
        Use this skill when:
        - You need to pick up trash or debris
        - The trash item is visible to the robot
        - The item is within the robot's manipulation range

        Do not use when:
        - The trash is too heavy (>200 g)
        - The trash is hazardous material
        - Multiple items need to be picked at once
        - The item is not clearly visible
        """

    def execute(self, description: str) -> Tuple[str, bool]:
        try:
            # 1) Move to a vantage point where item is in view
            navigation.go_to_in_sight(description, distance=0.2)

            # 2) Pre-grasp pose (example values; tune per robot)
            manipulation.set_ee_pose({"x": 0.30, "y": 0.00, "z": 0.10})
            time.sleep(0.5)

            # 3) Run a policy named "pick_item"
            manipulation.run_policy("pick_item")

            return f"Successfully picked up {description}", True
        except Exception as e:
            return f"Failed to pick up {description}: {e}", False

    def interrupt(self):
        # Halt movement and open gripper for safety
        navigation.interrupt()
        manipulation.interrupt()
        manipulation.set_gripper_pressure(0.0)

```

### Capture and Upload Image Skill

**\~/skills/capture\_and\_upload.py**

```python
from innate.skill import Skill
from innate import sensors
from typing import Tuple
from datetime import datetime
import io
import cv2
import numpy as np
from PIL import Image

from googleapiclient.discovery import build
from googleapiclient.http import MediaIoBaseUpload
from google.oauth2 import service_account


class CaptureAndUpload(Skill):
    """
    Capture an RGB image from the robot sensors and upload it
    to Google Drive (or Google Photos with a different API call).
    """

    def __init__(self):
        super().__init__()
        sensors.init()

        # Configure Google API (using a service account JSON)
        SCOPES = ["https://www.googleapis.com/auth/drive.file"]
        SERVICE_ACCOUNT_FILE = "/path/to/credentials.json"
        creds = service_account.Credentials.from_service_account_file(
            SERVICE_ACCOUNT_FILE, scopes=SCOPES
        )
        self.drive_service = build("drive", "v3", credentials=creds)

    def guidelines(self) -> str:
        return """
        Use this skill when:
        - You need to capture a visual snapshot from the robot
        - You want the image archived to cloud storage (Drive/Photos)

        Do not use when:
        - Sensors are not initialized
        - Network connectivity is unavailable
        """

    def execute(self) -> Tuple[str, bool]:
        try:
            # 1) Capture RGB image
            rgb_image, _ = sensors.get_rgbd()

            # 2) Convert to JPEG in memory
            image = Image.fromarray(cv2.cvtColor(rgb_image, cv2.COLOR_BGR2RGB))
            buffer = io.BytesIO()
            image.save(buffer, format="JPEG")
            buffer.seek(0)

            # 3) Upload to Drive
            timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
            file_metadata = {"name": f"robot_capture_{timestamp}.jpg"}
            media = MediaIoBaseUpload(buffer, mimetype="image/jpeg")

            uploaded = (
                self.drive_service.files()
                .create(body=file_metadata, media_body=media, fields="id")
                .execute()
            )

            return f"Image captured and uploaded to Drive (file ID: {uploaded['id']})", True
        except Exception as e:
            return f"Failed to capture/upload image: {e}", False

    def interrupt(self):
        # No long-running process; nothing special needed
        pass

```
