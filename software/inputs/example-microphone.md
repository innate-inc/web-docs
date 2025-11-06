# Example: Microphone

Innate Robots are designed with extensibility in mind.

MARS, in particular, can easily be augmented with a $50 microphone to be able to listen to you (see tested components in [extending-mars.md](../../robots/mars/extending-mars.md "mention"))



By default, a "micro" input device is available on MARS. If you have a microphone plugged in and are triggering a behavior using it, you will be able to talk directly to MARS. See below the relevant part of the code in the hello world behavior (same than in [definitions.md](../behaviors/definitions.md "mention")).

```python
def get_inputs(self) -> List[str]:
        # This directive can use an intergrated microphone input to hear user
        return ["micro"] 

```

The code for this input device is available in our open-source repositories at TBD.



#### How it looks like

{% embed url="https://youtu.be/Da_vpacFfvM" %}
