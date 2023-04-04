

FROM osrf/ros:noetic-desktop-full 
SHELL ["/bin/bash", "-c"]

RUN apt-get update && apt-get -y install git \
    python3-pip \
    python3-catkin-tools 
RUN pip3 install bosdyn-client bosdyn-mission bosdyn-api bosdyn-core

RUN mkdir -p ./catkin_ws/src
WORKDIR /catkin_ws/src
RUN git clone https://github.com/telios/spot_ros.git
WORKDIR /catkin_ws
RUN source /opt/ros/noetic/setup.bash && apt-get update && \
    rosdep update && rosdep install --from-paths src --ignore-src -r -y
RUN source /opt/ros/noetic/setup.bash && catkin build

ENV SPOT_VELODYNE_HOST 192.168.1.201
ENV SPOT_VELODYNE_PORT 2370
RUN sed --in-place --expression \
      '$isource "/catkin_ws/devel/setup.bash"' \
      /ros_entrypoint.sh

ENTRYPOINT ["/ros_entrypoint.sh"]