# Southern Arm Control Setup

This project contains files for the setup of the Southern Arm Control workspace and for the setup of the Ubutu and Raspian environments for the use of ROS.

## Build

To build the workspace for the first time use the build.sh script from the sac directory. After this run "catkin_make". The first build after opening the terminal ". devel/setup.bash" may need to be run if catkin_make does not work.

## Files

### build.sh
* This script can be used to build the workspace.
* This script is necessary for now as ROS does not build the sac_msgs package the first time before building the others.
* If you would like to delete this script after the first build it should be ok.

### gitPush.sh
* To push all of the packages in the workspace run this script (the comment entered will be used for all of the packages).
* This file will be moved to the sac workspace folder during workspace setup.

### gitPull.sh
* To pull all of the packages in the workspace run this script.
* This file will be moved to the sac workspace folder during workspace setup.

### setupPush.sh
* To push the setup project use run this script.
* The script will also copy the gitPush.sh and gitPull.sh files to the sac workspace.

### setupPull.sh
* To pull the setup project use run this script.
* The script will also copy the gitPush.sh and gitPull.sh files to the sac workspace.

### setup.sh
* Run this script to setup the programs, workspace, and packages for the Southern Arm Control workspace on Ubuntu.
* Much of the code for this script comes from http://wiki.ros.org/kinetic/Installation/Ubuntu.

### rpiSetup.sh
* Run this script to setup the programs, workspace, and packages for the Southern Arm Control workspace on Raspian.
* Much of the code for this script comes from http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Indigo%20on%20Raspberry%20Pi.
* The note at the top of the page that you can use the regular ros setup for the RPi 3 is wrong it does not work currently.

## Folders
### workspaceScripts
* This folder hold all of the scripts which will be moved to the base folder of the workspace during the setup.

### resources
* This folder holds several resource files which may be useful when editing the workspace or adding to it.

### templates
* This folder holds any style templates for use in the sac packages such as README.md files.

## Terms
These terms will apply to this and all other packages mention in the "packages" section below.

### SAC and sac
This is an abreviation for Southern Arm Control.

### Workspace
For this and all other Southern Arm Control packages, the term workspace will refer to all of the SAC packages as they exist in a sigle ROS workspace.

### Package
This refers to an individual ROS package within the SAC workspace or the sac_setup project. These are all noted in the "Packages" section below.

### RPi
This is an abreviation used for the Raspberry Pi.

## General Design Guidelines
1. The packages in the SAC workspace should be as disjoint as possible (they should not depend on another package to build), with exception to the sac_launch, which depends on everything, and the sac_msgs, which everything depends on, these packages were created to make the others more independant of eachother. The other package dependancy is from the gazebo package to the description package and this should be removed later if possible.
2. In keeping with the ROS methodology each node should do one task and nothing else, if another task is needed another node should be used for it.
3. The README.md files in each folder should contain all the information for that folder needed by a developer.
4. This workspace is ment to be used in overall robotics control education. In keeping with this the installation and use of the workspace should require as little knowlege of ROS and the nuts and bolts of the framework and system as possible to allow for a maximum amount of time spent working on algorithms and concepts.
5. This workspace should include only currently develped ROS components. This will allow the package to keep current with new releases of ROS on current versions of Ubuntu and Raspian.
6. Any code or examples of code used should be noted in the README.md in the description for the file the code was used in.
7. All README.md files should follow the same format as shown in the README_mainTemplate.md and README_subFolderTemplate.md. Each section of the template should be included only if it is applicable to the package or folder. This is to make the package more uniform and to simplify documentation.
8. General launch senarios should be launched from the sac_launch package. The launch files in this package should only have includes for launch files in other packages. The other packages' launch files should only include launch files from the immediate package.

## Installation
### Raspberry Pi
1. Obtain a Raspberry Pi (RPi) 3 (the workspace should also work on an RPi 2).
2. Obtain an 8Gb class 10 micro SD card for the pi (lower class levels will work but will be much slower)
3. Install either Raspian Jessie with Pixel or Raspian Jessie Lite on the SD card using win32DiskImager if on windows.
4. Setup Raspian as you like on the RPi installing any programs you would like to have (run "sudo raspi-config" to setup any RPi features of the pi itself, don't forget to change the keyboard layout to English (US), you will need to reboot for this to take effect).
5. Install git.
6. Clone this project into the home directory of the RPi.
7. Run rpiSetup.sh, this will install ros, the sac workspace, and all other items for the workspace.
8. The sac_setup project can now be removed from the RPi if desired.
9. Follow the instructions in the sac workspace for running, modifying, and using the packages.

### Ubuntu 16.04
1. Install Ubuntu 16.04 on either a computer or a virtual machine.
2. Setup Ubuntu as you like installing any programs you would like to have.
3. Install git.
4. Clone this project into the home directory of Ubuntu
5. Run setup.sh, this will install ros, the sac workspace, and all other items for the workspace.
6. The sac_setup project can now be removed from Ubuntu if desired.
7. Follow the instructions in the sac workspace for running, modifying, and using the packages.

## packages
### sac_setup
This is not a ros package, but a project to setup ros and all of its components, along with the Southern Arm Control workspace. Start here to use the SAC system.

### sac_controllers
This package contains all of the overall controllers for the workspace to perform top level actions with the arm.

### sac_translators
This package contains translator nodes to translate from the global inputs of the controllers to the joint space inputs of the motors.

### sac_drivers
This package contains the nodes to communicate with the hardware and low level components of the robot.

### sac_gazebo
This package contains all of the components for using and running gazebo.

### sac_description
This package contains the descriptions for the robots used.

### sac_msgs
This package contains the custom message, service, and action files used in the rest of the workspace all other packages will depend on this package.

### sac_launch
This package contains all of the global launch files used to launch files from the other nodes.

### sac_config (not in use)
This package was generated by teh MoveIt! Setup Assistant for the Scorbot and Andreas arms.

### scorbot_config (not in use)
This package was generated by the MoveIt! Setup Assistant for the Scorbot arm.

### andreas_arm_config (not in use)
This package was generated by the MoveIt! Setup Assistant for the Andreas arm.

## General SAC workspace notes
* When generating messages and services create the service, build it, and make sure the *.h files have been generated before #including the files in any source files. This is because the service headers are generated last in the cmake process. This can be done by adding the package the message is in to the dependancies.
* When running the gitPush.sh on "Bash on Ubuntu on Windows" "cache" in the first line might need to be changed to "no-cache".
* When including a message from another package the package the message is in needs to be included in the find_packages of the CMakeLists.txt of the package using the remote message.
* Joints in the urdf/xacro must all be in different positions if a joints x, y, and z are all 0 the joint will not be able to be controlled.
* If the Gazebo simulator crashes when starting the launch file may need to be run again this is aparently an issue with Gazebo 8.
* If changes are made to the robot description and the robot starts to go crazy moving this is because gazebo is very touchy on xacro parameters.
* When running gazebo if it is switched to a wireframe in the view menu it will run much smoother.
* After installing the system if it will not build or run and it says scripts are missing check if all of the programs in the setup script installed correctly.
