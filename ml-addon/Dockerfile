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
  jupyter labextension install @jupyter-widgets/jupyterlab-manager@^0.38.1 && \
  jupyter labextension install jupyterlab_bokeh@0.6.3
  
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    cd && \
    rm -rf /tmp/facets
