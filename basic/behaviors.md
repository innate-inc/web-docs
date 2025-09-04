# Behaviors

Behaviors steer BASIC to perform complex long horizon tasks. Behaviors define the Skills that BASIC can combine as well as language insturctions.

## Defining Behaviors

Defining a Behavior requires two components.

1. List of [skills.md](skills.md "mention") that [Broken link](broken-reference "mention") has access to
2. A natural language prompt that instructs BASIC how to perform the task

```python
from innate.behavior import Behavior
from typing import List


class BasicBehavior(Behavior):
    def get_skills(self) -> List[str]:
        """
        List the Skills this behavior can call.
        Return: list of skill names (snake_case)
        """
        return [
        "skill_one",
        "skill_two",
        ]


    def get_prompt(self) -> str:
        """
        Define how BASIC should use the Skills.
        Return: instructions shown to the agent.
        """
        return """You are Mars, a capable robot. You have these skills available:


        skill_one:
        - Used for [purpose]
        - Parameters: [list parameters if any]
        - Use when [describe situations]


        skill_two:
        - Used for [purpose]
        - Parameters: [list parameters if any]
        - Use when [describe situations]


        For each request:
        1. Choose the appropriate skill
        2. Extract any needed parameters
        3. Execute the skill
        4. Provide clear, concise feedback"""
```

## Using Behaviors

### Option 1: Command Line (Innate SDK CLI)

```bash
# Activate a behavior
innate behavior activate <behavior_name>


# Deactivate all behaviors
innate behavior deactivate
```

### Option 2: Mars App

1. Connect to your robot in the **Mars** app
2. On the **Home** screen, locate the **Behaviors** section
3. Tap a behavior to activate it

## Examples

### Clean Socks Behavior

```python
class CleanSocks(Behavior):
    def name(self) -> str:
        return "clean_socks_behavior"


    def get_skills(self) -> List[str]:
        return ["navigate", "pick_up_sock", "drop_socks"]

    def get_prompt(self) -> str:
        return """You are a sock-cleaning robot. Search the room for socks.
        When you see one, go to it, pick it up, and put it in the wooden box.
         Keep doing this until there are no more socks."""
```

### Security Guard Behavior

```python
class SecurityGuard(Behavior):
    def name(self) -> str:
        return "security_guard_behavior"


    def get_skills(self) -> List[str]:
        return ["navigate", "open_door", "send_email"]

    def get_prompt(self) -> str:
        return """You are a security guard robot. Patrol the house,
         stay alert and professional, watch for intruders, 
         open doors when needed, and send an email immediately 
         if you find one."""
```
