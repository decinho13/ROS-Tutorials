FROM decinho13/ros-notebook:robotics-hl
USER root

RUN apt-get install -y ros-kinetic-moveit ffmpeg

RUN pip -m install \
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
  dill
  numba \
  bokeh \
  sqlalchemy \
  hdf5 \
  vincent \
  beautifulsoup4 \
  protobuf \
  xlrd &&\
  jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
  jupyter labextension install @jupyter-widgets/jupyterlab-manager && \
  jupyter labextension install jupyterlab_bokeh

  RUN pip3 -m install \
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
    dill
    numba \
    bokeh \
    sqlalchemy \
    hdf5 \
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

#Machine Learning libraries
RUN pip install setuptools enum34 empy tensorflow keras theano pyspark nltk
RUN pip install elephas gym keras-rl opencv-python torchvision

RUN pip3 install setuptools enum34 empy tensorflow keras theano pyspark nltk
RUN pip3 install elephas gym keras-rl opencv-python torchvision

#jupyros libraries
RUN pip install bqplot pyyaml empy
RUN pip install jupyros && jupyter nbextension enable --py --sys-prefix jupyros && jupyter labextension install jupyter-ros

RUN pip3 install bqplot pyyaml empy
RUN pip3 install jupyros && jupyter nbextension enable --py --sys-prefix jupyros && jupyter labextension install jupyter-ros

USER 10001
WORKDIR ${APP_ROOT}
