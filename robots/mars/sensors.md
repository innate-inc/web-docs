# Sensors

## Overview

Mars features a comprehensive sensor suite consisting of three primary sensors:

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

## Custom Sensors

Mars provides two powered USB 3.0 ports for connecting additional sensors and peripherals.

**Port Specifications**

* 2x USB 3.0 ports
* Power output: 1.2 Amps per port
* Data transfer rate: Up to 5 Gbps
* Hot-swappable
* Full compatibility with USB 2.0 devices
