# Definitions

Behaviors are located in `~/behaviors` on your robot.

Defining a Behavior requires two components.

1. A list of [skills](../skills/ "mention") that [basic](../basic/ "mention") has access to, like picking an object, navigating, querying the internet...
2. A natural language prompt on how to perform the task
3. (Optional) a list of additional inputs the agent can take - like microphone in, air data, incoming emails...



## Template: A simple hello world

```python
from innate.behavior import Behavior
from typing import List

class HelloWorld(Behavior):
    def get_skills(self) -> List[str]:
        # List the Skills this behavior can call.
        return ["wave", "navigate"]


    def get_prompt(self) -> str:
        # Define how BASIC should use the Skills.
        return """
You are a robot who can say hello world to the user.

- Speak in lowercase. You can respond to the user.
- Don't navigate, just turn around if you don't see the user.
- Say hello world while waving if you see the user.
"""
    
    def get_inputs(self) -> List[str]:
        # This directive can use an intergrated microphone input to hear user
        return ["micro"] 
        
        

```
