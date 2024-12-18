---
icon: golang
---

# Get started

The Innate SDK introduces a new paradigm in which **every physical robot is an AI agen**t.

In this framework, every **function call** is either a learned physical action, a digital operation, or a combination of both.

Build complex sophisticated AI applications in minutes using state-of-the-art manipulation, navigation and reasoning capabilities.

### Example: Create a Service Robot

Let's create a simple agent that can navigate rooms and serve glasses - a perfect introduction to physical AI development.

### 1. Installation

_You  need to have a robot implementing the Innate SDK. Innate is selling our first such robots for $2,000 a piece,_ [_see our website_](https://innate.bot)_._

```bash
pip install innate-sdk 
```

Then follow the instructions in [maurice-setup.md](../setup/maurice-setup.md "mention") and [workstation-setup.md](../setup/workstation-setup.md "mention").

You can start coding right after by defining files in `~/primitives` and `~/directives`

### 2. Create Glass Handling Primitives

A primitive is akin to a function call for an LLM. This is what the robot will call when it believes it is in the right circumstances to call. You can add guidelines to tell it more about when to call it, when to interrupt it...

```python
# Create a primitive for grabbing cardboard glasses
# In ~/primitives/grab_glass.py
from innate import Primitive, manipulation
from typing import List

class GrabGlass(Primitive):
    def init(self):
        super().init()
        manipulation.init()

    def guidelines(self):
        return """Can be called in front of a cardboard glass, to let the arm grab it. 
        If the glass is not cardboard, the primitive should not be called.
        The glass has to be handed by a human, it cannot be picked up from the ground."""

    async def execute(self):
        await manipulation.run_policy("pickup_glass")
        return "Retrieved glass.", True

# In ~/primitives/serve_glass.py
class ServeGlass(Primitive):
    def __init__(self):
        super().__init__()
        manipulation.init()
    
    async def execute(self):
        # Gentle handover motion for glass
        await manipulation.run_policy("handover_glass")
        
        return f"Served glass.", True
```

### 2b. (if required) Train the pickup and handover policies

If the policies "handover\_glass" and "pickup\_glass" were not trained, you have to collect data and send it to us to train and load the policy onto your robot.

```bash
# Train policies (if not already available)
# 1. Collect demonstrations:
innate manipulation train  # Choose 'l' for learned policy, name it "pickup_glass" or "handover_glass"
# 2. Upload data and wait for training:
innate data upload pickup_glass
innate policy download pickup_glass  # Download once training is complete
```

Below is how the process looks like once you're in training mode. The SDK will guide you to collect episodes for the task you want to teach. You can learn more about training and inference in [Manipulation](../basics/manipulation.md).

<figure><img src="../.gitbook/assets/Teaching Maurice (1).gif" alt="" width="200"><figcaption><p>In training mode, you collect data</p></figcaption></figure>

### 3. Create a Directive

This is what describes the purpose of the robot during its execution. You can switch between directives. Here, the directive makes sure the robot is aware of its physical capabilities to act in the real world.

```python
# In ~/directives/serving_directive
from innate import Directive
from typing import List

class ServingDirective(Directive):
    def get_primitives(self) -> List[str]:
        return ["grab_glass", "serve_glass"] # Refers to the files defined above
    
    def get_prompt(self) -> str:
        return """You are a helpful robot called Maurice that serves drinks to people.
Navigate between rooms to find people. When you find someone, ask if they want a drink,
and if so, navigate to the right place to get one, pick it up and go back to give it.

You have the personality of a bartender.
"""

# Register and activate the directive
robot.set_directive(ServingDirective())
```

### 4. Run Your Agent

First move the robot around a little with the app so that it memorizes the place.&#x20;

Then let the agent run, using either the app, or the terminal:

```bash
innate sdk directive activate serving_directive
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



Ready to build something more complex? Check out our detailed [examples](../basics/user-primitives.md) and join our developer community below.

## Getting Started

1. Follow our Setup Guide to get Maurice up and running

{% content-ref url="broken-reference" %}
[Broken link](broken-reference)
{% endcontent-ref %}

2. Learn about basic Navigation and Manipulation Control

{% content-ref url="../basics/navigation.md" %}
[navigation.md](../basics/navigation.md)
{% endcontent-ref %}

{% content-ref url="../basics/manipulation.md" %}
[manipulation.md](../basics/manipulation.md)
{% endcontent-ref %}



3. Explore creating User Primitives

{% content-ref url="../basics/user-primitives.md" %}
[user-primitives.md](../basics/user-primitives.md)
{% endcontent-ref %}

4. Join our community to share and learn from other builders



**Maurice is built for hackers who want to push the boundaries of what's possible with embodied AI. Whether you're building a robotic bartender or something entirely new, we can't wait to see what you'll create.**
