FROM nvidia/cuda:10.0-cudnn7-runtime-ubuntu16.04
# ------------------------------------------ Install dependencies ---------------------------------------------------------
RUN apt-get update && apt-get install -q -y \
    dirmngr \
    gnupg2 \
    lsb-release \
    python3-pip python-pip \
    wget ca-certificates \
    emacs git inkscape jed libsm6 libxext-dev \
    libxrender1 lmodern netcat pandoc python-dev texlive-fonts-extra \
    texlive-fonts-recommended texlive-generic-recommended \
    texlive-latex-base texlive-latex-extra texlive-xetex \
    libgoogle-glog-dev libgtest-dev libiomp-dev libleveldb-dev \
    liblmdb-dev libopencv-dev libopenmpi-dev libsnappy-dev libprotobuf-dev \
    openmpi-bin openmpi-doc protobuf-compiler libgflags-dev \
    unzip nano ssh libjansson-dev npm libboost-dev imagemagick libtinyxml-dev mercurial cmake build-essential curl \
    && rm -rf /var/lib/apt/lists/*
    
# ------------------------------------------------- Install ROS ----------------------------------------------

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
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

# install ROS packages: only ros-base for communication, other tools are installed manually
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
    xvfb=2:1.18.4-0ubuntu0.7 \
	x11-apps=7.7+5+nmu1ubuntu1 \
	netpbm=2:10.0-15.3\
    && rm -rf /var/lib/apt/lists/

#Install ROS applications
RUN pip install catkin_tools rospkg
RUN pip3 install catkin_tools rospkg
RUN apt-get install -y ros-kinetic-moveit \
	ros-kinetic-teleop-twist-keyboard \
	ros-kinetic-map-server \
	ros-kinetic-fake-localization \
	ros-kinetic-ros-control ros-kinetic-ros-controllers \
    	python-rospkg ros-kinetic-teb-local-planner ros-kinetic-ackermann-msgs \
    	ros-kinetic-effort-controllers ros-kinetic-joy \
    	ros-kinetic-tf2-sensor-msgs python-rosinstall ros-kinetic-geometry-tutorials ros-kinetic-rosbash \
	ros-kinetic-rqt-tf-tree ros-kinetic-cv-bridge ros-kinetic-image-geometry \
	ros-kinetic-rosserial-arduino ros-kinetic-rosserial-embeddedlinux ros-kinetic-rosserial-windows \
	ros-kinetic-rosserial-server ros-kinetic-rosserial-python \
	ros-kinetic-openni-camera ros-kinetic-joystick-drivers ros-kinetic-navigation ros-kinetic-industrial-core
	
# ------------------------------------ Install Ml applications dependencies --------------------------------------------
RUN apt-get install -y 
RUN curl -sL https://deb.nodesource.com/setup_8.x | bash - && apt-get install -y nodejs

# ------------------------------------- Install Jupyter Notebook and Lab -----------------------------------------------
# --------------------------------------- for Python 3 and Pyhton 2 ----------------------------------------------------
RUN pip2 install --upgrade pip==18.0
RUN pip2 install \
  notebook==5.6.0 \
  ipywidgets==7.3.0 \
  ipykernel==4.8.2 \
  matplotlib==2.2.2 \
  jupyterlab==0.33.4

RUN python3 -m pip --version && python3 -m pip install --upgrade pip 
RUN pip3 install \
  notebook \
  ipywidgets\
  ipykernel \
  jupyterlab

# --------------------------------- Install Python libs for machine Learning tool-chain --------------------------------
# --------------------------------------- for Python 3 and Pyhton 2 ----------------------------------------------------
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
    future \
    typing \
    hypothesis
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

# -------------------------------------- Install Deep Learning libraries ------------------------------------------------
# --------------------------------------- for Python 3 and Pyhton 2 -----------------------------------------------------

RUN pip2 install setuptools enum34 empy tensorflow keras pyspark nltk
RUN pip2 install elephas gym keras-rl opencv-python torchvision

RUN pip3 install setuptools enum34 empy tensorflow keras pyspark nltk
RUN pip3 install elephas gym keras-rl opencv-python torchvision

RUN pip3 install tensorflow-gpu
RUN pip2 install tensorflow-gpu

RUN pip install https://download.pytorch.org/whl/cu100/torch-1.1.0-cp27-cp27mu-linux_x86_64.whl
RUN pip install https://download.pytorch.org/whl/cu100/torchvision-0.3.0-cp27-cp27mu-linux_x86_64.whl

RUN pip3 install https://download.pytorch.org/whl/cu100/torch-1.1.0-cp36-cp36m-win_amd64.whl
RUN pip3 install https://download.pytorch.org/whl/cu100/torchvision-0.3.0-cp36-cp36m-win_amd64.whl

RUN pip3 install trackpy

RUN pip2 install --upgrade azureml-sdk[notebooks,explain,automl]
RUN pip3 install --upgrade azureml-sdk[notebooks,explain,automl]

RUN pip2 install azure-cognitiveservices-vision-computervision
RUN pip3 install azure-cognitiveservices-vision-computervision

# ------------------------------- Add ROS example application and openai_ros package ------------------------------------------
USER root
ENV APP_ROOT=/opt/app-root
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT}

COPY . ${APP_ROOT}/bin/
# Install frugally_deep
RUN cd ${APP_ROOT}/bin/ && \
	git clone https://github.com/Dobiasd/FunctionalPlus && \
	cd FunctionalPlus && \
	mkdir -p build && cd build && \
	cmake .. && \
	make && sudo make install && \
	cd ../..
RUN cd ${APP_ROOT}/bin/ && \
	hg clone https://bitbucket.org/eigen/eigen/  && \
	cd eigen  && \
	mkdir -p build && cd build  && \
	cmake ..  && \
	make && sudo make install  && \
	ln -s /usr/local/include/eigen3/Eigen /usr/local/include/Eigen  && \
	cd ../..
RUN cd ${APP_ROOT}/bin/ && \
	git clone https://github.com/nlohmann/json  && \
	cd json  && \
	git checkout v3.1.2  && \
	mkdir -p build && cd build  && \
	cmake ..  && \
	make && sudo make install  && \
	cd ../..
RUN cd ${APP_ROOT}/bin/ && \
	git clone https://github.com/Dobiasd/frugally-deep && \
	cd frugally-deep && \
	mkdir -p build && cd build  && \
	cmake .. && \
	make && sudo make install && \
	cd ../..
RUN cd ${APP_ROOT}/bin/ && mkdir share
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/erlerobot/gym-gazebo.git && cd gym-gazebo && pip install -e .
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/pytorch/pytorch.git && cd pytorch && \
	git submodule update --init --recursive && python setup.py install
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/RoboStack/jupyter-ros.git
RUN cd ${APP_ROOT}/bin/ && mkdir ros_ws && cd ros_ws && mkdir src && cd src && \
    git clone https://bitbucket.org/theconstructcore/openai_ros.git
RUN rosdep update
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; cd  ~/bin/ros_ws; catkin_make; source devel/setup.bash; rosdep install openai_ros'

# ----------------------------------- Change User permissions for use in Openshift -------------------------------------------
RUN chmod -R u+x ${APP_ROOT}/bin && \
    chgrp -R 0 ${APP_ROOT} && \
    chmod -R g=u ${APP_ROOT} /etc/passwd
    
RUN /bin/bash -c '/opt/app-root/bin/entrypoint.sh'
USER 10001
WORKDIR ${APP_ROOT}
ENTRYPOINT [ "uid_entrypoint" ]
EXPOSE 8888
CMD ["jupyter", "lab","--NotebookApp.token=''", "--no-browser", "--ip", "0.0.0.0"]
