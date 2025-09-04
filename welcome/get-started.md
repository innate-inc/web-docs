---
icon: golang
---

# Get started

Mars and BASIC introduce a new paradigm in programming robots through code, demonstrations and language.

BASIC functions through Skills and Behaviors. Skills are atomic capabilities built for MARS. These can be digital like send emails, or physical like pick up socks, navigate to room. Skills can be written as pure code using the sdk, trained policies for manipulation and navigation or both.

Behaviors give BASIC the ability to compose atomic skills and perform complex long horizon task wrapping together its skills with reasoning, memory and decision making capabilities

### Example: Security Robot

This is a quick walk through of how Mars can be programmed to patrol a house and send alerts over email

### 1. Installation

Install the Innate SDL on your workstation, or download the Innate app from your app store

```bash
pip install innate-sdk 
```

You can start coding right after by defining files in `~/skills` and `~/behaviors`

### 2. Train Skill to Open Doors

This is best done through the app, collect a new dataset of demonstrations, choose your parameters and train on our servers

### 3. Email Skill

Through code add a Skill that gives the robot the ability to send emails to different users. Define a guideline for how to use the skill and provide a method to execute

```python
"""
Example: Minimal SendEmail skill
----------------------------------
This snippet demonstrates how to build a simple Skill class
that sends an email using SMTP. It is designed for documentation
purposes: clean, minimal, and easy to follow.
"""

import os, smtplib
from email.mime.text import MIMEText
from brain_client.primitives.types import Skill, SkillResult


class SendEmail(Skill):
    # Every skill should define a name property
    @property
    def name(self):
        return "send_email"

    # Provide short usage guidelines
    def guidelines(self):
        return "Send an emergency email. Provide subject, message, and recipients."

    def __init__(self):
        # Configure SMTP server and credentials via environment variables
        self.smtp_server = "smtp.gmail.com"
        self.smtp_port = 587
        self.smtp_user = os.getenv("SMTP_USER", "")
        self.smtp_pass = os.getenv("SMTP_PASS", "")

    # The core method: what happens when the skill is executed
    def execute(self, subject: str, message: str, recipients):
        # Allow either a single recipient string or a list
        if isinstance(recipients, str):
            recipients = [recipients]

        # Build the email
        msg = MIMEText(message)
        msg["From"] = self.smtp_user
        msg["To"] = ", ".join(recipients)
        msg["Subject"] = subject

        # Connect to the SMTP server and send the message
        with smtplib.SMTP(self.smtp_server, self.smtp_port) as server:
            server.starttls()
            server.login(self.smtp_user, self.smtp_pass)
            server.sendmail(self.smtp_user, recipients, msg.as_string())

        # Return a simple success result
        return f"Email sent to {', '.join(recipients)}", SkillResult.SUCCESS

```

### 4. Create a Behavior

This is what describes the purpose of the robot during its execution. Define through prompting what the robot should do in different situations, add its relevant skills.

```python
class SecurityGuard(Behavior):
    def name(self) -> str:
        return "security_guard_behavior"

    def get_skills(self) -> List[str]:
        return ["navigate", "open_door", "send_email"]

    def get_prompt(self) -> str:
        return """You are a security guard robot. Patrol the house, 
        stay alert and professional, watch for intruders, open doors
         when needed, and send an email immediately if you find one."""


```

### 4. Run The Behavior

New Behaviors are automatically registered. You can run them from the app.

{% embed url="https://youtu.be/b7cNKEcER24" %}

### 5. Share

Behaviors and Skills are shareable working across robots. Show the word what you've built.

**Mars is designed for builders who want to push the boundaries of what's possible. Whether you're building a 3D printing assistant  or something entirely new, we can't wait to see what you'll create.**
