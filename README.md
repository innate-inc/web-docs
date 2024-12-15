---
icon: hand-wave
---

# Welcome

The Innate SDK introduces a new paradigm in which **every physical robot is an AI agen**t.

In this framework, every **function call** is either a learned physical action, a digital operation, or a combination of both.

Build complex sophisticated AI applications in minutes using state-of-the-art manipulation, navigation and reasoning capabilities.

### Example: Create a Serving Robot

Let's create a simple agent that can navigate rooms and serve glasses - a perfect introduction to physical AI development.

### 1. Installation

_You  need to have a robot implementing the Innate SDK. Innate is selling our first such robots for $3,000 a piece,_ [_see our website_](https://innate.bot)_._

```bash
pip install innate-sdk
```

### 2. Basic Setup

You can start a project in a new directory in the robot (after logging in via SSH)

```python
from innate_sdk import Agent, Primitive, Directive
from innate_sdk import manipulation, navigation
from typing import List

# Initialize your robot as an agent
robot = Agent()
```

### 3. Create Glass Handling Primitives

A primitive is akin to a function call for an LLM. This is what the robot will call when it believes it is in the right circumstances to call.

```python
class GrabGlass(Primitive):
    def __init__(self):
        super().__init__()
        manipulation.init()
    
    def execute(self):
        # Execute grab glass policy
        manipulation.run_policy("pickup_glass")
        
        return f"Retrieved glass.", True

class ServeGlass(Primitive):
    def __init__(self):
        super().__init__()
        manipulation.init()
    
    def execute(self):
        # Gentle handover motion for glass
        manipulation.run_policy("handover_glass")
        
        return f"Served glass.", True
```

### 3b. (if required) Train the pickup and handover policies

If the policies "handover\_glass" and "pickup\_glass" were not trained, you have to collect data and send it to us to train and load the policy onto your robot.

```bash
# Train policies (if not already available)
# 1. Collect demonstrations:
innate-sdk manipulation train  # Choose 'l' for learned policy, name it "pickup_glass" or "handover_glass"
# 2. Upload data and wait for training:
innate-sdk data upload pickup_glass
innate-sdk policy download pickup_glass  # Download once training is complete
```

Below is how the process looks like once you're in training mode. The SDK will guide you to collect episodes for the task you want to teach. You can learn more about training and inference in [Manipulation](basics/manipulation.md).

<figure><img src=".gitbook/assets/Teaching Maurice (1).gif" alt="" width="200"><figcaption><p>In training mode, you collect data</p></figcaption></figure>

### 4. Create a Directive

This is what describes the purpose of the robot during its execution. You can switch between directives. Here, the directive makes sure the robot is aware of its physical capabilities to act in the real world.

```python
class ServingDirective(Directive):
    def __init__():
        self.primitives = [GrabGlass(), ServeGlass()]
    
    def get_primitives(self) -> List[Primitive]:
        return self.primitives
    
    def get_prompt(self) -> str:
        return """You are a helpful robot called Maurice that serves drinks to people.
Navigate between rooms to find people. When you find someone, ask if they want a drink,
and if so, navigate to the right place to get one, pick it up and go back to give it.

You have the personality of a bartender.
"""

# Register and activate the directive
robot.set_directive(ServingDirective())
```

### 5. Run Your Agent

```python
# Start the robot with the serving directive
robot.run()

# Your robot will now be serving glasses,
# navigating between rooms and interacting with people
```

This is what the resulting execution looks like:

{% embed url="https://www.youtube.com/watch?v=EraAyDKoZpI" %}
Executing the ServingDirective
{% endembed %}



### Core Concepts

* **Primitives**: Building blocks that combine physical actions, learned behaviors, and digital operations
* **Directives**: Natural language instructions that guide how your robot uses primitives
* **Policies**: Learned behaviors for complex physical tasks like manipulation
* **Navigation**: Built-in mapping and movement capabilities

### Full Platform Features

🤖 Full robotic control (navigation, manipulation, sensing)

🧠 Built-in AI agent capabilities

📱 Simple Python SDK and CLI tools

🛠 Extensible hardware support

🎓 Learning from demonstration

👀 Advanced visual understanding



Ready to build something more complex? Check out our detailed [examples](basics/user-primitives.md) and join our developer community below.

## Getting Started

1. Follow our Setup Guide to get Maurice up and running

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

2. Learn about basic Navigation and Manipulation Control

{% content-ref url="basics/navigation.md" %}
[navigation.md](basics/navigation.md)
{% endcontent-ref %}

{% content-ref url="basics/manipulation.md" %}
[manipulation.md](basics/manipulation.md)
{% endcontent-ref %}



3. Explore creating User Primitives

{% content-ref url="basics/user-primitives.md" %}
[user-primitives.md](basics/user-primitives.md)
{% endcontent-ref %}

4. Join our community to share and learn from other builders

[![Join our Discord server](https://i0.wp.com/nikke.gg/wp-content/uploads/join-us-discord.png?fit=728%2C200\&ssl=1)](https://discord.com/invite/PNtn52yZ)



**Maurice is built for hackers who want to push the boundaries of what's possible with embodied AI. Whether you're building a robotic bartender or something entirely new, we can't wait to see what you'll create.**
