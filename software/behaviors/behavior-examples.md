# Behavior examples

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
