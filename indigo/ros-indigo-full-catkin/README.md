# Build this image

``sudo docker build -t jenniferbuehler/ros-indigo-full-catkin .``


# About this image

This docker image is based on [jenniferbuehler/ros-indigo-base-catkin](https://hub.docker.com/r/jenniferbuehler/ros-indigo-base-catkin/)
and adds the ubuntu package ros-indigo-desktop-full to it.

Additionally, there are some efforts in testing stage to support running
on OpenGL applications.
Despite the full ROS support incl. Gazebo,
this docker image does not support graphical interfaces with OpenGL,
because support for this depends on the locally used graphics card.
The problem is that it looks like the same graphic drivers as the
hosts is required in order to enable hardware acceleartion.    
See also comment in the [DockerHub Gazebo](https://hub.docker.com/_/gazebo/) description,
*Deployment suggestions -> Devices*.    
I'm still looking into whether it's possible to run gazebo
without HW acceleration and using Mesa libraries only....

# Run ROS on several containers

Usage is as with the docker images [ros::<distro>-ros-base](https://hub.docker.com/_/ros/):

To use ROS from several terminals, for example run roscore in a separate terminal:

1. First start a network:      
    ``sudo docker network create ros-net``

2. Then, run roscore:    
    ``sudo docker run -ti --rm --name master --net ros-net jenniferbuehler/ros-indigo-full-catkin roscore``

3. Then you may run other ROS nodes in another terminal, e.g. just run a rostopic list:    
    ``sudo docker run -it --rm  --net ros-net --name topicls --env ROS_HOSTNAME=topicls --env ROS_MASTER_URI=http://master:11311/  jenniferbuehler/ros-indigo-full-catkin rostopic list``


# Run gazebo

**UNDER DEVELOPMENT**

At this stage, you cannot run Gazebo yet. The instructions
are included for the test stage.

If you want to run gazebo, you will always first have to call

``xhost +``

in order to allow clients to connect from any host.

Then:

``sudo docker run -ti --rm -e DISPLAY=$DISPLAY --privileged -v /tmp/.X11-unix:/tmp/.X11-unix jenniferbuehler/ros-indigo-full-catkin gazebo``
