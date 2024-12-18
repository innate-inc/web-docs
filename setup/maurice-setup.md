---
icon: wrench
---

# Maurice Setup

### Charging Maurice

To charge Maurice, follow these simple steps:

1. Locate the included wall charger that came with your device.
2. Find the charging port located on the back of Maurice.
3. Connect the charger to Maurice's charging port at the rear.
4. Plug the wall charger into a standard electrical outlet.
5. Monitor Maurice's status light while charging:
   * Red light indicates low battery
   * Yellow light indicates medium charge level
   * Green light indicates fully charged

For optimal performance, charge Maurice when the status light turns red. A complete charging cycle typically occurs when the light changes from red to green.

Note: Continue to use only the wall charger included with Maurice to ensure safe and proper charging.

***

### Power Maurice On

To power on Maurice:

1. Press the power button once to turn Maurice on.
2. The status light will indicate Maurice's current battery level:
   * Red light: low battery
   * Yellow light: medium charge
   * Green light: full charge

Wi-Fi Connection Status:

* Blinking light indicates Maurice is attempting to connect to Wi-Fi
* Solid light confirms Maurice has successfully connected to Wi-Fi

If this is your first time powering on Maurice and you haven't set up the Wi-Fi connection yet, please proceed to the Connection Setup section below.

Note: If Maurice's status light continues to blink, check your Wi-Fi connection or refer to the troubleshooting section.

***

### Connection Setup

There are two paths to establish network connectivity for Maurice: via the Innate Builder App or through a direct Wi-Fi configuration.

Initial Setup Mode: Press and hold the power button for 5 seconds until the LED indicator begins pulsing blue, indicating Maurice has entered configuration mode.

Method 1: Innate Builder App Configuration

1. Install the Innate Builder App
2. Navigate to: Robots → Setup New Robot
3. The app will scan for available Bluetooth LE devices
4. Locate and select "Maurice\_XXX" from the discovered devices
5. Once paired:
   * Select target Wi-Fi SSID
   * Input network credentials
6. Upon successful connection:
   * Status LED transitions to solid state (color reflects battery level)
   * Bluetooth connection terminates
   * App control interface becomes active

Method 2: Direct Wi-Fi Configuration

1. Maurice creates a local access point "Maurice\_XXX"
2. Connect to this network
3. Access the configuration interface at 192.168.1.1
4. Navigate to connection settings
5. Input target network credentials:
   * SSID
   * Password
6. Upon successful connection:
   * Local AP terminates
   * Status LED transitions to solid state

Network Requirements: For successful communication, both the control device and Maurice must operate on the same subnet. Maurice supports standard Wi-Fi protocols.
