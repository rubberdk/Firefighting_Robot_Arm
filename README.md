# Firefigther Robot Arm
HDT Adroit 6DOF A24 Pincer Manipulation of a fire extingushier using thermal camera

written by Dong Ho Kang (DK)

You can find more background of this probject from:
(portfolio link)

## INTRODUCTION
This project contatins a firefighting mechanism using HDT Adroit 6 DOF A24 Pincer robot arm and FLIR Lepton 2.5 thermal camera for this project. The goal of this project is to detect a heat source (fire), grab the fire extinguisher, and operate it.

Manipulating a robot arm to detect a fire and use a fire extinguisher can be beneficial in industrial settings such as warehouses which have already adapted using robot arms. Especially, maintenance costs of fire sprinkles due to frozen pipes can be deducted in warehouses. Additionally, implementing a fire safety feature to a domestic service robot arms can appeal to the general crowd.

## HARDWARE REQUIREMENTS
- HDT Adroit 6DOF A24 Pincer
- Fixed-hose fire extinguisher
- FLIR Lepton 2.5 - Thermal Imaging Module
- PureThermal 2 - FLIR Lepton Smart I/O Board

## SOFTWARE REQUIREMENTS
- ROS-noetic (http://wiki.ros.org/noetic/Installation)
- Moveit (https://moveit.ros.org/install/)
- rqt_image_view

## PACAGKES
This repository contatins two packages, and you can find their details in their README files

### PACAKGE 1: `thermal_image_processing`
This package takes care of detecting a highest spot in the camera view and reading its temperature from the thermal camera.

I implemented the node from USB Video Class capture examples for PureThermal 1 / PureThermal 2 FLIR Lepton Dev Kit
(https://github.com/groupgets/purethermal1-uvc-capture).

### PACAKGE 2: `control`
This package creates enviroment for Moveit and controls the arm and pincer movement based on information published from `thermal_image_processing` package


## OPERATION
Note that you will need `hdt_6dof_a24_pincer` control packages to use for the operation.

1. First, you will need to start the robot.
    Run `roslaunch control hdt_arm_bringup.launch`
    this launch file loads controllers for the robot to move.
    Note that if you plug in xbox controller or dualshock4 controller, you may not be able to use the robot using codes.

    If you want to run the robot in simulation,
    Run `roslaunch control hdt_arm_bringup.launch simulation:=true`
    `simulation` argument is `false` by default.

2. Then, you can start the thermal camera and arm control nodes
    Run `roslaunch control fire_fight.launch`
        This will create enviroment in Rviz for the Moveit.
        The robot will move to its `grab_ready` position

    `grab_ready` position looks like this:

    ![grab_ready](https://github.com/rubberdk/Firefighting_Robot_Arm/blob/master/images/grab_ready.jpg?raw=true)

    And this is how I attached the thermal camera on robot. If you change the orientation of the camera, you will have to adjust inital joint angles for `search` function in `arm_control` node accordingly.


3. Now, you can operate the robot using `rosservice`

- `rosservice call /ready` : It moves the robot to its `ready` position.
- `rosservica call /grab_ready` : It moves the robot to its `grab_ready` position.
- `rosservica call /search` : It scans the robot's enviroment and executes joint movements to align the center of the camera view with the heat source, and it remembers the robot's current joint positions.
- `rosservica call /grab` : It grabs the fire extingushier.
- `rosservica call /locate`: It moves to stored joint positions from `search`. This service is for to aim the fire with the fire extinguisher after grabbing it.
- `rosservica call /press` : It closes the grippers so that it will press the lever of the fire extinguisher.
- `rosservica call /open` : It opend its grippers wide. 




