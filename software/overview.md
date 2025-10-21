---
description: How to create Behavior Apps for Innate Robots using the SDK - or ROS2 straight
---

# Overview

## Core concepts

<figure><img src="../.gitbook/assets/Frame 1597880983.png" alt=""><figcaption><p>A system diagram of the software running on Innate robots</p></figcaption></figure>

#### An agentic OS

Innate robots run an agentic OS built on top of ROS2. It is powered by our cloud agent called BASIC.

This abstraction layer allows to create powerful agentic applications quickly without having to care about the usual suspects of classic robotics (unless you want to).



#### Behaviors

The central concept of the Innate OS is the **`behavior`** , which is our name for a **physical app for robots.** They are defined by a **system prompt** and a set of **skills** they can use.

{% hint style="info" %}
**Behaviors** are like **physical apps** for Innate robots.
{% endhint %}



The most simple behavior is:

```python
class HelloWorld(Behavior):
    def name(self) -> str:
        return "hello_world"

    def get_skills(self) -> List[str]:
        return [""]

    def get_prompt(self) -> str:
        return """You are just a robot saying hello_world once you start."""
```



This will start the robot and make it say hello world on the speakers once.

See more in [behaviors](behaviors/ "mention")



#### Skills

**Skills** are the second core concept of the Innate OS.

A skill can be defined with code, a model checkpoint (such as a **VLA**) or other specific interfaces we define for you. Adding a skill to a behavior is like giving additional capabilities to your robot.

{% hint style="info" %}
Similarly to agentic frameworks, **skills** can be thought as **tool calls**, with extra sauce.
{% endhint %}



Skills can be interrupted by the robot during execution if required, and send feedback in the context of to the running behavior.



See how to create skills in [skills](skills/ "mention")



#### BASIC

BASIC is the embodied AI agent that controls Mars. BASIC can run behaviors and skills, and gives Mars the ability to reason, memorize, plan and make decisions as it runs.



Understand more how BASIC runs in [basic](basic/ "mention")



#### ROS2 core

Our OS runs at the core on **ROS2** and can be augmented at that level by roboticists that understand it.

See [ros2-core.md](ros2-core.md "mention") for more information on nodes, topics, and services available.

