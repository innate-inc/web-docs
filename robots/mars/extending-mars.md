# Extending MARS

MARS was designed for hardware extensibility through several features:

* Most GPIO pins are available on the onboard computer.
* Two USB-A extension ports are available at the top of the robot.
* The hardware is [open-sourced](https://innate.bot/tbd) in order to be easier to modify, in particular the end-effector.



We describe below how we recommend to extend MARS on the hardware side.



## <i class="fa-hand">:hand:</i> Changing the end-effector

The end effector by default is a gripper (opposable thumbs).

By nature of robot learning, if you change the end effector to any other shape, you will have to recollect your data for your manipulation models, but nothing else in the operating system needs to change if you still use the last actuator.

Examples of two different types of end effectors we used without changing the code:

{% columns %}
{% column %}
<figure><img src="../../.gitbook/assets/1319_recol.png" alt=""><figcaption><p>MARS v1 gripper</p></figcaption></figure>


{% endcolumn %}

{% column %}
<figure><img src="../../.gitbook/assets/1320_recol.png" alt=""><figcaption><p>A gripper with just one side moving</p></figcaption></figure>


{% endcolumn %}
{% endcolumns %}



## <i class="fa-sensor">:sensor:</i> Adding sensors and effectors on the USB and GPIOs

We leave it to the user to integrate any additional sensors they want on the ports available. For any added device, users should make sure they install it properly on the robot. You can then feed the data in the BASIC OS by creating a Sensor object in the SDK (to be revealed...)

Some sensors we have already tried and work plug-and-play on MARS:

* <i class="fa-microphone-lines">:microphone-lines:</i> TONOR directional microphone ([$29 on Amazon](https://www.amazon.com/dp/B0CSCT63BL?ref=fed_asin_title)) - Plug it in and use the TONORMicrophoneInput (to be revealed...) to make MARS able to listen to you in real time.
* <i class="fa-smog">:smog:</i>  Blackiot [Polverine Air Quality Sensor](https://blackiot.swiss/polverine).



#### Example: Adding a microphone

By simply plugging a microphone in the robot and following the [example-microphone.md](../../software/inputs/example-microphone.md "mention") guide, you can now talk directly with your MARS, including when it's moving around and doing tasks.

{% embed url="https://youtu.be/Da_vpacFfvM" %}
