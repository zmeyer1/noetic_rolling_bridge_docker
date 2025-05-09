# To Apply the Bridge to a turtlebot
I used a wafflebot here where noetic was already installed

Step 1: Power on the bot. You'll need a charger or else a charged LIPO battery, then flick the power switch -- you should see all the lights turn on and Lidar spin

Step 2: Find the IP of the turtlebot either by connecting locally (user/pass is usually ubuntu/turtlebot) and then running sudo ifconfig, or monitoring the network side.

Step 3: Open VS code, press Ctrl+Shift+X, and Download the VS Code 'Remote - SSH' Extension (see this page for details on the extension: https://code.visualstudio.com/docs/remote/ssh-tutorial)

Step 4: Download the VS Code 'Dev Containers' Extension (looks like this: https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers)

Step 5: Click on the remote explorer tab on the left menu bar, then click the small plus that appears next to 'SSH'

Step 6: For the SSH connection command, enter **ssh ubuntu@XX.XXX.XX.XX** replacing the Xs with the IP of the turtlebot, and select any config file

Step 7: Click the small arrow that appears next to the IP on the left menu bar, then enter your password when prompted. Press Yes to any other provided options.

Step 8: You are now connected to the remote machine! First thing to do is update all system packages. Run: **sudo apt update** within the remote environment

Step 9: clone the bridge repository: **git clone --recursive git@github.com:zmeyer1/noetic_rolling_bridge_docker.git** (this copies all the files you need from this repository onto the machine)

Step 10: Install docker and give your user permission to use docker: **sudo apt install docker.io && sudo usermod -aG docker $USER** You may have to reboot after this step, if step 14 throws an error.

Step 11: Open the folder that contains the bridge click File->Open Folder, then select noetic_rolling_brige_docker

Step 12: Click the green square with the >< icon in the bottom left corner of the window. Then select, Reopen in Container. This will start the docker container construction, it may take some time.

Step 13: After the docker container builds, you now need to build the ros bridge, run **build_ros_bridge** This may take a lot of time and throw some warnings that have the words PRAGMA_BOOST in them.

Step 14: After the bridge is built, its finally ready to be used! Open a new terminal on you desktop and run **ssh ubuntu@XX.XXX.XX.XX**, entering your password as needed.

Step 15: In the new window, run the ros1 turtlebot launch command: **roslaunch turtlebot3_bringup turtlebot3_robot.launch**

Step 15: Finally, within the docker container (in VS Code) run **. ~/run_ros_bridge.bash**


### To Test the Bridge
Publish a twist message that should make the robot move forwards. Be ready with a message to stop it before it gets too far!

ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.1, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}"

ros2 topic pub /cmd_vel geometry_msgs/msg/Twist "{linear: {x: 0.0, y: 0.0, z: 0.0}, angular: {x: 0.0, y: 0.0, z: 0.0}}"
