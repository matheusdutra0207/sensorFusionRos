# Sensor Fusion

This project use the package [robot localization](http://wiki.ros.org/robot_localization) to fuse the robot poses estimation. The poses are estimated by two methods: [3D visual reconstruction](https://github.com/matheusdutra0207/is-reconstruction) and [ACML](http://wiki.ros.org/amcl) (Monte Carlo localization).

## Dependencies:

[Ros-map-server-microsservice](https://github.com/vinihernech/ros-map-server-microsservice): provide the environment map for ros and PIS.

## Robot localization package

The use of the robot_localization package is very well explained in this links:

- [Ros-sensor-fusion-tutorial](https://github.com/methylDragon/ros-sensor-fusion-tutorial/blob/master/01%20-%20ROS%20and%20Sensor%20Fusion%20Tutorial.md);

- [The Ros Robot_localization package](https://kapernikov.com/the-ros-robot_localization-package/).

In this [project](https://github.com/methylDragon/ros-sensor-fusion-tutorial/blob/master/02%20-%20Global%20Pose%20Estimate%20Fusion%20(Example%20Implementation).md), the robot localization package was used to Fusing the Marvelmind Indoor 'GPS' with amcl. 

## Amcl configuration

The deployment file, which is in [amcl/etc/k8s](https://github.com/matheusdutra0207/sensorFusionRos/blob/main/amcl/etc/k8s/deployment.yaml), contains the amcl configuration we are currently using. However, if you want to use robot_localization to provide the `map --> odom` transformation, you must set the tf_broadcast parameter to False (`tf_broadcast: False`).

## Is-reconstruction configuration

The ROS-translation microservice is used for the integration of Ros with the intelligent space. However, this project does not make use of it yet. Thus, the [reconstruction_3d](https://github.com/matheusdutra0207/sensorFusionRos/tree/main/reconstruction_3d) package is used to make the is-reconstruction measurements available to ROS.

### Streams:
| Name | ⇒ Input | Output  ⇒ | Description |
| ---- | ------- | --------- | ----------- |
| is/vo | :incoming_envelope: **topic:** `reconstruction.{ArUco_id}.ArUco` <br> :gem: **schema:** [Pose](https://github.com/labviros/is-msgs/tree/master/docs#is.common.Pose) | :incoming_envelope: **topic:**  `is/vo` <br> :gem: **schema:** [PoseWithCovarianceStamped](http://docs.ros.org/en/lunar/api/geometry_msgs/html/msg/PoseWithCovarianceStamped.html) | Takes the marker pose and publishes it to ROS with a specific covariance, which is statically defined in the [package configuration](https://github.com/matheusdutra0207/sensorFusionRos/blob/main/reconstruction_3d/etc/config/reconstruction.yaml).|

## Robot_localization configuration

The deployment file, which is in [robot_localization/etc/k8s](https://github.com/matheusdutra0207/sensorFusionRos/blob/main/robot_localization/etc/k8s/deployment.yaml), contains the robot_localization configuration we are currently using.

## Full_config configuration

Unites all three configurations (amcl, Is-reconstruction e robot_localization) into a single [deployment](https://github.com/matheusdutra0207/sensorFusionRos/blob/main/full_config/etc/k8s/deployment.yaml).

## Issues:

### The Cluster k8s network

ROS is a distributed computing environment. However, it has requirements of the network configuration: 
- There must be complete, bi-directional connectivity between all pairs of machines, on all ports. 
- Each machine must advertise itself by a name that all other machines can resolve.  

This project proposes to execute all the ROS necessary packages in a cluster k8s. This [tutorial](https://blog.zhaw.ch/icclab/challenges-with-running-ros-on-kubernetes/) show a solution to execute ROS nodes in the cluster k8s, but it not works in this project because the robot network does not belong to the cluster network and ROS nodes use TCP/UDP/DDS to communicate with each other on random ports, while k8s wants to expose services on configurable ports and typically over HTTP(S). SO, [Ingress controllers](https://kubernetes.io/docs/concepts/services-networking/ingress/) don't really play well with ROS.

The initial solution for all this is to declare the ROS packages with the manifest `hostnetwork:True`. But, any other solutions for this problem are welcome as suggestions :).
