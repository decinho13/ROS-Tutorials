FROM nvidia/cuda:10.0-cudnn7-runtime-ubuntu16.04
# install packages
RUN apt-get update && apt-get install -q -y \
    dirmngr \
    gnupg2 \
    lsb-release \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    python-rosdep \
    python-rosinstall \
    python-vcstools \
    && rm -rf /var/lib/apt/lists/*

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# bootstrap rosdep
RUN rosdep init \
    && rosdep update

# install ros packages
ENV ROS_DISTRO kinetic
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-core=1.3.2-0* \
    && rm -rf /var/lib/apt/lists/*
    
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-base=1.3.2-0* \
    && rm -rf /var/lib/apt/lists/*

# install ros tutorials packages
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-tutorials \
    ros-kinetic-common-tutorials \
    python-pip \
    xvfb=2:1.18.4-0ubuntu0.7 \
	x11-apps=7.7+5+nmu1ubuntu1 \
	netpbm=2:10.0-15.3\
    && rm -rf /var/lib/apt/lists/

RUN pip2 install --upgrade pip==18.0
RUN pip2 install \
  notebook==5.6.0 \
  ipywidgets==7.3.0 \
  ipykernel==4.8.2 \
  matplotlib==2.2.2 \
  jupyterlab==0.33.4

RUN apt-get update && apt-get install -y python3-pip python-pip
RUN python3 -m pip --version && python3 -m pip install --upgrade pip 
RUN pip3 install \
  notebook \
  ipywidgets\
  ipykernel \
  jupyterlab

#Install necessary system applicaion
RUN apt-get install -y\
    wget ca-certificates \
    emacs git inkscape jed libsm6 libxext-dev \
    libxrender1 lmodern netcat pandoc python-dev texlive-fonts-extra \
    texlive-fonts-recommended texlive-generic-recommended \
    texlive-latex-base texlive-latex-extra texlive-xetex \
    unzip nano ssh

#Install ROS applications
RUN pip install catkin_tools rospkg
RUN pip3 install catkin_tools rospkg
RUN apt-get install -y\
    ros-kinetic-ros-control ros-kinetic-ros-controllers \
    ros-kinetic-gazebo-ros-pkgs ros-kinetic-gazebo-ros-control \
    python-rospkg ros-kinetic-teb-local-planner ros-kinetic-ackermann-msgs \
    ros-kinetic-effort-controllers ros-kinetic-joy \
    ros-kinetic-tf2-sensor-msgs python-rosinstall   
RUN apt-get install -y ros-kinetic-moveit ffmpeg

#Install Ml applications
RUN apt-get install -y libjansson-dev npm libboost-dev imagemagick libtinyxml-dev mercurial cmake build-essential curl
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && apt-get install -y nodejs

RUN pip3 install \
    ipywidgets \
    pandas \
    numexpr \
    matplotlib \
    scipy \
    seaborn \
    scikit-learn \
    scikit-image \
    sympy \
    cython \
    patsy \
    statsmodels \
    cloudpickle \
    dill \
    numba \
    bokeh \
    sqlalchemy \
    h5py \
    vincent \
    beautifulsoup4 \
    protobuf \
    xlrd &&\
    jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
    jupyter labextension install jupyterlab_bokeh


RUN pip2 install \
  ipywidgets \
  pandas \
  numexpr \
  matplotlib \
  scipy \
  seaborn \
  scikit-learn \
  scikit-image \
  sympy \
  cython \
  patsy \
  statsmodels \
  cloudpickle \
  dill \
  numba \
  bokeh \
  sqlalchemy \
  h5py \
  vincent \
  beautifulsoup4 \
  protobuf \
  xlrd &&\
  jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
  jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
  jupyter labextension install jupyterlab_bokeh


RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/root/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot"

#Deep Learning libraries
RUN pip2 install setuptools enum34 empy tensorflow keras theano pyspark nltk
RUN pip2 install elephas gym keras-rl opencv-python torchvision

RUN pip3 install setuptools enum34 empy tensorflow keras theano pyspark nltk
RUN pip3 install elephas gym keras-rl opencv-python torchvision
RUN apt-get install python-rosgraph
#jupyros libraries
RUN pip3 install bqplot pyyaml empy
RUN pip3 install jupyros && jupyter nbextension enable --py --sys-prefix jupyros && jupyter labextension install jupyter-ros && jupyter nbextension enable --py --sys-prefix widgetsnbextension

RUN pip2 install bqplot pyyaml empy
RUN pip2 install jupyros && jupyter nbextension enable --py --sys-prefix jupyros && jupyter labextension install jupyter-ros && jupyter nbextension enable --py --sys-prefix widgetsnbextension

RUN apt-get install -y ros-kinetic-rosbridge-server ros-kinetic-cv-bridge



RUN pip3 install tensorflow-gpu
RUN pip2 install tensorflow-gpu
RUN pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.1.post2-cp35-cp35m-linux_x86_64.whl && pip install torchvision

RUN pip3 install trackpy

USER root
ENV APP_ROOT=/opt/app-root
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT}

#Add ROS example application and openai_ros package
COPY . ${APP_ROOT}/bin/
RUN cd ${APP_ROOT}/bin/ && mkdir share
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/erlerobot/gym-gazebo.git && cd gym-gazebo && pip install -e .
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/RoboStack/jupyter-ros.git
RUN cd ${APP_ROOT}/bin/ && mkdir ros_ws && cd ros_ws && mkdir src && cd src && \
    git clone https://bitbucket.org/theconstructcore/openai_ros.git &&\
    git clone https://github.com/mit-racecar/racecar.git &&\
    git clone https://github.com/mit-racecar/racecar-simulator.git
RUN rosdep update
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; cd  ~/bin/ros_ws; catkin_make; source devel/setup.bash; rosdep install openai_ros'

RUN /bin/bash -c '/opt/app-root/bin/entrypoint.sh'
RUN chmod -R u+x ${APP_ROOT}/bin && \
    chgrp -R 0 ${APP_ROOT} && \
    chmod -R g=u ${APP_ROOT} /etc/passwd
USER 10001
WORKDIR ${APP_ROOT}



#CMD ["sh","./bin/entrypoint.sh"]
ENTRYPOINT [ "uid_entrypoint" ]
EXPOSE 8888
CMD ["jupyter", "lab","--NotebookApp.token=''", "--no-browser", "--ip", "0.0.0.0"]
