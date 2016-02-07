FROM jenniferbuehler/graspit 

MAINTAINER Jennifer Buehler

ENV DEBIAN_FRONTEND noninteractive

# Install immediate depenencies which do not
# already come with graspit
RUN apt-get update && apt-get install -y \
    git \
    libeigen3-dev \
    && rm -rf /var/lib/apt/lists/

# install boost dependencies (may be removed
# in future)
RUN apt-get update && apt-get install -y \
    libboost-filesystem-dev \
    libboost-system-dev \
    libboost-thread-dev \
    libboost-program-options-dev \
    && rm -rf /var/lib/apt/lists/

# Pull the repository and build
RUN  bin/bash -c "mkdir -p /grasp_planning_graspit \
    && cd /grasp_planning_graspit \
    && git clone https://github.com/JenniferBuehler/jb-ros-packs.git \
    && mv jb-ros-packs/grasp_planning_graspit/* . \
    && rm -rf jb-ros-packs \
    && mkdir -p build \
    && cd build \
    && cmake -DGRASPIT_PATH=/usr -DCMAKE_INSTALL_PREFIX=/usr .. \
    && make && make install"
