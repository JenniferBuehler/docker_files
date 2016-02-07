### Build this image

``sudo docker build -t jenniferbuehler/jb-ros-packs .``

### About this image

This docker image is based on [jenniferbuehler/ros-indigo-full-catkin](https://hub.docker.com/r/jenniferbuehler/ros-indigo-full-catkin)
and adds the [jb-ros-packs](https://github.com/JenniferBuehler/jb-ros-packs) sources and dependencies.

### Using the GraspIt! planner 

Notes for using the GraspIt! planning algorithms:

* The GRASPIT environment variable is set to 
    the folder ``/graspit_home`` on the image. You can mount
    a folder on your computer to it so that you can use yor own robot files.
* You will need to connect your display to the docker image,
    because even though the graspit code runs in headless mode,
    it internally still requires X for Qt.

**Set up GraspIt folders and display sharing**

```
mkdir <your-graspit-home>
cp -rf <your-graspit-models-dir> <your-graspit-home>
xhost +
```

Note that **no soft links** are supported within ``<your-graspit-home>``, you will have to copy
entire files and directories into it.

### Run the planner directly 
 
**Step 1. Log on to the bash**

```
sudo docker run -ti --rm -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v <your-graspit-home>:/graspit_home \
    jenniferbuehler/jb-ros-packs
```

**Step 2. Running the planner**

Now you can test the planning from within the image terminal. You can access the files in ``<your-graspit-home>``
via ``/graspit_home``.

``grasp_planning --wld /graspit_home/worlds/<your-world>.xml --dir /graspit_home/<results-directory>``

This will place the results in ``<your-graspit-home>/tmp_results``.

For example for the Jaco arm sample files:

``grasp_planning --wld /graspit_home/worlds/jaco_robot_world.xml --dir /graspit_home/tmp_results/``


### Using ROS nodes to run the graspit planner

To run the planner with ROS nodes, you need to connect the ROS nodes started from
different terminals.

**Step 1. Start a network**

``sudo docker network create ros-net``

**Step 2. Run roscore on a container called "master"**

``sudo docker run -ti --rm --name master --net ros-net jenniferbuehler/jb-ros-packs roscore``

**Step 3. Start the planner**

*UNDER CONSTRUCTION*

Neither option doesn't really work yet saving the files
(also not with ``/graspit_home`` instead of ``<your-graspit-home>``)

```
sudo docker run -it --rm  --net ros-net --name graspit_services \
    -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix \
    --env ROS_HOSTNAME=graspit_services \
    --env ROS_MASTER_URI=http://master:11311/ \
    jenniferbuehler/jb-ros-packs \
    roslaunch grasp_planning_graspit_ros \
    graspit_planner_jaco_example.launch results_output_directory:=<your-graspit-home>/<results-directory>
```

*Alternatively:*

Log onto the bash first:

```
sudo docker run -it --rm  --net ros-net --name graspit_services \
    -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix \
    --env ROS_HOSTNAME=graspit_services \
    --env ROS_MASTER_URI=http://master:11311/ \
    jenniferbuehler/jb-ros-packs
```

And then launch the planner:

```
    roslaunch grasp_planning_graspit_ros \
    graspit_planner_jaco_example.launch \
    results_output_directory:=/graspit_home/<results-directory>
```
