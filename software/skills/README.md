# Skills

Skills are atomic robot capabilities that BASIC chains together to accomplish complex, long-horizon behaviors. Each skill encodes a single capability—physical (manipulation, navigation) or digital (emails, API calls)—that can be combined with others to form coherent action sequences.

When BASIC receives a request like "check on grandma," it decomposes this into a skill chain: navigate to bedroom → look around → send picture via email → speak reassurance. Four skills, one coherent behavior.

## Core Interface

Every skill implements three methods that BASIC uses for orchestration:

| Method | Purpose |
|--------|---------|
| `guidelines()` | Tells BASIC when and how to use the skill |
| `execute()` | Performs the skill's action, returns result and status |
| `cancel()` | Safely interrupts execution at any point |

### Skill Template

```python
from brain_client.skill_types import Skill, SkillResult

class MySkill(Skill):
    def __init__(self, logger):
        super().__init__(logger)
    
    @property
    def name(self):
        return "my_skill"  # Unique identifier
    
    def guidelines(self):
        return """
        Use this skill when:
        - [Specific situation 1]
        - [Specific situation 2]
        
        Do not use when:
        - [Limitation 1]
        - [Limitation 2]
        """
    
    def execute(self, param1: str, param2: float = 1.0):
        # Perform the skill's action
        return "Result message", SkillResult.SUCCESS
    
    def cancel(self):
        # Stop gracefully, leave robot in safe state
        return "Cancelled"
```

Skills are auto-discovered: drop a Python file in the `skills/` directory and the system loads it on startup.

## Skill Types

Skills divide into two categories based on how they're defined:

### Code Skills

Python classes that implement explicit logic. Used for:
- Navigation (path planning, movement)
- Digital operations (emails, API calls)
- Any behavior expressible as code

### Physical Skills

Defined by `metadata.json` plus associated data files. Two variants:

| Type | Definition | Use Case |
|------|------------|----------|
| **Learned** | Neural network checkpoint (ACT policy) | Manipulation tasks requiring visual adaptation |
| **Replay** | Recorded action sequence (H5 file) | Repeatable gestures and motions |

Physical skills are created through demonstration rather than programming—show the robot what to do, and it learns the behavior.

## Skill Categories

The following pages cover each skill category in detail:

- [**Navigation Skills**](navigation-skills.md) — Moving through space with Nav2 integration
- [**Manipulation Skills**](manipulation-skills.md) — Learned and replay physical skills
- [**Digital Skills**](digital-skills.md) — Email, API, and external service integration
- [**Code-Based Physical Skills**](more-physical-skills.md) — Custom physical behaviors in Python

Each page includes working examples from the codebase and guidance on creating new skills.

## Extensibility

The skill system is designed for growth. New capabilities are added by:
1. Dropping a Python file in `skills/` (code skills)
2. Creating a `skills/<name>/metadata.json` directory (physical skills)

No registration or configuration required. The robot discovers and loads new skills automatically.
