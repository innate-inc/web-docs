# Changing the Robot's Voice

MARS uses [Cartesia](https://cartesia.ai/) for text-to-speech. You can customize the robot's voice by changing the Cartesia voice ID in the brain launch file.

> **Note:** This process requires recompiling the ROS2 workspace. We know it's not the easiest way yet, and it will get easier soon.

## Steps

### 1. Find the launch file

The voice ID is configured in the brain client launch file:

```text
~/innate-os/ros2_ws/src/brain/brain_client/launch/brain_client.launch.py
```

### 2. Change the voice ID

Look for the `cartesia_voice_id_arg` parameter and update the `default_value`:

```python
cartesia_voice_id_arg = DeclareLaunchArgument(
    "cartesia_voice_id",
    default_value="9fdaae0b-f885-4813-b589-3c07cf9d5fea",  # Change this ID
    description="Cartesia Alfred voice id",
)
```

Replace the voice ID with your desired Cartesia voice ID.

### 3. Rebuild the workspace

Run the following command to recompile:

```bash
cd ~/innate-os/ros2_ws && colcon build
```

Wait until the build completes.

### 4. Restart the robot

Restart MARS for the changes to take effect.

## Finding Voice IDs

You can either:

- **Use an existing Cartesia voice** — Browse their voice library to find a voice ID that suits your needs
- **Create a custom voice** — Cartesia allows you to clone a voice with just 10 seconds of audio recording

Check out the [Cartesia documentation](https://docs.cartesia.ai/get-started/overview) to learn more about available voices and how to create your own.
