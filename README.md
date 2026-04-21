# KLI_Fusion Dataset

This is a point\-foot biped robot dataset containing leg kinematics \(joint encoders\), IMU and LiDAR data, collected for evaluating kinematic\-LiDAR\-inertial odometry methods for dynamic biped locomotion\.

![Oerview of KLI_Fusion](https://github.com/tanjinqian/KLI_Fusion/blob/main/Figs/toc_fig.pdf)

## Sequence

The publicly available datasets include simulation sequences and indoor experimental sequences\. The detailed introduction of each sequence is as follows:

- **sim\_forward_backward**: Simulation dataset collected in the Gazebo simulator\. The robot performed forward and backward dynamic motions, which is designed to test the Z\-axis drift suppression performance under abrupt motion changes\.

- **sim\_square_walk**: Simulation dataset collected in the Gazebo simulator\. The robot walked along a square trajectory to evaluate the overall localization and mapping accuracy\.

- **Vc_20**: Indoor experimental dataset collected in the motion capture lab\. The robot walked at 20% of its maximum speed, with ground truth provided by the NOKOV motion capture system\.

- **Vc_40**: Indoor experimental dataset collected in the motion capture lab\. The robot walked at 40% of its maximum speed, with ground truth provided by the NOKOV motion capture system\.

- **Vc_60**: Indoor experimental dataset collected in the motion capture lab\. The robot walked at 60% of its maximum speed, with ground truth provided by the NOKOV motion capture system\.

- **Vc_90**: Indoor experimental dataset collected in the motion capture lab\. The robot walked at 90% of its maximum speed, with ground truth provided by the NOKOV motion capture system\.

- **TRON1A.URDF and robot_description**:  The point-foot biped robot model and description.

### Rosbag Download

All the publicly available sequences are provided in link [Google Drive](https://drive.google.com/drive/folders/1SIEd3t_W9r6strmSbirXIwOsUkqxkCpL?usp=drive_link)\. The indoor sequences contain the ground truth from the NOKOV motion capture system for algorithm evaluation\. We will publish the outdoor datasets in the future\.

## Sensor Information

### sensor type

The data set includes LiDAR, IMU, joint encoders, etc\. Taking the sequences **sim\_forward\_backward**: and **indoor\_Vc90** as examples, the specific data format types are as follows:

```
path:        sim_forward_backward.bag
version:     2.0
duration:    57.6s
start:       Jan 01 1970 08:06:04.63 (364.63)
end:         Jan 01 1970 08:07:02.21 (422.21)
size:        413.4 MB
messages:    68304
compression: none [432/432 chunks]
types:       liko/kinect                      [31e69174d512daa8ff27306fe84f8504]
             livox_laser_simulation/CustomMsg [e4d6829bdfe657cb6c21a746c86b21a6]
             nav_msgs/Odometry                [cd5e73d190d741a2f92e81eda573aca7]
             sensor_msgs/Imu                  [6a62c6daae103f4ff57a132d6f95cec2]
             sensor_msgs/PointCloud2          [1158d486dd51d683ce2f1be655c3c181]
topics:      /ground_truth/state   28789 msgs    : nav_msgs/Odometry               
             /livox                  576 msgs    : sensor_msgs/PointCloud2         
             /livox/custom_msg       576 msgs    : livox_laser_simulation/CustomMsg
             /livox/imu             9596 msgs    : sensor_msgs/Imu                 
             /robot_motor          28767 msgs    : liko/kinect
```

```Plapath:        vc_90.bag
path:        Vc_90.bag
version:     2.0
duration:    1:23s (83s)
start:       Apr 09 2026 20:07:37.40 (1775736457.40)
end:         Apr 09 2026 20:09:00.41 (1775736540.41)
size:        326.4 MB
messages:    164130
compression: none [416/416 chunks]
types:       liko/kinect                 [31e69174d512daa8ff27306fe84f8504]
             livox_ros_driver2/CustomMsg [e4d6829bdfe657cb6c21a746c86b21a6]
             sensor_msgs/Imu             [6a62c6daae103f4ff57a132d6f95cec2]
topics:      /livox/imu      16602 msgs    : sensor_msgs/Imu            
             /livox/lidar      830 msgs    : livox_ros_driver2/CustomMsg
             /robot_motor   146698 msgs    : liko/kinect

```

### sensor extrinsic parameters

Extrinsic parameters from **IMU** to **Robot_base** are as follows (For Livox Mid\-360, this external parameter is fixed) :

```
# IMU to LiDAR extrinsic parameters
extrinsic_T: [ -0.011, -0.02329, 0.04412 ]
extrinsic_R: [ 1, 0, 0,
               0, 1, 0,
               0, 0, 1]
```

Extrinsic parameters from **IMU** to **Robot_base** are pre\-calibrated and stored in the **URDF**\. We designed a rigid custom sensor mounting platform to ensure stable inter\-sensor connections under severe dynamic impacts, avoiding extrinsic parameter changes during locomotion\.

### /livox/lidar : livox_ros_driver2/CustomMsg

**INCLUDE: /livox:sensor_msgs/PointCloud2; /livox/custom_msg:livox_laser_simulation/CustomMsg.**

This is the data provided by Livox Mid\-360 semi\-solid\-state LiDAR at roughly 10 HZ.

### /livox/imu: sensor\_msgs/Imu

This is the data provided by the built\-in ICM40609 IMU of the Livox LiDAR, which contains 9\-axis measurement information \(acceleration, angular velocity, quaternion\) at 200Hz\.

###  /robot_motor : liko/kinect

This is the data provided by the joint encoders of our point\-foot biped robot, which contains measurement information of joint angle, joint angular velocity and joint torque at 2000Hz\. We align its time with the IMU time and reduce its frequency to 200Hz to minimize the system's computational burden.  The joint torque is used for contact foot detection in our KLI\-Fusion framework\.

```
# the kinect motor msg in TRON1
uint64  stamp # Timestamp, usually indicating the time when these data were recorded or generated, in nanoseconds
float32[6] tau       # A vector used to store the current estimated output torque (in N*m)
float32[6] q      # A vector used to store the current angle (in radians)
float32[6] dq      # A vector storing the current velocity (in radians per second)
```

### Note

For more details about the KLI\-Fusion framework, please refer to our paper:
*KLI\-Fusion: Tightly\-coupled Kinematic\-LiDAR\-Inertial Odometry with Contact Foot Position Enhancement for Point\-foot Biped Robots*.
