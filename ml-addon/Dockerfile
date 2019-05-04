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

RUN pip2 install --upgrade pip==18.0
RUN pip2 install \
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
    unzip nano ssh

RUN pip3 install \
  notebook==5.6.0 \
  jupyterlab==0.33.4

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

RUN apt-get install -y ros-kinetic-rosbridge-server

# nvidia-container-runtime
ENV NVIDIA_VISIBLE_DEVICES all
ENV NVIDIA_DRIVER_CAPABILITIES compute,utility
ENV NVIDIA_REQUIRE_CUDA "cuda>=10.0 brand=tesla,driver>=384,driver<385"

# Add NVIDIA package repositories
# Add HTTPS support for apt-key
RUN apt-get install -y gnupg-curl
RUN wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_10.0.130-1_amd64.deb && dpkg -i cuda-repo-ubuntu1604_10.0.130-1_amd64.deb
RUN apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/7fa2af80.pub
RUN apt-get update && wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1604/x86_64/nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
RUN apt install -y ./nvidia-machine-learning-repo-ubuntu1604_1.0.0-1_amd64.deb
RUN apt-get update && mkdir /usr/lib/nvidia 

# Install development and runtime libraries (~4GB)
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    cuda-10-0 \
    libcudnn7=7.4.1.5-1+cuda10.0  \
    libcudnn7-dev=7.4.1.5-1+cuda10.0


# Install TensorRT. Requires that libcudnn7 is installed above.
RUN apt-get update && \
        DEBIAN_FRONTEND=noninteractive apt-get install -y nvinfer-runtime-trt-repo-ubuntu1604-5.0.2-ga-cuda10.0 \
        && apt-get update \
        && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends libnvinfer-dev=5.0.2-1+cuda10.0

        
RUN pip3 install tensorflow-gpu
RUN pip3 install https://download.pytorch.org/whl/cu100/torch-1.0.1.post2-cp35-cp35m-linux_x86_64.whl && pip install torchvision


USER root
ENV APP_ROOT=/opt/app-root
ENV PATH=${APP_ROOT}/bin:${PATH} HOME=${APP_ROOT}

#Add ROS example application and openai_ros package
COPY . ${APP_ROOT}/bin/
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/erlerobot/gym-gazebo.git && cd gym-gazebo && pip install -e .
RUN cd ${APP_ROOT}/bin/ && git clone https://github.com/RoboStack/jupyter-ros.git
RUN cd ${APP_ROOT}/bin/ && mkdir ros_ws && cd ros_ws && mkdir src && cd src && \
    git clone https://bitbucket.org/theconstructcore/openai_ros.git &&\
    git clone https://github.com/mit-racecar/racecar.git &&\
    git clone https://github.com/mit-racecar/racecar-simulator.git
RUN rosdep update
RUN /bin/bash -c '. /opt/ros/kinetic/setup.bash; cd  ~/bin/ros_ws; catkin_make; source devel/setup.bash; rosdep install openai_ros'


RUN chmod -R u+x ${APP_ROOT}/bin && \
    chgrp -R 0 ${APP_ROOT} && \
    chmod -R g=u ${APP_ROOT} /etc/passwd
USER 10001
WORKDIR ${APP_ROOT}

EXPOSE 8888
CMD ["jupyter", "lab","--NotebookApp.token=''", "--no-browser", "--ip", "0.0.0.0"]
