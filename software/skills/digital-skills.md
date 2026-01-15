# Digital Skills

Digital skills enable the robot to interact with external services—sending emails, making API calls, retrieving information. These are always code skills, implementing explicit protocols and handling authentication, errors, and network conditions.

## Characteristics

Unlike physical skills that deal with real-world variability, digital skills operate in a deterministic domain:

- **Protocol-based**: Follow defined APIs and standards
- **Atomic**: Many operations cannot be cancelled once started
- **Reliable**: Once working, behavior is consistent
- **Network-dependent**: Must handle connectivity issues

## Built-in Skills

### SendEmail

Sends email notifications, typically for alerts or status updates.

```python
class SendEmail(Skill):
    @property
    def name(self):
        return "send_email"

    def guidelines(self):
        return (
            "Use to send an emergency email notification. Provide a subject and "
            "message. You can optionally provide a list of recipients, otherwise "
            "it will be sent to the default list. This should be used when a "
            "potential emergency is detected and assistance might be required."
        )

    def execute(self, subject: str, message: str, recipients: list[str] = None):
        # Send via SMTP
        # Returns (message, SkillResult)
```

**Parameters:**

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `subject` | str | Yes | Email subject line |
| `message` | str | Yes | Email body content |
| `recipients` | list[str] | No | Recipients (defaults to configured list) |

### SendPictureViaEmail

Sends an email with the robot's current camera view attached.

```python
class SendPictureViaEmail(Skill):
    def get_required_robot_states(self) -> list[RobotStateType]:
        return [RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64]

    def update_robot_state(self, **kwargs):
        self.last_main_camera_image_b64 = kwargs.get(
            RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64.value
        )

    def execute(self, subject: str, message: str, recipient: str = None):
        if not self.last_main_camera_image_b64:
            return "No image available to send", SkillResult.FAILURE
        # Attach image and send
```

This skill demonstrates **state dependencies**—declaring required robot state that the system provides at 50Hz.

### RetrieveEmails

Fetches recent emails from configured account.

```python
class RetrieveEmails(Skill):
    def guidelines(self):
        return (
            "Use to retrieve recent emails from the configured email account. "
            "Provide the number of emails to retrieve (default is 5). "
            "Returns email subjects and content."
        )

    def execute(self, count: int = 5):
        # Connect to IMAP, fetch emails
        # Returns formatted email list
```

## Creating Digital Skills

### Template

```python
import os
from brain_client.skill_types import Skill, SkillResult

class MyDigitalSkill(Skill):
    def __init__(self, logger):
        super().__init__(logger)
        self.api_key = os.environ.get("SERVICE_API_KEY")
        if not self.api_key:
            raise ValueError("SERVICE_API_KEY not configured")

    @property
    def name(self):
        return "my_digital_skill"

    def guidelines(self):
        return "Use when [describe appropriate use cases]"

    def execute(self, param: str):
        try:
            # Call external service
            result = self._call_service(param)
            return f"Success: {result}", SkillResult.SUCCESS
        except TimeoutError:
            return "Service timed out", SkillResult.FAILURE
        except Exception as e:
            return f"Error: {e}", SkillResult.FAILURE

    def cancel(self):
        return "Operation cannot be cancelled"
```

### Best Practices

**Authentication**
- Store credentials in environment variables or secret managers
- Never hardcode passwords or API keys
- Validate credentials at initialization

**Error Handling**
```python
def execute(self, query: str):
    try:
        response = self.client.call(query, timeout=10)
        return f"Result: {response}", SkillResult.SUCCESS
    except RateLimitError:
        return "Rate limit exceeded", SkillResult.FAILURE
    except NetworkError:
        return "Network unavailable", SkillResult.FAILURE
    except Exception as e:
        return f"Unexpected error: {e}", SkillResult.FAILURE
```

**Timeouts**
- Always set explicit timeouts on network calls
- Prevent blocking indefinitely on slow services

**Idempotency**
- Design operations to be safely retryable when possible
- Consider partial failure scenarios

## Requesting Robot State

Skills can declare dependencies on robot sensor data:

```python
from brain_client.skill_types import RobotStateType

class MySkill(Skill):
    def get_required_robot_states(self) -> list[RobotStateType]:
        return [RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64]

    def update_robot_state(self, **kwargs):
        self.image = kwargs.get(RobotStateType.LAST_MAIN_CAMERA_IMAGE_B64.value)
```

Available state types:

| State Type | Description |
|------------|-------------|
| `LAST_MAIN_CAMERA_IMAGE_B64` | Latest camera frame (base64 JPEG) |
| `LAST_ODOM` | Current odometry |
| `LAST_MAP` | Occupancy grid |
| `LAST_HEAD_POSITION` | Head tilt angle |

## Cancellation

Many digital operations are atomic and cannot be meaningfully cancelled:

```python
def cancel(self):
    return "Email sending is an atomic operation that cannot be canceled once started"
```

BASIC understands this limitation and factors it into planning decisions.
