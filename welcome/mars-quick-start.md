---
description: >-
  Learn how to control MARS with your phone and the controller arm, make it
  navigate and talk, and trigger autonomous behaviors & skills made by others
icon: golang
---

# MARS Quick Start

This page describes the experience of receiving your MARS robot for the first time and running it.

If you want a more technical introduction to our SDK and the underlying ROS2 system, please go to:

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}



## A quick overview

{% embed url="https://youtu.be/_Cw5fGa8i3s" %}



## Powering up and connecting to MARS

MARS turns on automatically when plugging the battery in. Put it on the floor, plug the battery, and open the Innate Controller App (works on iPhone and Android).



The app is available <i class="fa-google-play">:google-play:</i> [here for android](https://innate.bot/) and <i class="fa-app-store-ios">:app-store-ios:</i> [here for ios](https://innate.bot/).



{% embed url="https://youtu.be/lbwYFGgOh-g" %}



## Experience the phone control

Once connected, you can verify that MARS is properly running by controling the robot straight through the app.

1. Go to manual control
2. Make the robot move with the joystick and the head with the slider.
3. Plug the controller arm in and toggle the arm control to verify that the robot mimics your movement.



{% embed url="https://youtu.be/BhP6Z0h69Ho" %}



## Talk to MARS for the first time

MARS can run BASIC, our embodied agent that allows the robot to act in real-time and decide what to do based on what it sees and hears.&#x20;

Put MARS on a table, in front of you, go to "Agentic" on the app, and ask it what it sees. You should start seeing its thoughts and answers appear in the chat.



{% embed url="https://youtu.be/GRWwaOIKmec" %}



## Make MARS navigate

When running BASIC, MARS can navigate. On the agentic screen, ask him things such as "move forward 1m", or "go to the fridge" if the fridge is in sight.

You can also try more complex requests such as "explore until you see a human".



{% embed url="https://youtu.be/EsLdogaaA90" %}



## Use an autonomous arm AI skill

Innate robots can use "skills" to perform actions in the physical world or the digital world.

You can observe which ones are installed by going in the app to Skills (in the tab bar, middle icon) and looking at the list of physical and digital skills installed.



To run one, go back to the home screen, select manual control, and open the skills menu to pick one to use. Then press the <i class="fa-play">:play:</i> button and observe.



**VIDEO**



## Use an existing behavior

BASIC allows to run programs we call "behaviors" that determine the robot's purpose and abilities. On the app, you can see which ones are already installed.

Try out the "**Roast me**" one, the "**Find my shoes**", or the "**Pick up the trash**" straight from the app.



Before running a behavior, you can observe what it was programmed to do.



**VIDEO**



## Create your first map

Innate robots running BASIC have spatial memory, but they currently require a map for it to be fully functional, so that the robot can remember where it saw things.



To create a map, in the app, go in Settings -> Mapping and press the <i class="fa-plus">:plus:</i> button. Once the robot is mapping, move around and observe the map being created.



Once you're satisfied of how it looks like, you can save the map which will automatically activate in your robot.



**VIDEO**



## Navigate, this time with memory

Now that MARS has spatial memory, you can drive it around and it will memorize what it sees. Try to drive it around your kitchen then, in agentic mode, ask "go to the fridge" and observe it going there.



**VIDEO**



## Congrats, you can control your robot!

Now you know how to run basic controls of the robot from the app.

<i class="fa-arrow-right">:arrow-right:</i> Next up: Create your first behavior and train your first manipulation model, to run them autonomously!

{% content-ref url="mars-quick-development.md" %}
[mars-quick-development.md](mars-quick-development.md)
{% endcontent-ref %}



## You have completed the quick start!

Congrats!

You can now create basic behaviors that allow the robot to interact with the world. There is a lot more to try.

You can create additional skills like "wave" and "navigate" to use the arm for learned actions or have the arm access the internet to use Google, send emails, call other agents...



Go to the following pages to learn more:

PAGE FOR TEACHING THE ARM A SKILL

PAGE TO WRITE SKILLS WITH CODE

PAGE TO TEACH AN ARM SKILL WITH INVERSE KINEMATICS

PAGE FOR SHARING A BEHAVIOR

ADVANCED: USE AND MODIFY THE CORE ROS2 OS

ADVANCED: IMPLEMENTING A DIFFERENT MANIPULATION MODEL





Innate robots introduce a new paradigm in programming robots through code, demonstrations and language.

Our robots run through **skills and behaviors.**

**Skills** are atomic capabilities built for MARS. These can be digital written in code, like sending emails, or physical like pick up socks, navigate to room.

Skills can be written as **pure code** using the sdk, trained models for manipulation (**VLAs**) and navigation or both.



**Behaviors** are like an app for your robot.

A BASIC behavior is a composition of atomic skills with a system prompt. A robot running on a behavior performs complex long horizon task wrapping together its skills with reasoning, memory and decision making capabilities.

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

### 5. Run The Behavior

New Behaviors are automatically registered. You can run them from the app.

{% embed url="https://youtu.be/b7cNKEcER24" %}

### 6. Share

Behaviors and Skills are shareable working across robots.&#x20;

You can simply share the files in a github repository like our [examples repository](https://app.gitbook.com/u/zOCxacgyelY4F6REjthFZpRxvMC2).



**Mars is designed for builders who want to push the boundaries of what's possible. Whether you're building a 3D printing assistant  or something entirely new, we can't wait to see what you'll create.**
