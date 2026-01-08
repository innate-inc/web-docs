# Connecting via SSH

This page explains how to connect to your MARS robot via SSH.

---

## Via WiFi

If your robot is connected to the same network as your computer, you can SSH into it using its hostname or IP address:

```bash
ssh innate@<robot-hostname-or-ip>
```

---

## Via USB-C (Jetson)

If WiFi is unavailable or unreliable, you can connect directly to the Jetson's USB-C port.

1. Connect a USB-C cable between your computer and the Jetson's USB-C port on MARS.
2. The Jetson exposes a network interface over USB-C at a fixed IP address.
3. SSH into the robot:

```bash
ssh innate@192.168.55.1
```

This method is useful when:

- The robot is not yet configured for WiFi.
- You're debugging network issues.
- You need a direct, reliable connection.
