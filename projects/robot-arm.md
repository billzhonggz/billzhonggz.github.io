---
layout: default
---

# Motion Capture Robot Arm

<video style="width: 100%;" src="/assets/video/Leap-Motion-Robot-Arm.mp4" controls preload="none"></video>

The **Motion Capture Robot Arm** is a 3D printed robot arm, controlled by [Leap Motion](https://www.leapmotion.com/). The basic logic is, Leap Motion captures the motion of operator's hand, then covert fingers' vectors to angles and output to [Arduino](https://www.arduino.cc/), finally the Arduino gives control signals to servos and servos power the fingers on the robot arm.

## Hardware
There are two parts of hardware in this project. Leap Motion and the robot arm. The hardware part was mostly done by [Garfield Wu](https://github.com/wjqzero).

### Leap Motion
![Leap Monitor](/assets/img/robot-arm/leap-monitor.jpg)

Leap Motion is a hardware with two infrared cameras launched in 2013. It can capture motions on two human hands and it has a well-encapsulated interfaces help developers find the "hands" quickly.

### Robot Arm
![3D Printed Parts](/assets/img/robot-arm/3d-printed-part.png) 

The robot arm was 3D printed and the model we used can be found [here](https://hackaday.io/project/102439-robot-hand-bionic-hand-prosthesis-prototype) (a big thank to the author :)).

![failed print](/assets/img/robot-arm/print-failed.jpg)

But we quickly found out the product we made was hard to use. Due to printers' precision, most of the parts we printed cannot be used immediately and the printing process always fails. Before we could assemble the arm, we had to sand the surfaces to ensure all parts can move smoothly (kind of :joy:).

The fingers are powered by servos. Servos are installed inside the stand. And each finger is connected with a servo by fishing wire. By sending angles to Arduino, the included standard servo library can convert the angles to PWM singles, and the servos can turn to the angles pointed.

The parameters of converting finger angles to servo angles set differently for different servos. We spent some time on tuning these parameters.

## Software

In software side, I looked at the code from acali (check out [here](http://www.instructables.com/id/Robotic-Hand-controlled-by-Gesture-with-Arduino-Le/)). He provided us with an easy way to finish this project. 

We follow the acali's idea, applied a tech stack base on Node.js because I have done a Node.js project SE Retail Management System. The project applies the following frameworks to achieve its logic.

* Leap Motion's LeapJS API [GitHub](https://github.com/leapmotion/leapjs)
* Johnny-Five JavaScript Robotics & IoT Platform [Website](http://johnny-five.io/)
* Firmata Arduino Library (included in Arduino IDE examples) [GitHub](https://github.com/firmata/arduino)

Since I have the most experiences in software engineering, I was very happy to see this stack with libraries :).

Both LeapJS and Jhonny-Five works on my computer, LeapJS gets finger vectors, then we calculate them to finger and servo angles, then send angles by serial ports with Johnny-Five library.

## Features

At this moment, the robot arm can follow exactly what the human arm does. Because the all components are decoupled, we can easily switch any of them to achieve more functions. It can be entertaining.

Here are some components can be switched or expanded.

* Servo controller: from Arduino to any other controller with PWM outputs.
* Transmission method of controlling signals: from USB serial port to WiFi, Bluetooth, or cellular.
* Motion capture: from Leap Motion to [Kinect](https://developer.microsoft.com/en-us/windows/kinect), [Intel RealSense](https://software.intel.com/realsense), or double cameras (multi-view) with [OpenCV](https://opencv.org/).
* Actions toward motions: from simple imitation to reaction to some condition. Like [Morra game](https://en.wikipedia.org/wiki/Morra_(game)).