# Getting-Started
A step by step guide to getting MURsim to run on your Ubuntu.

# 0) Make sure you are running on an NVIDIA GPU and have enough space (at least 85GB) on Ubuntu
To partition your current Windows 10 drive to dual boot Ubuntu, this works pretty well https://www.youtube.com/watch?v=lI3ywh636IU

If you already have Ubuntu installed but don’t have enough space, please allocate/get more space.

If you're using dual boot with Windows and want to allocate more space:
1. Disk Management to shrink Windows Volume
   - If you’re facing issues while shrinking, https://www.winhelponline.com/blog/you-cannot-shrink-volume-beyond-point-disk-mgmt/?fbclid=IwAR2yXPd_RQhAVZplS2mzlSXEOtv-hrGCqICEZGgSrhtawZwSMEfaIvcGhUM
2. Use trial Ubuntu from Ubuntu boot iso to repartition extra space into your existing Ubuntu with GParted

## 1) Get Docker
1. Follow steps here: https://docs.docker.com/engine/install/ubuntu/
2. Test if it works running `sudo docker run hello-world`

## 2) Building MURauto Docker Image
1. Clone https://github.com/MURDriverless/MUR_Docker to a folder for Docker
2. Follow the instructions under Build and Usage. You'll need to Google search for the missing files.
3. After you do `sudo ./run.sh`, you should now be in docker already and get something like
```
randomalphanum
non-network local connections...
root@randomalphanum: /usr/local#
```

The randomalphanum at the top is the long UUID identifier for the docker container, while the short version is the one beside `root@`. You'll need the identifier if you would like to run multiple instance of docker, which will be on the 3).

4. Exit Docker for now with Ctrl+D or run `exit`

## 3) Easier way to run docker
1. Clone https://github.com/MURDriverless/mur_docker_launcher to the Docker folder in 2)
2. Follow instructions under Usage
3. You should now be able to launch docker with `sudo mdock`
4. To run multiple instances of the docker container, you'll need to run
```
sudo docker exec -it container_UUID bash
```

## 4) Getting MURsim on Docker
1. Make a folder that’s designated as the MUR workspace folder, for the purpose of this tutorial it'll be called MURworkspace
2. Make a src folder (/MURworkspace/src/), and then make a simulation folder in src (/MURworkspace/src/simulation)
   - src is the source folder, you’ll be putting packages you want to build there
   - simulation folder will hold all the mursim stuff, you can name it whatever you want
3. Get mur_init.sh from https://github.com/MURDriverless/mursim_init/tree/dev/Andrew_working/mur_init, put it in the simulation folder (/MURworkspace/src/simulation)
4. Run `sudo mdock` on the workspace folder you designated for MUR workspace (/MURworkspace/)
5. Run `cd /workspace` to get into the workspace folder on docker, it should show the path to /MURworkspace/
6. Run `cd src/simulation/` to get into the simulation folder
7. Run `sudo ./mur_init.sh`, you may run into two issues here
   - Command not found error
     - Run `chmod +x mur_init.sh` to give it executable permission
   - Permission denied (publickey) error when its cloning the required packages
     - Follow this and do the next steps until you add the SSH key to your github account https://docs.github.com/en/github/authenticating-to-github/checking-for-existing-ssh-keys
     - Its probably good to not put a passphrase for now, since it'll keep asking for your passphrase when you do the next step, you can always change it and add later https://docs.github.com/en/github/authenticating-to-github/working-with-ssh-key-passphrases
8. Once that is done, run “cd ../..” to get back to /MURworkspace/
9. Please run `catkin build` ONLY when you no longer need access to the laptop and can keep it powered for half an hour or more. It may hang, just leave it and let it do its thing. It may fail, doesn’t matter, just run “catkin build” again until it succeeds.

## 5) Run MURsim Slow Lap
1. Run `source devel/setup.bash` to source the setup file
2. Run `roslaunch mursim_gazebo slow_lap.launch`
3. Voila! 

# What's next?
ROS will be integral to everything we do, so you can start with the ROS tutorials if you've had no experience at all

ROS: http://wiki.ros.org/ROS/Tutorials/InstallingandConfiguringROSEnvironment, at the bottom you can go to the next tutorial.

After going through the ROS tutorials, you can test your understanding by going through the turtlesim tutorials http://wiki.ros.org/turtlesim/Tutorials/Moving%20in%20a%20Straight%20Line, perhaps with C++ since the Python codes are already available on the site. You must be able to at least complete the turtlesim tutorial before you can be certain you have a basic understanding on how ROS works and how nodes communicate with one another, so do give it a go!
