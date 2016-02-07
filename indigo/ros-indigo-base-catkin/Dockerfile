# This Dockerfile adds a catkin workspace to the
# indigo-ros-base image. The workspace is initialized
# at /catkin_ws.
FROM ros:indigo-ros-base

MAINTAINER Jennifer Buehler

# Needed to suppress warning when installing g++:
# debconf: unable to initialize frontend: Dialog
# debconf: (TERM is not set, so the dialog frontend is not usable.)
# debconf: falling back to frontend: Readline
# ...
ENV DEBIAN_FRONTEND noninteractive

# Need to install g++ in order to enable catkin_ws
# creation (otherwise cmake fails with this error:
# CMake Error: your CXX compiler: "CMAKE_CXX_COMPILER-NOTFOUND" was not found.   Please set CMAKE_CXX_COMPILER to a valid compiler path or name.
# There is probably an easier way to fix this, but installing g++ did
# the job, even though gcc/catkin was already installed.
RUN apt-get update && apt-get install -y \
    g++ \
    vim \
    && rm -rf /var/lib/apt/lists/


# add environment setup 
RUN bin/bash -c "echo 'source /opt/ros/indigo/setup.bash' >> .bashrc"

# Setup catkin workspace
RUN /bin/bash -c "source /opt/ros/indigo/setup.bash && \
                  mkdir -p /catkin_ws/src && \
                  cd /catkin_ws/src && \
                  catkin_init_workspace && \
                  cd /catkin_ws/ && \
                  catkin_make && \
                  echo 'source /catkin_ws/devel/setup.bash' >> /.bashrc"

RUN bin/bash -c "source .bashrc"
