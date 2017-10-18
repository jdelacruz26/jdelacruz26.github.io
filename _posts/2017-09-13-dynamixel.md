---
layout: post
comments: true
title:  "Controlling the Dynamixel XH430-V210"
desc: "This is a code aimed to control the Dynamixel XH430-V210 motor"
keywords: "code,OpenCM,ROS,Dynamixel"
date: 2017-10-13
categories: [Ros]
tags: [ROS, Dynamixel, OpenCM, Ubuntu]
icon: fa-gears
---

# <i class="fa fa-cogs" aria-hidden="true"></i> Dynamixel XH430-V210

This is a code aimed to control the Dynamixel XH430-V210 motor. The operation of the motor consists in driving the motor to two specific positions without exceeding a giving torque, *open* and *closed* positions. In order to do this, a *current-based Position control* is used.

---

## <i class="fa fa-list" aria-hidden="true"></i> Dependencies
* [**OpenCM9.04**][1]
* [**OpenCM 485 EXP**][2]
* [**OpenCM IDE:**][3]
* [**Dynamixel Motor XH430-V210**][4]

---
## <i class="fa fa-cog" aria-hidden="true"></i> Guide lines for driving the motor using the OpenCM9+OpenCM 485 EXP board
1. Connect the OpenCM 485 EXP board to a 24V source. <i class="fa fa-cogs" aria-hidden="true"></i>


1. Approach the Dynamixel motor's shaft to the initial position (*open position*) manually.

1. Power on the OpenCM expansion board using the *Power Switch*.

1. Hold **Button1** until the blue led and the green led turn on. Now the controller will move the motor's shaft to the initial position, *open position*. Once the motor reaches the initial position, the controller will deactivate the torque.  

1. It is time to drive the Dynamixel!, be sure that the workbench has been already set it up. Hold **Button2** until the red led and the green led turn on and then release it in order to drive the motor to the *closed Position*. As in the previous step,the controller will drive the shaft until the *maximum position* or the *maximum torque* are reached.

1.  <i class="fa fa-exclamation" aria-hidden="true"></i> It is possible to enable or disable the torque manually, this could be done pushing the **Button1** and **Button2** at the same time.

1. <i class="fa fa-exclamation-triangle" aria-hidden="true"></i> **Warning**: Once you have finished using the system, return the shaft to the initial position before powering off the motor or the OpenCM expansion board. Notice that the next time the controller is going to initialize the motor, it will drive the shaft to the absolute position taking in account only one turn.

<!--![OpenCM9.0 + 485 EXP board](figure/PenCM.png){width=50%}-->

<div style="text-align:center;">
<img src="/static/assets/img/blog/ros/PenCM.png" alt="Drawing" width= "500px"/>
</div>
<div style="text-align:center">
OpenCM9.04 + OpenCM 485 EXP board + Dynamixel XH430-V210
</div>

## <i class="fa fa-info" aria-hidden="true"></i> Leds codification

The control board has three different leds which can be used in order to give a feedback about the current process or state of the code running on the board.

* **Blue led**: It indicates the state of the torque. The torque has two possible states, enable and disable. When it is enable, the blue led will turn on; it will turn off when it is disable.

* **Green led**: The green led turns on when the controller commands the shaft to the initial position. Once the initial position is reached, the led will still on until the current position exceeds a tolerance, in this case, the led will start flashing. When the green led is flashing means the shaft has an offset respect to the *open position*.

* **Red led**:The red led turns on when the shaft is commanded to the *closed position*. Once the closed position is reached the led will still on, at least the shaft is moved from his reference position.if this occurs, the red led will start flashing.

## <i class="fa fa-github" aria-hidden="true"></i> [OpenCM code](https://github.com/jdelacruz26/misccode/blob/dynamixel/sketchbook/igmr_dynamixel_control/igmr_dynamixel_control.ino)
**Initialization values** are the values that can be modified by the user. Notice that the *open_position* will be always taken as 0 degrees.  

```c
/*********************** initializing values ********************************/
float closed_position_rev = 7 ;//number of revolutions
float open_position_rev = 0; //initial position, 0 rev.
float closed_current_A = 0.15; //target current in A. Max value = 0.50
float open_current_A = 0.18; //target current in A. Max value = 0.50
float profile_velocity_rps = 0.7; //revolution per second (RPS). Max value = 1.45
int pos_threshold = 57; //~5°
```
---
[1]:http://support.robotis.com/en/product/controller/opencm9.04.htm
[2]:http://support.robotis.com/en/product/controller/opencm_485_exp.htm
[3]:http://support.robotis.com/en/software/robotis_opencm_main.htm
[4]:http://support.robotis.com/en/product/actuator/dynamixel_x/xh_series/xh430-w350_main.htm