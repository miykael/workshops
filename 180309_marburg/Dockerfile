# This is the Dockerfile to create the docker image for the workshop in Marburg
#
# For the python and nipype part of the workshop use the following command:
#    docker run -it --rm -p 8888:8888 miykael/workshop_marburg
#
# For the MNE part of the workshop use the following command:
#    docker run -it --rm -p 8888:8888 miykael/workshop_marburg ./mne.sh

FROM djarecka/nipype_tutorial

ARG DEBIAN_FRONTEND=noninteractive

#-------------------------
# Create conda environment
#-------------------------
RUN conda create -y -q --name mne python=2.7 \
                                  scipy \
                                  matplotlib \
                                  scikit-learn \
                                  mayavi \
                                  jupyter \
                                  spyder \
    && sync && conda clean -tipsy && sync \
    && /bin/bash -c "source activate mne \
    && pip install -q --no-cache-dir PySurfer \
                                     mne" \
    && sync \
    && sed -i '$isource activate neuro' $ND_ENTRYPOINT

RUN /bin/bash -c "printf '#!/bin/bash\nsource activate mne\njupyter notebook\' > /home/neuro/mne.sh"
RUN chmod +x /home/neuro/mne.sh

USER neuro

WORKDIR /home/neuro

CMD ["jupyter-notebook"]
