# Sensors

## Overview

Maurice features a comprehensive sensor suite consisting of three primary sensors:

#### **Forward-Facing RGBD Camera**

* High-quality depth perception with 7.5cm stereo baseline
* 150° diagonal field of view
* Effective depth range: 40cm - 6m
* Depth accuracy: <2% error up to 3.5m, <4% to 6.5m, <6% to 9m

#### **Gripper-Mounted RGB Camera**

* 160° diagonal field of view
* 2MP resolution (1920x1080)
* 30 FPS at full resolution
* Ideal for close-range manipulation tasks and visual servoing

#### **2D LiDAR**

* 360° scanning coverage
* Range: 0.15m - 12m
* Angular resolution: ≤1°
* Distance resolution: <0.5mm
* Scan rate: 5.5Hz
* Primary sensor for SLAM and navigation

This sensor configuration enables robust environmental perception, precise manipulation, and reliable navigation through complementary sensing modalities. Each sensor's data can be accessed and processed through the Innate SDK.

***

## CLI Access

**Stream Sensor Data**

View live sensor data streams in a visualization window:

```bash
# Stream RGBD camera
innate sensor play rgbd
# Shows color and depth streams in separate windows# Stream gripper camera
innate sensor play gripper
# Shows color stream from gripper camera# Stream LiDAR data
innate sensor play lidar
# Shows 2D scan visualization
```

**Capture Sensor Data**

Save sensor data snapshots to file:

```bash
# Capture RGBD data
innate sensor capture rgbd
# Saves color image as {timestamp}_rgb.png and depth as {timestamp}_depth.png# Capture gripper camera image
innate sensor capture gripper
# Saves image as {timestamp}_gripper.png# Capture LiDAR scan
innate sensor capture lidar
# Saves scan data as {timestamp}_scan.txt
```

By default, captures are saved in the current working directory. Use the optional `--output` flag to specify a different save location:

```bash
innate sensor capture rgbd --output /path/to/directory
```

***

***

## Python SDK

**Setup**

```python
from innate import sensors
sensors.init()
```

**RGBD Camera**

```python
# Get current RGBD data
rgb_image, depth_image = sensors.get_rgbd()
# Returns:#   rgb_image: PIL.Image - RGB image (1280x800)#   depth_image: PIL.Image - 16-bit depth image (1280x800)
```

**Gripper Camera**

```python
# Get current gripper camera image
gripper_image = sensors.get_gripper()
# Returns: PIL.Image - RGB image (1920x1080)
```

**LiDAR**

```python
# Get current LiDAR scan
scan = sensors.get_lidar()
# Returns: numpy.ndarray - Array of distances in meters# Length: 360 elements (one per degree)
```

***

## Custom Sensors

Maurice provides two powered USB 3.0 ports for connecting additional sensors and peripherals.

**Port Specifications**

* 2x USB 3.0 ports
* Power output: 1.2 Amps per port
* Data transfer rate: Up to 5 Gbps
* Hot-swappable
* Full compatibility with USB 2.0 devices
