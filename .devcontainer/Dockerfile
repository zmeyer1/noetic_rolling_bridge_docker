FROM ubuntu:24.04

# Add ubuntu user with same UID and GID as your host system, if it doesn't already exist
# Since Ubuntu 24.04, a non-root user is created by default with the name vscode and UID=1000
ARG USERNAME=ubuntu
ARG USER_UID=1000
ARG USER_GID=$USER_UID
RUN if ! id -u $USER_UID >/dev/null 2>&1; then \
        groupadd --gid $USER_GID $USERNAME && \
        useradd -s /bin/bash --uid $USER_UID --gid $USER_GID -m $USERNAME; \
    fi
# Add sudo support for the non-root user
RUN apt-get update && \
    apt-get install -y sudo && \
    echo "$USERNAME ALL=(root) NOPASSWD:ALL" > /etc/sudoers.d/$USERNAME && \
    chmod 0440 /etc/sudoers.d/$USERNAME

# Install ROS1 Noetic
RUN apt update
RUN apt install -y software-properties-common curl
RUN add-apt-repository -y ppa:ros-for-jammy/noble
RUN add-apt-repository -y universe
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN apt update
RUN apt install -y ros-noetic-desktop-full

# Install ROS2 Rolling
RUN curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt update
RUN apt install -y ros-rolling-desktop-full

# Switch from root to user
USER $USERNAME

# Add user to video group to allow access to webcam
RUN sudo usermod --append --groups video $USERNAME

# Update all packages
RUN sudo apt update && sudo apt upgrade -y

RUN sudo apt install -y build-essential

# Install Git
RUN sudo apt install -y git

# Install VIM
RUN sudo apt install -y vim 

# Install Colcon Build tools
RUN sudo apt install -y python3-colcon-common-extensions

# Install Cyclone DDS Implementation
RUN sudo apt install -y ros-rolling-rmw-cyclonedds-cpp


################################
## ADD ANY CUSTOM SETUP BELOW ##
################################

RUN echo "alias sourceros2='source /opt/ros/rolling/setup.bash'" >> ~/.bashrc
RUN echo "alias sourceros1='source /opt/ros/noetic/setup.bash'" >> ~/.bashrc
RUN echo "export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp" >> ~/.bashrc
RUN echo "alias start_dds='fastdds discovery --server-id 0 -p 11888'" >> ~/.bashrc


RUN echo "alias build_ros_bridge='colcon build --symlink-install --packages-select ros1_bridge --cmake-force-configure'" >> ~/.bashrc
RUN echo "alias ros_bridge='sourceros1 && sourceros2 && ros2 run ros1_bridge dynamic_bridge --bridge-all-1to2-topics'" >> ~/.bashrc

RUN echo "source /bridge_ws/install/setup.bash" >> ~/.bashrc

RUN echo "#!/usr/bin/bash" >> ~/run_ros_bridge.bash
RUN echo "while true; do ros_bridge; done" >> ~/run_ros_bridge.bash






