# noetic_rolling_bridge_docker
A bridge between ROS2 Rolling and ROS1 Noetic. 

Automatically publishes all topics on ros1 to ros2, but won't create additional topics to ros1 unless there is a subscriber listening for it.

## Requirements
Requires the docker.io library (apt install docker.io)

If you wish to use VISUAL STUDIO CODE for your development, requires the dev containers module.
    
See this tutorial: https://code.visualstudio.com/docs/devcontainers/containers

## To clone the repository
git clone --recursive https://github.com/zmeyer1/noetic_rolling_bridge_docker.git

## To build the docker container
If you are using dev containers through vscode, then you should just be able to 'Reopen in Container' in the current folder

If you are using docker manually, run the setup_docker.bash script (still in progress)

## To build the bridge
In a terminal inside the docker container, run 'build_ros_bridge', the first time this will take 15+ minutes

## To run the bridge
In a terminal on your ros 1 system, run roscore or your launch file

In a terminal inside the docker container, run '. ~/run_ros_bridge.bash' and the bridge should come up

It will throw some debug errors about the Log topic, ignore them, they're expected.
