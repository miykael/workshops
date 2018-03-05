# This is the Dockerfile to create the docker image for the workshop in Marburg
#
# For the python and nipype part of the workshop use the following command:
#    docker run -it --rm -p 8888:8888 miykael/workshop_marburg

FROM djarecka/nipype_tutorial

ARG DEBIAN_FRONTEND=noninteractive

USER root

# Install Swig for PyMVPA
RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends swig \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER neuro

#---------------------------------
# Update conda environment 'neuro'
#---------------------------------

# Make sure that jupyter notebook can chose the environments
RUN bash -c "source activate neuro \
    && conda install -y -q nb_conda"

# Install missing python software
RUN bash -c "source activate neuro \
    && conda install -y -q bokeh \
                           holoviews \
                           plotly \
                           scikit-image"

# Install PdVega
RUN bash -c "source activate neuro \
    && pip install pdvega \
    && jupyter nbextension install --sys-prefix --py vega3 \
    && jupyter nbextension enable vega3 --py --sys-prefix"

#----------------------------------------
# Create conda environment for Python 2.7
#----------------------------------------
RUN conda create -y -q --name py27 python=2.7 \
                                   ipython \
                                   jupyter \
                                   jupyter_contrib_nbextensions \
                                   matplotlib \
                                   mayavi \
                                   nbformat \
                                   nibabel \
                                   numpy \
                                   pandas \
                                   pip \
                                   scikit-image \
                                   scikit-learn \
                                   scipy \
                                   seaborn \
                                   spyder \
                                   statsmodels \
    && conda clean -tipsy \
    && /bin/bash -c "source activate py27 \
    && pip install -q --no-cache-dir mne \
                                     PySurfer \
                                     pdvega \
                                     pymvpa2 \
                                     https://github.com/nipy/nipype/tarball/master \
                                     https://github.com/INCF/pybids/tarball/master \
                                     nilearn \
                                     datalad[full] \
                                     nipy \
                                     duecredit \
                                     pprocess" \
    && sed -i '$isource activate neuro' $ND_ENTRYPOINT

#---------------------------------------
# Install MNE in the 'neuro' environment
#---------------------------------------
RUN bash -c "source activate neuro \
    && conda install -y -q jupyter \
                           matplotlib \
                           mayavi \
                           scikit-learn \
                           scipy \
                           spyder \
    && conda clean -tipsy \
    && pip install -q --no-cache-dir mne \
                                     PySurfer"

USER root

# Install workshop notebooks and slides
COPY [".", "/home/neuro/workshop"]
RUN chown -R neuro /home/neuro/workshop

USER neuro

WORKDIR /home/neuro

CMD ["jupyter-notebook"]
