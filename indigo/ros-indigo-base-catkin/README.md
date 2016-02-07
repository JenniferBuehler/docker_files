# Build this image

``sudo docker build -t jenniferbuehler/ros-indigo-base-catkin .``

# Run ROS on several containers

This Docker image adds a catkin workspace to the official ros::indigo-ros-base docker image.
The workspace is initialized in /catkin_ws. You may initialize ROS by sourcing the /.bashrc.

This image can be used as a base for your other packages which require a catkin workspace to compile.

# Usage
Usage is as with the docker images [ros::<distro>-ros-base](https://hub.docker.com/_/ros/):

To use ROS from several terminals, for example run roscore in a separate terminal:

1. First start a network:      
    ``sudo docker network create ros-net``

2. Then, run roscore:    
    ``sudo docker run -ti --rm --name master --net ros-net jenniferbuehler/ros-indigo-base-catkin roscore``

3. Then you may run other ROS nodes in another terminal, e.g. just run a rostopic list:    
    ``sudo docker run -it --rm  --net ros-net --name topicls --env ROS_HOSTNAME=topicls --env ROS_MASTER_URI=http://master:11311/  jenniferbuehler/ros-indigo-base-catkin rostopic list``
