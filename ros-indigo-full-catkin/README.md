# UNDER CONSTRUCTION

This Dockerfile is not working yet with the graphics.
Problem is that it looks like everybody needs to install
the same graphics driver as they have on their machine in order
to enable hardware acceleartion. I'm still looking in to whether
it's possible to run gazebo without HW acceleration and 
using Mesa libraries only....

# Build this image

sudo docker build -t jenniferbuehler/ros-indigo-full-catkin .

# Run gazebo

If you want to run gazebo, you will always first have to call

``xhost +``

first, in order to allow clients to connect from any host.

Then:

sudo docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jenniferbuehler/ros-indigo-full-catkin gazebo


# Run ROS on several containers

First start network:

sudo docker network create ros-net

sudo docker run -ti --rm --name master --net ros-net -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix jenniferbuehler/ros-indigo-full-catkin roscore

sudo docker run -it --rm  --net ros-net  --name topicls --env ROS_HOSTNAME=topicls  --env ROS_MASTER_URI=http://master:11311/ jenniferbuehler/ros-indigo-full-catkin rostopic list
