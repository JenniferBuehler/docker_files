# Build this image

sudo docker build -t jenniferbuehler/ros-indigo-base-catkin .

# Run ROS on several containers

sudo docker network create ros-net

sudo docker run -ti --rm --name master --net ros-net jenniferbuehler/ros-indigo-base-catkin roscore

sudo docker run -it --rm  --net ros-net --name topicls --env ROS_HOSTNAME=topicls --env ROS_MASTER_URI=http://master:11311/ jenniferbuehler/ros-indigo-base-catkin rostopic list
