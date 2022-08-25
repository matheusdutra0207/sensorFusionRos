FROM ros:noetic-ros-base

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Install Robot localization
RUN apt-get update && apt-get --yes install ros-noetic-robot-localization

RUN source /opt/ros/noetic/setup.bash

WORKDIR /opt/ros/config
COPY robot_localization/config/ekf_template.yaml .

WORKDIR /opt/ros/launch
COPY robot_localization/launch/ekf.launch .








