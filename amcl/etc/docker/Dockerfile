FROM ros:noetic-ros-base

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Install Robot localization
RUN apt-get update && apt-get --yes install ros-noetic-navigation

RUN source /opt/ros/noetic/setup.bash

WORKDIR /opt/ros/config
COPY amcl/config/amcl_template.yaml .

WORKDIR /opt/ros/launch
COPY amcl/launch/amcl.launch .