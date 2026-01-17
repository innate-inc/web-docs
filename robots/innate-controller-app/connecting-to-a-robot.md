# Connecting to a robot

The Innate App connects to your robot via WiFi. It can also work with your phone hotspot.

## First-Time Connection

If this is the first time you are connecting, the app will guide you through an onboarding flow:

1. **Bluetooth Pairing** — The app uses Bluetooth Low Energy (BLE) to discover your robot and configure its network settings
2. **WiFi Configuration** — Tell the robot which network to connect to
3. **Connection** — Once on the same network, the app connects to the robot via WiFi

The robot publishes its hostname with a `.local` suffix (e.g., `mars-robot.local`), making it easy to connect even if the IP address changes.



{% columns fullWidth="true" %}
{% column width="33.33333333333333%" %}
<figure><img src="../../.gitbook/assets/Screenshot_20251006-180117.png" alt="" width="375"><figcaption><p>Press on the robot when it appears to connect via bluetooth</p></figcaption></figure>


{% endcolumn %}

{% column width="33.33333333333333%" %}
<figure><img src="../../.gitbook/assets/Screenshot_20251006-180143.png" alt=""><figcaption><p>If it's already on the same network than your phone, you can connect</p></figcaption></figure>
{% endcolumn %}

{% column width="33.33333333333333%" %}
<figure><img src="../../.gitbook/assets/Screenshot_20251006-180157.png" alt="" width="375"><figcaption><p>Otherwise, select "Change Robot WiFi" to pick another network to connect it to</p></figcaption></figure>
{% endcolumn %}
{% endcolumns %}



Once connected, you can also get your current IP on the network in the Configuration Tab -> WiFi
