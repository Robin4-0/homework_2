# Homework 2 - Manipulation

Suppose that two of the objects listed in Homework 1 are  randomly placed on the table in front of the manipulator robot. From the command line, a human user asks the robot to move one of these objects, identified via frame_id. 

The system exploits the detection method implemented for solving Homework 1 to detect the requested object. From this information, the student should implement a MoveIt! routine letting the UR5 manipulator robot pick up that object and place it on a target area while avoiding the other object on the table (collision object). The target area is freely chosen by the student. 

Suggested manipulation routine:
1.	Assign to the manipulator robot an initial configuration (home pose);
2.	Define a collision object for every object on the table;
3.	Make the robot move on a target position (pose 1) that is Z cm above the requested object pose (align the end effector with the center of the object);
4.	Make the robot move (Z-z) cm above the marker (pose 2) through a linear movement. This pose must guarantee the correct grasp of the object;
5.	Remove the target object from the collision objects;
6.	Attach the object to the eef both through MoveIt! and through the Gazebo plugin;
7.	Close the gripper;
8.	Come back to pose 1;
9.	Move the manipulator to a (Z-z) target pose (pose 3);
10.	Move the manipulator to the final pose (inside an area you selected at your choice);
11.	Open the gripper;
12.	Detached the object via MoveIt! and the Gazebo plugin.


Information about the gripper usage: 

As depicted in the robin_arena Readme.md, the following commands let actuate the gripper and solve the simulation problems related to an inaccurate gripper closure: 

### Robotiq 3-Finger Adaptive Gripper

- Activation:

```sh
$ rostopic pub --once /left_hand/command robotiq_3f_gripper_articulated_msgs/Robotiq3FGripperRobotOutput "{rACT: 1, rMOD: 0, rGTO: 0, rATR: 0, rGLV: 0, rICF: 0, rICS: 0, rPRA: 0, rSPA: 0, rFRA: 0, rPRB: 0, rSPB: 0, rFRB: 0, rPRC: 0, rSPC: 0, rFRC: 0, rPRS: 0, rSPS: 0, rFRS: 0}"
```

- Deactivation:

```sh
$ rostopic pub --once /left_hand/command robotiq_3f_gripper_articulated_msgs/Robotiq3FGripperRobotOutput "{rACT: 0, rMOD: 0, rGTO: 0, rATR: 0, rGLV: 0, rICF: 0, rICS: 0, rPRA: 0, rSPA: 0, rFRA: 0, rPRB: 0, rSPB: 0, rFRB: 0, rPRC: 0, rSPC: 0, rFRC: 0, rPRS: 0, rSPS: 0, rFRS: 0}"
```

- Close:

```sh
$ rostopic pub --once /left_hand/command robotiq_3f_gripper_articulated_msgs/Robotiq3FGripperRobotOutput "{rACT: 1, rMOD: 0, rGTO: 1, rATR: 0, rGLV: 0, rICF: 0, rICS: 0, rPRA: 250, rSPA: 200, rFRA: 200, rPRB: 0, rSPB: 0, rFRB: 0, rPRC: 0, rSPC: 0, rFRC: 0, rPRS: 0, rSPS: 0, rFRS: 0}"
```
 
- Open:

```sh
$ rostopic pub --once /left_hand/command robotiq_3f_gripper_articulated_msgs/Robotiq3FGripperRobotOutput "{rACT: 1, rMOD: 0, rGTO: 1, rATR: 0, rGLV: 0, rICF: 0, rICS: 0, rPRA: 0, rSPA: 200, rFRA: 0, rPRB: 0, rSPB: 0, rFRB: 0, rPRC: 0, rSPC: 0, rFRC: 0, rPRS: 0, rSPS: 0, rFRS: 0}"
```

### Attach/detach objects [in simulation]

- Attach object:

```sh
$ rosservice call /link_attacher_node/attach "model_name_1: 'ROBOT_NAME'
link_name_1: 'ROBOT_LINK_NAME'
model_name_2: 'OBJECT_NAME'
link_name_2: 'OBJECT_LINK_NAME'"
```

where *ROBOT_NAME*, *ROBOT_LINK_NAME*, *OBJECT_NAME*, and *OBJECT_LINK_NAME* are the GAZEBO names of robot and object that has to be attached - and their links. E.g., if the user wants to attach the UR5 robot and the third simulated cube: *ROBOT_NAME = robin_ur5*, *ROBOT_LINK_NAME = wrist_3_link*, *OBJECT_NAME = cube3*, and *OBJECT_LINK_NAME = cube3_link*. 

- Detach object:

```sh
$ rosservice call /link_attacher_node/detach "model_name_1: 'ROBOT_NAME'
link_name_1: 'ROBOT_LINK_NAME'
model_name_2: 'OBJECT_NAME'
link_name_2: 'OBJECT_LINK_NAME'"
```
where names are defined as before.

