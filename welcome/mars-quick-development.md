---
description: >-
  Learn to create your first behavior, train your first manipulation model, give
  MARS the ability to read emails, and put the pieces together
icon: screwdriver-wrench
---

# MARS Quick Development

## Create your first behavior

Now that you know the basics, you can create your first behavior for Mars using the SDK. On the app, go to Settings -> Wifi and read the IP of the robot.



Now, on your PC, ssh in the robot with&#x20;

```bash
ssh jetson1@<YOUR-ROBOT-IP>
```





Go to `~/behaviors/` and create a `hello_world.py` behavior file:

```python
from typing import List
from brain_client.behaviors.types import Behavior

class HelloWorld(Behavior):
    @property
    def name(self) -> str:
        return "hello_world"

    def get_skills(self) -> List[str]:
        return [
            "navigate_to_position",
            "wave",
        ]

    def get_prompt(self) -> str:
        return 
"""
You are a friendly greeting robot whose sole purpose is to say hello world to the user!

Your personality:
- You are a nice and cheerful robot.

Instructions:
- When you see a user in front of you, say "hello world" and wave at the user.
- Don't navigate, just turn around if you don't see the user.
"""

```



"_wave_" and "_navigate\_to\_position_" are basic skills that come already created for the robot. This behavior makes use of them to act autonomously.



Save the file, then restart the robot (unplug and plug again), open the app and start your behavior. Sit in front of the robot, and observe!



**VIDEO**



To dive more in the details of how to develop behaviors:

{% content-ref url="../software/behaviors.md" %}
[behaviors.md](../software/behaviors.md)
{% endcontent-ref %}



## Train your first manipulation model for a skill

Innate Robots arms can be trained using state-of-the-art manipulation AI models, running straight on the onboard computer. For MARS, we developed an improved version of ACT with a reward model - see [more details here](https://innate.bot/tbd)



To train it, you can use the app to collect episodes of data for imitation learning. I.E. you will be repeatedly performing the task with the robot for a given amount of repetitions to make sure it learns it the way you want.



In the app, go to Skills -> Physical, create a new skill, name it, and press "Add Episodes".



Then, arm the arm and press record to collect an episode. Ideally, all episodes should start in a similar position and end in a similar position, following roughly the same movement. Start with very similar trajectories to accomplish the goal while making sure that the arm camera has the objective of motion relatively in sight. More guidelines on training can be found [here](https://innate.bot/tbd).



Below, an example of training the arm to pick up a cherry.



{% embed url="https://youtu.be/dr1TuHpc_94" %}



Once you collected around 50 episodes, you can start considering stopping data collection. We can easily train your dataset for you if you go to the Training tab and press Train with whole dataset. You can also use the episodes for yourself by ssh-ing in the robot and getting them there.



Once the model is trained (which takes up to 4 hours), you can get it back on your robot and then trigger it from Manual Control Screen!



**VIDEO**



## Create your first digital skill

Innate robots can also run any kind of code in the embodied agent BASIC, which can be used to query APIs online or run custom routines onboard.

Below is an example of how to create a skill that queries gmail to read the last emails.

```python
import imaplib
import email
from brain_client.skill_types import Skill, SkillResult


class RetrieveEmails(Primitive):
    def __init__(self, logger):
        self.logger = logger
        self.imap_server = "imap.gmail.com"
        self.email = "your_email@gmail.com"

    @property
    def name(self):
        return "retrieve_emails"

    def guidelines(self):
        return "Use to retrieve recent emails. Provide count (default 5). Returns subjects and content."

    def execute(self, count: int = 5):
        count = min(max(1, count), 20)
        try:
            mail = imaplib.IMAP4_SSL(self.imap_server, 993)
            mail.login(self.email, self.password)
            # ... fetch and process emails ...
            email_data = "Email 1: Subject, From, Content..."
            self._send_feedback(email_data)
            return f"Retrieved {count} emails with subjects and content", PrimitiveResult.SUCCESS
        except Exception as e:
            return f"Failed to retrieve emails: {str(e)}", PrimitiveResult.FAILURE

```



You can run this skill in a behavior that can query it:



```python
from typing import List
from brain_client.behaviors.types import Behavior

class EmailAssistant(Behavior):
    @property
    def name(self) -> str:
        return "email_assistant"

    def get_skills(self) -> List[str]:
        return [
            "retrieve_emails"
        ]

    def get_prompt(self) -> str:
        return 
"""
You are an email assistant. 
When the user sits in front of you, you should tell them what their last email is
"""

```



Below is the result of running it:

**VIDEO**



To learn more about the Skills SDK:

{% content-ref url="../software/skills/" %}
[skills](../software/skills/)
{% endcontent-ref %}

