# Hawaii Robotics - Adaptive Manufacturing Robot

## Overview
This project was developed as a **4th-year capstone project** to design and implement an **adaptive manufacturing robot**.  
The goal was to build a robotic system capable of adjusting to varying inputs and conditions, enabling more flexible and efficient manufacturing processes. We successfully designed and built a fully custom robotic solution within six months.

---

## Features
- ✅ Fully custom adaptive robot control system for manufacturing tasks
- ✅ Ergonomic, cost-effective “teacher arms” for bi-manual teleoperation
- ✅ Custom PCBs for real-time encoder integration, LED control, and power management
- ✅ Trainable with 25–50 demonstrations; autonomously completes tasks with error correction and adapts to slight changes in orientation or placement
- ✅ Modular software architecture supporting future expansion

---

## System Architecture

- **Software**: Python, C++, ROS, ACT, I2C, UART, Interbotix Dynamixel Drivers
<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/b60cd47d-4422-4331-962a-47c81064d591" />
- **Hardware**: Custom PCBs / Teensy 4.1, AS5600L Hall-Effect Encoders, I2C Extender, Dynamixel servos, Logitech WebCams
  
---

## My Contributions
This was a team project. My personal contributions included:
- Designed, implemented, and tested all software and firmware.
- Integrated software with hardware components
- Developed ACT (Action Chunking with Transformer) integrations
- Assisted with mechanical and electrical assembly and testing

---

## Challenges + Lessons Learned

- Controlling the gripper was a major challenge because the servo had to run in position mode to match teleoperation commands, which often caused stalls when the following error grew too large. The servo didn’t expose current or torque directly, but instead reported a “present load” value representing the percentage of maximum torque, with errors triggered at 1000. To prevent overloads, I implemented logic that records the last safe gripper position (around 900 present load) and rejects further closing commands beyond this point, ensuring the servo operates without error.

- Another major challenge was achieving a high observation collection frequency. Each cycle required reading seven magnetic encoder sensors per arm over a shared I2C bus (via a Teensy on our custom PCB, streamed over serial) as well as image data from four Logitech webcams connected via USB. Although we expected to easily reach 60 Hz, initial tests showed only 20–30 Hz. After several days of debugging—checking the Teensy firmware, desktop collection code, and control software—I discovered the real issue was USB bandwidth contention: we had unknowingly overloaded one controller by plugging too many devices into the same hub. Simply redistributing the cameras across different USB ports resolved the problem.

- ROS may have been somewhat overkill for this project, but I chose it for several reasons:
  - Enabled software development and testing before hardware was ready, including visualizing the robot in Rviz and determining safe joint limits to prevent damage during initial testing
  - Facilitated modular software design and cross-component communication
  - Provided an opportunity to gain hands-on experience with ROS

Looking back, I believe this project could have been significantly simplified by not using ROS.

---

## Usage
See hawaii_ros_ws for usage instructions.

---

## Results
- Demonstration data (joint encoder positions and images from 4 cameras) collected reliably at 60 Hz
- Autonomously completes tasks after 25–50 demonstrations
- 1st Place at Norman Esch Pitch Competition and Best Prototype Award for graduating Mechatronics class
