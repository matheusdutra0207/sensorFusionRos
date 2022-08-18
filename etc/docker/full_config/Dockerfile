FROM ros:noetic-ros-base

# Change the default shell to Bash
SHELL [ "/bin/bash" , "-c" ]

RUN source /opt/ros/noetic/setup.bash \
    && mkdir -p /opt/ros/src \
    && cd opt/ros/src \
    && catkin_create_pkg reconstruction_3d std_msgs rospy roscpp

COPY src/reconstruction_3d/src opt/ros/src/reconstruction_3d/src

RUN apt-get update \
    &&  apt-get --yes install ros-noetic-navigation \
    && apt-get --yes install ros-noetic-robot-localization \
    && apt-get --yes install python3-pip

WORKDIR opt/ros

COPY requirements.txt .
RUN pip3 install -r requirements.txt
RUN pip3 install --upgrade protobuf==3.20.0

RUN source /opt/ros/noetic/setup.bash && \
catkin_make && \
source devel/setup.bash

WORKDIR /opt/ros/map
COPY src/full_config/map/labsea.yaml .
COPY src/full_config/map/labsea.pgm .

WORKDIR /opt/ros/config/amcl_config
COPY src/full_config/config/amcl_config/amcl_template.yaml .

WORKDIR /opt/ros/config/rec_config
COPY src/full_config/config/rec_config/reconstruction_template.yaml .

WORKDIR /opt/ros/config/robot_localization_config
COPY src/full_config/config/robot_localization_config/ekf_template.yaml .

WORKDIR /opt/ros/launch
COPY src/full_config/launch/sensor_fusion_ekf.launch .

COPY bootstrap.sh .
RUN chmod +x bootstrap.sh
