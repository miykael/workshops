# This is the Dockerfile to create the docker image for the workshop in Frankfurt
#
# For the python and nipype part of the workshop use the following command:
#    docker run -it --rm -p 8888:8888 miykael/workshop_frankfurt

FROM miykael/nipype_tutorial

ARG DEBIAN_FRONTEND=noninteractive

USER root

# Install software for MNE
RUN apt-get update -qq \
    && apt-get install -y -q --no-install-recommends \
           gcc \
           libc-dev \
           libglu1-mesa \
           libgl1-mesa-glx \
           libxt6 \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

USER neuro

#---------------------------------------
# Install MNE in the 'neuro' environment
#---------------------------------------
RUN bash -c 'curl -L https://raw.githubusercontent.com/mne-tools/mne-python/master/environment.yml -o /opt/conda/environment.yml'
RUN conda env create -q --name mne --file /opt/conda/environment.yml \
    && rm -f /opt/conda/environment.yml

#------------------------------------------------------------
# Update conda environment 'neuro' for visualization examples
#------------------------------------------------------------

# Install missing python software
RUN bash -c "source activate neuro \
    && conda install -y -q bokeh \
                           holoviews \
                           nilearn \
                           plotly \
                           scikit-image \
                           scipy=0.19" \
    && conda clean -tipsy

# Install PdVega
RUN bash -c "source activate neuro \
    && pip install pdvega \
    && jupyter nbextension install --sys-prefix --py vega3 \
    && jupyter nbextension enable vega3 --py --sys-prefix"

USER root

# Install workshop notebooks and slides
COPY [".", "/home/neuro/workshop"]
RUN chown -R neuro /home/neuro/workshop
RUN rm -rf /opt/conda/pkgs/*

USER neuro

RUN mkdir -p ~/.jupyter && echo c.NotebookApp.ip = \"0.0.0.0\" > ~/.jupyter/jupyter_notebook_config.py
WORKDIR /home/neuro

CMD ["jupyter-notebook"]
