FROM decinho13/ros-notebook:base
USER root
#Install python 3 kernel
RUN apt-get update && apt-get install sudo
RUN sudo apt-get update && sudo apt-get install -y python3-pip python-pip
RUN python3 -m pip --version && python3 -m pip install --upgrade pip && python3 -m pip install ipykernel &&\
    python3 -m ipykernel install --user

#Install necessary system applicaion
RUN sudo apt-get install -y\
    wget ca-certificates \
    emacs git inkscape jed libsm6 libxext-dev \
    libxrender1 lmodern netcat pandoc python-dev texlive-fonts-extra \
    texlive-fonts-recommended texlive-generic-recommended \
    texlive-latex-base texlive-latex-extra texlive-xetex \
    unzip nano

#Install ROS applications
RUN pip install catkin_tools rospkg
RUN pip3 install catkin_tools rospkg
RUN sudo apt-get install -y\
    ros-kinetic-ros-control ros-kinetic-ros-controllers \
    ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control \
    python-rospkg ros-kinetic-teb-local-planner ros-kinetic-ackermann-msgs \
    ros-kinetic-effort-controllers ros-kinetic-joy \\
    ros-kinetic-tf2-sensor-msgs python-rosinstall ros-kinetic-ros-tutorials \
    ros-kinetic-common-tutorials
    
#Add ROS example application and openai_ros package
WORKDIR ${APP_ROOT}
RUN mkdir ros_ws && cd ros_ws && mkdir src && cd src && \
    git clone https://bitbucket.org/theconstructcore/openai_ros.git &&\
    git clone https://github.com/mit-racecar/racecar.git &&\
    git clone https://github.com/mit-racecar/racecar-simulator.git
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; cd  ~/ros_ws; catkin_make; source devel/setup.bash; rosdep install openai_ros'

USER 10001
WORKDIR ${APP_ROOT}
