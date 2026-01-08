# Connecting via SSH

This page explains how to connect to your MARS robot via SSH.

The default credentials are:

- **Username:** `jetson1`
- **Password:** `goodbot`

---

## Via WiFi

If your robot is connected to the same network as your computer, you can SSH into it using its hostname:

```bash
ssh jetson1@<robot-name>.local
```

### Finding your robot's hostname

The hostname is based on **your robot's name** (the one you set in the app). The name is automatically converted to a valid hostname:

- Letters become lowercase
- Spaces and special characters become hyphens (`-`)
- Multiple hyphens are collapsed into one
- Leading/trailing hyphens are removed

**Examples:**

| Robot Name | Hostname | SSH Command |
|------------|----------|-------------|
| `My Robot` | `my-robot.local` | `ssh jetson1@my-robot.local` |
| `MARS_01` | `mars-01.local` | `ssh jetson1@mars-01.local` |
| `Test Bot` | `test-bot.local` | `ssh jetson1@test-bot.local` |

If you haven't named your robot, the default hostname is `mars.local`:

```bash
ssh jetson1@mars.local
```

---

## Via USB-C (Jetson)

If WiFi is unavailable or unreliable, you can connect directly to the Jetson's USB-C port.

1. Connect a USB-C cable between your computer and the Jetson's USB-C port on MARS.
2. The Jetson exposes a network interface over USB-C at a fixed IP address.
3. SSH into the robot:

```bash
ssh jetson1@192.168.55.1
```

This method is useful when:

- The robot is not yet configured for WiFi.
- You're debugging network issues.
- You need a direct, reliable connection.
