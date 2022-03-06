## Roboclaw Arduino ROS Driver
This repository is created to run the roboclaw motor controller with Arduino Mega and Nvidia Jetson Nano over ROS serial communication.

## Modification
The original repository was created to use 2 unit of roboclaw for 4 Wheel Drive (Roboclaw0-->Left side, Roboclaw1-->Right side). This repository is created to use 1 unit of roboclaw (channel1-->Left side, channel2-->Right side) along with arduino mega for 2 Wheel Drive.

## Download and Install
This work is tested with Nvidia Jetson Nano Jetpack L4T5.1, arduino 1.8.15, [roserial Arduino Library 0.7.9](https://www.arduino.cc/reference/en/libraries/rosserial-arduino-library/) and [roboclaw arduino library](https://github.com/basicmicro/roboclaw_arduino_library).  First, download and install arduino on jetson nano.  
```
git clone https://github.com/JetsonHacksNano/installArduinoIDE.git
cd installArduinoIDE
./installArduinoIDE.sh
```
The first time you will open arduino ide, it will show up some library and board update. Do that in order to save time. In order to run this repo, we need some specific arduino library. Download [roserial Arduino Library 0.7.9](https://www.arduino.cc/reference/en/libraries/rosserial-arduino-library/) and [roboclaw arduino library](https://github.com/basicmicro/roboclaw_arduino_library), extract them if needed and put those 2 folders inside `~/Arduino/libraries/` directory. Now, clone this repository to compile and upload the code to your arduino mega board.
```
git clone https://github.com/ArghyaChatterjee/arduino_teleop.git
cd arduino_teleop/teleop
arduino teleop.ino
```
Compile the code via verify button to see the errors on arduino terminal if any. If not, upload the code directly to the arduino mega board via upload button. Remember to select `ttyACM0` port during upload. Once uploaded, you can use rosserial node inside your roslaunch file this way to communicate with roboclaw via arduino mega:
```
<node name="serial_node" pkg="rosserial_python" type="serial_node.py">
		<param name="port" value="/dev/ttyACM0"/>
		<param name="baud" value="38400"/>
</node>
```

## Synopsis

The `teleop` sketch interfaces with two Roboclaw controllers and a `rosserial` bridge over USB.  It subscribes to the cmd_vel topic which contains the robot's target twist and then converts these msgs to left/right wheel spin speeds.  The roboclaw's PID controllers obtain these desired speeds with the input of encoders.

`tune_pid` is used to tune (using the arduino serial moniter) the pids of the roboclaws.  To tune PID, the phidgets board will need to actually be collecting encoder data(even though you don't actually use this data) otherwise the encoders will not be powered and the roboclaws will not get an encoder signal.

## Installation

Uses an arduino mega.  Need to have ros and Roboclaw arduino libarys to flash to the arduino. Serial1 is used for communication with RoboClaw controllers.  Roboclaw controllers must be wired in single mode as per the roboclaw user manual.  roboclaw controls the left/right side wheels of the robot respectively.  
