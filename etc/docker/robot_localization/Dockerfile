FROM osrf/ros:noetic-desktop-full

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Install Robot localization
RUN apt-get update && apt-get --yes install ros-noetic-robot-localization

RUN source /opt/ros/noetic/setup.bash

WORKDIR /opt/ros/config
COPY ekf_template.yaml .

WORKDIR /opt/ros/launch
COPY ekf.launch .








