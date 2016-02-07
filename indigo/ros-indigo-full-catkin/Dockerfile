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

FROM jenniferbuehler/ros-indigo-base-catkin 

MAINTAINER Jennifer Buehler

# install ros-indigo-desktop-full
RUN apt-get update && apt-get install -y \
    ros-indigo-desktop-full \
    && rm -rf /var/lib/apt/lists/


# TESTING: see if it's possible to run
# without hardware accelleration.
RUN apt-get update && apt-get install -y \
    mesa-utils \
    && rm -rf /var/lib/apt/lists/

# just in case it's not overwritten,
# set to default 0 here
ENV DISPLAY :0
