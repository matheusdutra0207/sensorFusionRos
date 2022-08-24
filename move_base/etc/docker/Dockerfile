FROM ros:noetic-ros-base

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

RUN apt-get update \
    &&  apt-get --yes install ros-noetic-navigation 

WORKDIR opt/ros

RUN source /opt/ros/noetic/setup.bash 

WORKDIR /opt/ros/move_base/config/base_local_planner_config
COPY src/move_base/config/base_local_planner_config/base_local_planner_params.yaml .

WORKDIR /opt/ros/move_base/config/costmap_common_config
COPY src/move_base/config/costmap_common_config/costmap_common_params.yaml .

WORKDIR /opt/ros/move_base/config/global_costmap_config
COPY src/move_base/config/global_costmap_config/global_costmap_params.yaml .

WORKDIR /opt/ros/move_base/config/local_costmap_config
COPY src/move_base/config/local_costmap_config/local_costmap_params.yaml .

WORKDIR /opt/ros/move_base/launch
COPY src/move_base/launch/move_base.launch .

