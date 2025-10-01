# Extending MARS

MARS was designed for hardware extensibility through several features:

* Most GPIO pins are available on the onboard computer.
* Two USB-A extension ports are available at the top of the robot.
* The hardware is [open-sourced](https://innate.bot/tbd) in order to be easier to modify, in particular the end-effector.



We describe below how we recommend to extend MARS on the hardware side.



## Changing the end-effector

The end effector by default is a gripper (opposable thumbs).

By nature of robot learning, if you change the end effector to any other shape, you will have to recollect your data for your manipulation models, but nothing else in the operating system needs to change if you still use the last actuator.

Examples of two different types of end effectors we used without changing the code:

{% columns %}
{% column %}
**TBD**

**New end effector of v1**
{% endcolumn %}

{% column %}
**TBD**

**Old effector of v0.9**
{% endcolumn %}
{% endcolumns %}



## Adding sensors and effectors on the USB and GPIOs

We leave it to the user to integrate any additional sensors they want on the ports available. For any added device, users should make sure they install it properly on the robot. You can then feed the data in the BASIC OS by creating a Sensor object in the SDK (to be revealed...)

Some sensors we have already tried and work plug-and-play on MARS:

* TONOR directional microphone ([$29 on Amazon](https://www.amazon.com/dp/B0CSCT63BL?ref=fed_asin_title)) - Plug it in and use the TONORMicrophoneInput (to be revealed...) to make MARS able to listen to you in real time.
* _To be continued..._

