# In order to support graphical interfaces,
# this should be run with 
# docker run -it --rm \
#     -e DISPLAY=$DISPLAY \
#     -v /tmp/.X11-unix:/tmp/.X11-unix \  
#     ros-indigo-full-catkin <cmd> 
#
# The -e and -v commands are needed to display on the host X server.
# For hardware support, you will also need:
#  --privileged   (to access the graphics card) 
#  It may also be required to call
#  $ xhost +
#  before running the container.

FROM jenniferbuehler/ros-indigo-full-catkin 

MAINTAINER Jennifer Buehler


# Install required dependencies
RUN apt-get update && apt-get install -y \
    libsoqt4-dev \
    libcoin80-dev \
    libqt4-dev \
    libblas-dev \
    liblapack-dev \
    libqhull-dev \
    && rm -rf /var/lib/apt/lists/

# Install required ROS dependencies
RUN apt-get update && apt-get install -y \
    ros-indigo-eigen-conversions \
    ros-indigo-household-objects-database \
    ros-indigo-household-objects-database-msgs  \
    ros-indigo-manipulation-msgs \
    ros-indigo-object-recognition-msgs \
    ros-indigo-roslint \
    && rm -rf /var/lib/apt/lists/

# Install Jaco dependencies
RUN apt-get update && apt-get install -y \
    ros-indigo-control-msgs \
    ros-indigo-laser-filters \
    ros-indigo-openni-launch \
    && rm -rf /var/lib/apt/lists/


# get jaco dependencies into catkin_ws
RUN bin/bash -c "cd /catkin_ws/src \
     && git clone https://github.com/Kinovarobotics/kinova-ros.git \
     && rm -rf kinova-ros/jaco_demo \ 
     && rm -rf kinova-ros/jaco_driver \ 
     && rm -rf kinova-ros/jaco_model"

# get graspit git repo
RUN bin/bash -c "cd /catkin_ws/src \
     && git clone https://github.com/JenniferBuehler/graspit.git"

# XXX test stage: force removal of existing catkin_ws
RUN bin/bash -c "rm -rf /catkin_ws/src/jb-ros-packs"

# get jb-ros-packs git repo
RUN bin/bash -c "cd /catkin_ws/src \
     && git clone https://github.com/JenniferBuehler/jb-ros-packs.git"

# TODO: We still have to make sure that in <graspit-dir>/CMakeLists.txt:
# set (BUILD_TESTS false)
# However for now this is default in my fork of graspit.
# After that, we could ensure that the instructions to set up gtest
# are done here as well.

# Build
RUN bin/bash -c "source /.bashrc \
    && echo $CATKIN_SHELL \
    && cd /catkin_ws \
    && catkin_make \
    && catkin_make install"

# set GRASPIT environment to /graspit_home
RUN bin/bash -c "mkdir -p /graspit_home"
ENV GRASPIT /graspit_home

# set PATH to the catkin install directory
# RUN bin/bash -c "echo 'export PATH=$PATH:/catkin_ws/install/lib/' >> .bashrc"
# RUN bin/bash -c "cat .bashrc >> .profile"
ENV PATH ${PATH}:/catkin_ws/install/lib/

#RUN bin/bash -c "echo 'echo src-bashrc' >> .bashrc"
RUN bin/bash -c "source .bashrc"

# setup entrypoint
COPY ./entrypoint /
ENTRYPOINT ["/entrypoint"]

CMD ["bash","-l"]
