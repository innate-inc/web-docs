---
icon: brain
---

# Orchestrator

## Overview

The Orchestrator is Maurice's AI agent - a reactive physical intelligence that interacts with the world through robot operations and user-defined primitives. When users give natural language instructions, the Orchestrator intelligently selects and executes appropriate robot behaviors to accomplish the requested tasks.

The Orchestrator acts as a bridge between user intentions and robot actions by:

* Understanding natural language requests
* Selecting appropriate primitives for tasks
* Coordinating physical and digital operations
* Providing feedback about actions and outcomes

## Extending with Directives

Builders can customize and extend the Orchestrator's capabilities through Directives. A Directive tells the Orchestrator:

1. Which primitives it can use
2. How to understand when and how to use them

### Creating Directives

#### Basic Structure

A Directive consists of two key components:

```python
from innate.directive import Directive
from typing import List

class BasicDirective(Directive):
    def get_primitives(self) -> List[str]:
        """
        Define which primitives this directive can use
        Returns: List of primitive names
        """
        return [
            "primitive_one",
            "primitive_two"
        ]

    def get_prompt(self) -> str:
        """
        Define how to use the primitives
        Returns: String with usage instructions
        """
        return """You are Maurice a smart robot. You have these primitives available:

primitive_one:
- Used for [purpose]
- Parameters: [list parameters if any]
- Use when [describe situations]

primitive_two:
- Used for [purpose]
- Parameters: [list parameters if any]
- Use when [describe situations]

For each request:
1. Choose the appropriate primitive
2. Extract any needed parameters
3. Execute the primitive
4. Provide clear feedback"""

```

When creating a Directive:

1. Create a Python file in the `~/directives` directory
2. Inherit from the Directive base class
3. Implement get\_primitives() to list available primitives
4. Implement get\_prompt() to define usage instructions

### Using Directives

You can activate and deactivate directives in two ways:

#### Option 1: Command Line Interface

Use the Maurice SDK CLI commands:

```bash
# Activate a directive
innate directive activate directive_name

# Deactivate all directives
innate directive deactivate

```

#### Option 2: Maurice App

1. Connect to your robot through the Maurice app
2. Navigate to the Directives page
3. Click your desired directive to activate it
4. Hit the cancel button to deactivate the current directive

When a directive is activated, the Orchestrator will use its configuration to understand and respond to user requests.

## Examples

### Maurice Security System

Here's a complete example showing how to build a simple security system using the Orchestrator:

#### 1. Creating the Notify User Primitive

File: `~/primitives/notify_user.py`

```python
from innate.primitive import Primitive
from typing import Tuple
import smtplib
from email.message import EmailMessage

class NotifyUser(Primitive):
    def __init__(self):
        super().__init__()
# Email configuration
        self.smtp_server = "smtp.gmail.com"
        self.smtp_port = 587
        self.sender_email = "maurice@example.com"
        self.user_email = "user@example.com"
        self.sender_password = "your-app-password"

    def guidelines(self) -> str:
        return """
        Use this primitive to notify the user via email about important events or findings.
        Do not use for routine or non-critical notifications.
        """

    def execute(self, description: str) -> Tuple[str, bool]:
        try:
            msg = EmailMessage()
            msg.set_content(description)
            msg['Subject'] = 'Security Alert from Maurice'
            msg['From'] = self.sender_email
            msg['To'] = self.user_email

            with smtplib.SMTP(self.smtp_server, self.smtp_port) as server:
                server.starttls()
                server.login(self.sender_email, self.sender_password)
                server.send_message(msg)

            return f"Notification sent: {description}", True
        except Exception as e:
            return f"Failed to send notification: {str(e)}", False

```

#### 2. Creating the Patrol Primitive

File: `~/primitives/patrol.py`

```python
from innate.primitive import Primitive
from innate import navigation
from typing import Tuple
import time

class Patrol(Primitive):
    def __init__(self):
        super().__init__()
        navigation.init()
        self.locations = ["kitchen", "living_room", "door"]

    def guidelines(self) -> str:
        return """
        Use this primitive to perform a security patrol of the house.
        The robot will visit key locations in sequence.
        """

    def execute(self) -> Tuple[str, bool]:
        try:
            for location in self.locations:
                navigation.go_to_memory(location)
                time.sleep(5)

            navigation.go_home()
            return "Patrol completed successfully", True
        except Exception as e:
            return f"Patrol interrupted: {str(e)}", False

    def interrupt(self):
        navigation.interrupt()

```

#### 3. Creating the Security Directive

File: `~/directives/security_directive.py`

```python
from innate.directive import Directive
from typing import List

class SecurityDirective(Directive):
    def get_primitives(self) -> List[str]:
        return [
            "patrol",
            "notify_user"
        ]

    def get_prompt(self) -> str:
        return """You are Maurice's security system. You monitor the house and notify the owner of any issues.

Available primitives:

patrol:
- Used to inspect key areas of the house
- No parameters needed
- Use when:
  - Asked to check the house
  - Regular security rounds are needed
  - Suspicious activity reported

notify_user:
- Used to send important alerts to the owner
- Parameters: description (string) - what to notify about
- Use when:
  - Suspicious activity is detected
  - Important events need attention
  - After patrol if issues found

For each request:
1. If asked to check the house, use patrol
2. If something suspicious is found, use notify_user
3. Always explain what you're doing
4. Maintain security awareness at all times

Safety is the top priority. If anything seems unsafe, notify the user immediately."""

```

#### Using the Security System

1. Place files in correct directories:
   * Put primitives in `~/primitives/`
   * Put directive in `~/directives/`
2. Activate the security directive using either:

```
innate directive activate security_directive
```

Or use the Maurice app:

* Navigate to the Directives page
* Click on "security\_directive"

3\. The robot can now handle commands like: - "Check the house" - "Do a security patrol" - "Alert me if anything seems wrong"

The Orchestrator will use the directive's prompt to understand these requests and execute the appropriate primitives in response.
