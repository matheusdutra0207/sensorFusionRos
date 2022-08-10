FROM osrf/ros:noetic-desktop-full

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

# Install Robot localization
RUN apt-get update && apt-get --yes install ros-noetic-navigation

RUN source /opt/ros/noetic/setup.bash

WORKDIR /opt/ros/map
COPY map.yaml .
COPY map.pgm .

WORKDIR /opt/ros/config
COPY amcl_template.yaml .

WORKDIR /opt/ros/launch
COPY amcl.launch .