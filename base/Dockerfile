FROM ros:kinetic-ros-base

# install ros tutorials packages
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-tutorials \
    ros-kinetic-common-tutorials \
    python-pip \
    xvfb=2:1.18.4-0ubuntu0.7 \
	x11-apps=7.7+5+nmu1ubuntu1 \
	netpbm=2:10.0-15.3\
    && rm -rf /var/lib/apt/lists/

RUN pip install --upgrade pip==18.0
RUN pip install \
  notebook==5.6.0 \
  ipywidgets==7.3.0 \
  ipykernel==4.8.2 \
  matplotlib==2.2.2 \
  jupyterlab==0.33.4

RUN apt-get update && apt-get install -y python3-pip python-pip
RUN python3 -m pip --version && python3 -m pip install --upgrade pip && python3 -m pip install ipykernel &&\
    python3 -m ipykernel install --user

#Install necessary system applicaion
RUN apt-get install -y\
    wget ca-certificates \
    emacs git inkscape jed libsm6 libxext-dev \
    libxrender1 lmodern netcat pandoc python-dev texlive-fonts-extra \
    texlive-fonts-recommended texlive-generic-recommended \
    texlive-latex-base texlive-latex-extra texlive-xetex \
    unzip nano

#Install ROS applications
RUN pip install catkin_tools rospkg
RUN pip3 install catkin_tools rospkg
RUN apt-get install -y\
    ros-kinetic-ros-control ros-kinetic-ros-controllers \
    ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control \
    python-rospkg ros-kinetic-teb-local-planner ros-kinetic-ackermann-msgs \
    ros-kinetic-effort-controllers ros-kinetic-joy \
    ros-kinetic-tf2-sensor-msgs python-rosinstall

USER root
ENV APP_ROOT=/opt/app-root
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT}

#Add ROS example application and openai_ros package
RUN cd ${APP_ROOT}/bin/ && mkdir ros_ws && cd ros_ws && mkdir src && cd src && \
    git clone https://bitbucket.org/theconstructcore/openai_ros.git &&\
    git clone https://github.com/mit-racecar/racecar.git &&\
    git clone https://github.com/mit-racecar/racecar-simulator.git
RUN rosdep update
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; cd  ~/ros_ws; catkin_make; source devel/setup.bash; rosdep install openai_ros'

COPY . ${APP_ROOT}/bin/
RUN chmod -R u+x ${APP_ROOT}/bin && \
    chgrp -R 0 ${APP_ROOT} && \
    chmod -R g=u ${APP_ROOT} /etc/passwd
USER 10001
WORKDIR ${APP_ROOT}

EXPOSE 8888
CMD ["jupyter", "lab", "--no-browser", "--ip", "0.0.0.0"]
