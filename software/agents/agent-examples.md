# Agent examples

## Examples

### Clean Socks Agent

```python
from typing import List
from brain_client.agent_types import Agent

class CleanSocksAgent(Agent):
    @property
    def id(self) -> str:
        return "clean_socks"

    @property
    def display_name(self) -> str:
        return "Clean Socks"

    @property
    def display_icon(self) -> str:
        return "assets/clean_socks.png"

    def get_skills(self) -> List[str]:
        return ["navigate_to_position", "pick_up_sock", "drop_socks"]

    def get_prompt(self) -> str:
        return """You are a sock-cleaning robot. Search the room for socks.
        When you see one, go to it, pick it up, and put it in the wooden box.
        Keep doing this until there are no more socks."""
```

### Security Guard Agent

```python
from typing import List
from brain_client.agent_types import Agent

class SecurityGuardAgent(Agent):
    @property
    def id(self) -> str:
        return "security_guard_directive"

    @property
    def display_name(self) -> str:
        return "Security Guard"

    @property
    def display_icon(self) -> str:
        return "assets/security_guard.png"

    def get_skills(self) -> List[str]:
        return ["navigate_to_position", "open_door", "send_email"]

    def get_prompt(self) -> str:
        return """You are a security guard robot. Patrol the house,
         stay alert and professional, watch for intruders, 
         open doors when needed, and send an email immediately 
         if you find one."""
```
