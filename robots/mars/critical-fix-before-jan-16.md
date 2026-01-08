# Critical Fix for Robots Received Before January 16

{% hint style="warning" %}
**This guide is only for robots received before January 16, 2025.** If you received your robot after this date, this fix has already been applied and you can skip this page.
{% endhint %}

Robots shipped before January 16 require a critical update before normal operation. This update must be applied via a Wi-Fi hotspot connection before using the standard app-based setup.

---

## Overview

The standard setup flow requires your robot to connect to your home Wi-Fi through the app. However, robots received before January 16 have a bug that prevents this from working correctly until a fix is applied.

To work around this, you will:

1. Create a Wi-Fi hotspot on your phone with a specific name and password
2. The robot will automatically connect to this hotspot
3. Connect to the robot via Bluetooth in the app to get its IP address
4. SSH into the robot from your computer and apply the update

---

## Step 1: Create a Wi-Fi hotspot

On your phone, create a Wi-Fi hotspot with the following credentials:

- **Network name (SSID):** `FBI_VAN_6`
- **Password:** `12345678`

{% tabs %}
{% tab title="iPhone" %}

1. Go to **Settings** → **Personal Hotspot**
2. Tap **Wi-Fi Password** and set it to `12345678`
3. For the network name, go to **Settings** → **General** → **About** → **Name** and set it to `FBI_VAN_6`
4. Toggle **Allow Others to Join** to ON
{% endtab %}

{% tab title="Android" %}

1. Go to **Settings** → **Connections** → **Mobile Hotspot and Tethering**
2. Tap **Mobile Hotspot**
3. Set **Network name** to `FBI_VAN_6`
4. Set **Password** to `12345678`
5. Toggle the hotspot ON
{% endtab %}
{% endtabs %}

---

## Step 2: Turn on your robot

Power on your MARS robot by plugging in the battery. Wait for the LiDAR to start spinning and for the startup sound.

The robot will automatically detect and connect to the `FBI_VAN_6` hotspot.

---

## Step 3: Get the robot's IP address via Bluetooth

1. Open the Innate app on your phone
2. On the Connection screen, you should see your robot available in the list under "Bluetooth" connections (or refresh until you do).
3. Top on it and just stay on the next screen.
4. Note the **IP address** shown for your robot (it could be something like `172.20.10.x` but depends on your phone)

{% hint style="info" %}
You should not fully connect to the robot via wifi once you reach that screen. Just stay there where it gives you wifi information and proceed to the update below.
{% endhint %}

---

## Step 4: Connect your computer to the hotspot

Connect your computer to the same `FBI_VAN_6` Wi-Fi hotspot that you created. This puts your computer on the same network as the robot.

---

## Step 5: SSH into the robot and apply the update

Open a terminal on your computer and SSH into the robot using the IP address you noted:

```bash
ssh jetson1@<robot-ip-address>
```

For example:

```bash
ssh jetson1@172.20.10.5
```

The default credentials are:

- **Username:** `jetson1`
- **Password:** `goodbot`

Once connected, run the update:

```bash
innate-update apply
```

Wait for the update to complete. You will see progress messages and a final confirmation that the update was successful.

---

## Step 6: Proceed with normal setup

Once the update is complete:

1. Turn off your phone's hotspot
2. Restart your robot (unplug and replug the battery)
3. Get back to the standard [onboarding process](https://innate.bot/onboarding) to connect your robot to your home Wi-Fi through the app.

Your robot is now ready for normal operation!

---

## Troubleshooting

If you encounter issues:

- **Robot doesn't connect to hotspot:** Make sure the hotspot name is exactly `FBI_VAN_6` (lowercase) and the password is exactly `12345678`
- **Can't see IP in app:** Wait a minute for the robot to fully boot and connect, then refresh the Bluetooth screen
- **SSH connection refused:** Ensure your computer is connected to the same hotspot and the IP address is correct
- **Update fails:** Check that the robot has internet access through the hotspot (your phone needs mobile data enabled)

If problems persist, reach out on [Discord](https://discord.com/invite/KtkyT97kc7) or email [axel@innate.bot](mailto:axel@innate.bot).
