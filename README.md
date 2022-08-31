# Sensor Fusion

This project use the package [robot localization](http://wiki.ros.org/robot_localization) to fuse the robot poses estimation. The poses are estimated by two methods: 3D visual reconstruction and [ACML](http://wiki.ros.org/amcl)(Monte Carlo localization).


### Issues:

#### The Cluster k8s network

ROS is a distributed computing environment. However, it has requirements of the network configuration: 
- There must be complete, bi-directional connectivity between all pairs of machines, on all ports. 
- Each machine must advertise itself by a name that all other machines can resolve.  

This project proposes to execute all the ROS necessary packages in a cluster k8s. This [tutorial](https://blog.zhaw.ch/icclab/challenges-with-running-ros-on-kubernetes/) show a solution to execute ROS nodes in the cluster k8s, but it not works in this project because the robot network does not belong to the cluster network and ROS nodes use TCP/UDP/DDS to communicate with each other on random ports, while k8s wants to expose services on configurable ports and typically over HTTP(S). SO, [Ingress controllers](https://kubernetes.io/docs/concepts/services-networking/ingress/) don't really play well with ROS.

The initial solution for all this is to declare the ROS packages with the manifest `hostnetwork:True`. But, any other solutions for this problem are welcome as suggestions :).
