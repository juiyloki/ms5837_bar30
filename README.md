# ms5837_bar_ros

[![GitHub stars](https://img.shields.io/github/stars/tasada038/ms5837_bar_ros.svg?style=social&label=Star&maxAge=2592000)](https://github.com/tasada038/ms5837_bar_ros/stargazers/)
[![GitHub forks](https://img.shields.io/github/forks/tasada038/ms5837_bar_ros.svg?style=social&label=Fork&maxAge=2592000)](https://github.com/tasada038/ms5837_bar_ros/network/)
[![GitHub issues](https://img.shields.io/github/issues/tasada038/ms5837_bar_ros.svg)](https://github.com/tasada038/ms5837_bar_ros/issues/)

## Overview

ROS 2 package for Blue Robotics Bar30 and Bar02 High Resolution Depth/Pressure Sensor

**Keywords:** ROS 2, Bar30, Bar02

### License

The source code is released under a [MIT license](LICENSE).

## Requirements
[Bar30 High-Resolution 300m Depth/Pressure Sensor](https://bluerobotics.com/store/sensors-sonars-cameras/sensors/bar30-sensor-r1/)

[Bar02 Ultra High Resolution 10m Depth/Pressure Sensor](https://bluerobotics.com/store/sensors-sonars-cameras/sensors/bar02-sensor-r1-rp/)

## Installation

Clone with `--recursive` in order to get the necessary `ms5837-python` library:

> **Warning**\
> The python library will generate a syntax error if there is a hyphen in the name.\
> Therefore, we need to move ms5837 package in the submodule.

```
sudo apt-get install i2c-tools
sudo pip3 install smbus2
```

I2C Address: default is `0x76`, so check the i2c address.

```sh: Terminal
i2cdetect -y 1
Warning: Can't use SMBus Quick Write command, will skip some addresses
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f
00:
10:
20:
30: -- -- -- -- -- -- -- --
40:
50: -- -- -- -- -- -- -- -- -- -- -- -- -- -- -- --
60:
70:

# If it is not detected by i2cdtect, run the following:
# If you use more than two, you will need to use another device as dump detection is not possible with 0x76.

i2cdump -y 1 0x76
No size specified (using byte-data access)
     0  1  2  3  4  5  6  7  8  9  a  b  c  d  e  f    0123456789abcdef
00: 7b 00 XX XX XX XX XX XX XX XX XX XX XX XX XX XX    {.XXXXXXXXXXXXXX
10: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
20: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
30: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
40: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
50: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
60: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
70: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
80: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
90: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
a0: 00 00 b2 b2 b1 b1 6b 6b 71 71 78 78 67 67 XX XX    ..????kkqqxxggXX
b0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
c0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
d0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
e0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
f0: XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX XX    XXXXXXXXXXXXXXXX
```

Build and run execute the following:

```sh
cd dev_ws/src
git clone -b master --recursive https://github.com/tasada038/ms5837_bar_ros.git
cd ~/dev_ws/src/ms5837_bar_ros/ms5837_bar_ros/
mv ms5837-python/ms5837 ./
sudo rm -r ms5837-python/*.py
cd ~/dev_ws
colcon build --packages-select ms5837_bar_ros
```

## Run
Publish bar data
```
. install/setup.bash
ros2 run ms5837_bar_ros bar30_node
ros2 run ms5837_bar_ros bar02_node
```

Publish bar data using Rviz2
```
. install/setup.bash
ros2 launch ms5837_bar_ros bar30.launch.py
ros2 launch ms5837_bar_ros bar02.launch.py
```

![ms5837_barxx_img](img/ms5837_barxx.png)

## Ping sonar Topics
The topics of the ms5837_bar_ros are as follows.

> **Note**\
> xx is 30 or 02.


```
$ ros2 topic list
/barxx/pressure
/barxx/temperature
/barxx/depth
/barxx/odom
```

- std_msgs.msg Float32: /barxx/pressure
- std_msgs.msg Float32: /barxx/temperature
- std_msgs.msg Float32: /barxx/depth
- nav_msgs.msg Odometry: /barxx/odom

## License
This action is licensed under the MIT License. This project is originally created by [Blue Robotics](https://github.com/bluerobotics), and maintained continuously by Takumi Asada.

Projects in .gitmodules files are covered by Blue Robotics Inc's MIT License.
Other software components are licensed under this project's license.
