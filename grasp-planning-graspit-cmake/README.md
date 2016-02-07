This docker image is based on jenniferbuehler/graspit,
which does not support graphical interfaces with OpenGL,
because support for this depends on the locally used
graphics card.

So while you cannot use graspit_simulator (unless you extend this image accordingly),
you can run the EigenGrasp planner in headless mode.

This image installs grasp_planning_graspit
in the /usr directory of the image after pulling it from my
 git repository and building it with cmake.

# Build this image

``sudo docker build -t jenniferbuehler/grasp_planning_graspit .``

# 1. Starting the bash terminal

The $GRASPIT environment variable is set to 
the folder ``/graspit_home`` on the local image. You can mount
a folder on your computer to it so that you can use your own robot files.

You will still need to connect your display, because even though 
the graspit code runs in headless mode, it internally still
requires X for Qt.

```
mkdir <your-graspit-home>
cp -rf <your-graspit-"models"-dir> <your-graspit-home>
xhost +
sudo docker run -ti --rm -e DISPLAY=$DISPLAY -v /tmp/.X11-unix:/tmp/.X11-unix -v <your-graspit-home>:/graspit_home jenniferbuehler/grasp_planning_graspit
```


# 2. Running the planner

Now you can test the planning from within the image terminal. You can access the files in ``<your-graspit-home>``
via ``/graspit_home``.

*Note*: **no soft links** are supported within ``<your-graspit-home>``, you will have to copy
entire files and directories into it.

``grasp_planning --wld /graspit_home/worlds/<your-world>.xml --dir /graspit_home/<results-directory>``

This will place the results in ``<your-graspit-home>/tmp_results``.

For example for the Jaco arm sample files:

``grasp_planning --wld /graspit_home/worlds/jaco_robot_world.xml --dir /graspit_home/tmp_results/``


