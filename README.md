

This repository contains the gazebo model and MoveIt! plugins for the Motoman HC10-DT with a Velodyne VLP-16 lidar as an end-effector for 3D mapping. 

#### Commands

**Gazebo** `roslaunch motoman_hc10_gazebo hc10dt_gazebo.launch`

**MoveIt!** `roslaunch motoman_hc10dt_moveit_config moveit_planning_execution.launch`

**python** `rosrun move_hc0dt trial.py`



#### Dependencies

Ensure that the following package dependencies are installed in your system:

- ros-kinetic-ros-control
- ros-kinetic-ros-controllers
- ros-kinetic-controller-interface
- ros-kinetic-controller-manager
- ros-kinetic-controller-manager-msgs
- ros-kinetic-gazebo-ros
- ros-kinetic-gazebo-ros-control
- ros-kinetic-joint-state-controller
- ros-kinetic-joint-trajectory-controller
- ros-kinetic-effort-controllers
- ros-kinetic-velocity-controllers



#### Known Errors

```
[ERROR] [1589821347.035206609, 0.367000000]: No p gain specified for pid.  Namespace: /gazebo_ros_control/pid_gains/joint_1_s
```

This is not really an error. The PID parameters are optional if you are using gazebo_ros_control. The error comes because when initializing RobotHWSim, the robot_ros_control tries to load the pid_gains. Basically, if pid parameters were found, gazebo_ros_control will use pid controllers in ROS to control the joints (velocity or position) by effort. Otherwise, the joints will be controlled with gazebo methods.

https://answers.ros.org/question/293830/what-is-the-fix-for-no-p-gain-specified-for-pid-namespace-gazebo_ros_controlpid_gainsback_right_wheel_joint-ros-melodic/


#### Git Workflow

##### Forking

Create a fork of `armlab-clemson/nsf-hc10dt` in your own account. Clone this fork into your computer.


	$ mkdir ~/git && cd ~/git
	$ git clone --recurse-submodules https://github.com/USER/nsf-hc10dt.git
	$ cd nsf-hc10dt


Notice that the option `--recurse-submodules` is passed to `git-clone`. This automatically initializes and updates any included submodules.

The `ros-stacks` directory may contains external repositories as Git submodules. One example is the `motoman_experimental` repository. If the directory is empty (perhaps you forgot `--recurse-submodules` when cloning), you'll have to perform the following to get the code for each submodule.

	~/git/arm-scr$ cd ros-stacks/motoman_experimental
	$ git submodule init
	$ git submodule update 

##### Adding remotes

Next, set up additional **remotes** for this repository. The one you cloned from, your fork,  will be named `origin` by default. Then, we'll refer to the mainline respository as the `upstream` repository.

	~/git/nsf-hc10dt$ git remote -v # inspect remotes
	origin	https://github.com/USER/nsf-hc10dt.git (fetch)
	origin	https://github.com/USER/nsf-hc10dt.git (push)
	$ git remote add upstream https://github.com/armlab-clemson/nsf-hc10dt.git
	$ git remote -v
	origin	https://github.com/USER/nsf-hc10dt.git (fetch)
	origin	https://github.com/USER/nsf-hc10dt.git (push)
	upstream	https://github.com/armlab-clemson/nsf-hc10dt.git (fetch)
	upstream	https://github.com/armlab-clemson/nsf-hc10dt.git (push)

##### Branches

Next you should look at your **branches** and create a new one for your next development topic so you can start making changes safely. Suppose you're working on a fix to the colors used in the OTTO1500 model.

    ~/git/nsf-hc10dt$ git branch # inspect branches, master is current
    * master
    
    $ git branch dev-1.1 #create branch fix-otto-colors
    $ git checkout dev-1.1 # check out branch fix-otto-colors
    $ git branch # inspect branches, fix-otto-colors is current
    * dev-1.1
      master

Now go make some changes to fix the colors, test them, **add** them, and **commit** them. Assuming that you only changed the XACRO macro for OTTO:

    ~/git/nsf-hc10dt$ git add ros-stacks/clearpath_otto1500_support/urdf/otto1500_macro.xacro
    $ git commit -m 'fix otto colors'
    $ git log # inspect the commit history for the current branch
    commit xxxxxxx...
    Author: User Name <user@clemson.edu>
    Date:   ...
    
    fix otto colors

Repeat this edit/test/commit cycle until you're ready to share your changes. Then **push** your topic branch to your fork remember that the **remote** alias for your fork is `origin`.

    ~/git/nsf-hc10dt$ git push origin dev-1.1

Finally, after making all the changes, merge the branch with the master. 


    ~/git/nsf-hc10dt$ git checkout master
    ~/git/nsf-hc10dt$ git merge dev-1.1

In the browser GUI, you will see a new pull request to merge the branches. Click on the pull request and accept it. Resolve the conflicts - if there are any - and merge the branches with comments if needed. 

Now delete the branch after merging. 

    ~/git/nsf-hc10dt$ git branch -d dev-1.1


##### Pull Requests


To pull your changes into the `armlab-clemson/nsf-hc10dt` repository, submit a pull request to the admin. Once you push the changes to your repo, `Compare` & `pull request` buttons will appear in GitHub. Select compare and after resolving conflicting changes, submit the pull request.


#### Git Workspace Configuration

```
nsf_hc10dt/
    ros-stacks/
	motoman/
        industrial_core/
```

#### ROS Workspace Configuration

```
ros_ws/
    build/
    devel/
    src/
	<symbolic links from ros-stacks>
```

##### Adding packages to the src directory in this workspace

We'll do this by going into src and creating symbolic links to the desired ROS packages or stacks that we've cloned from Git repositories.

```
~/ros_ws$ cd src

~/ros_ws/src$ ln -s ~/nsf-robot-project/nsf-hc10dt/ros-stacks/motoman
```

Then go up a level and build the workspace again to compile the package(s).

```
~/ros_ws/src$ cd ..
$ catkin_make
```

As you checkout different branches your symbolic links may become invalid. This is okay. Just build the workspace with bad links and those targets will be skipped.


####
Developers:
* Mark T
* Adhiti R
