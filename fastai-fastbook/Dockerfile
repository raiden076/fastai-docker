# Paperspace base image is located in Dockerhub registry: paperspace/gradient_base
# Paperspace Fast.ai image is located Dockerhub registry: paperspace/fastai

# ==================================================================
# Initial Setup
# ------------------------------------------------------------------

FROM paperspace/gradient-base:pt112-tf29-jax0317-py39-20221019


# ==================================================================
# Directories & Tools
# ------------------------------------------------------------------

RUN mkdir /content && \
    apt-get update -y && \
    apt-get install -y graphviz


# ==================================================================
# Mambaforge
# ------------------------------------------------------------------

# Based on https://github.com/conda-forge/miniforge
COPY env.yaml /tmp/env.yaml

RUN wget "https://github.com/conda-forge/miniforge/releases/latest/download/Mambaforge-$(uname)-$(uname -m).sh" && \
    bash Mambaforge-$(uname)-$(uname -m).sh -b && \
    ~/mambaforge/condabin/conda init && \
    /root/mambaforge/bin/mamba install jupyter jupyterlab ipython -c conda-forge -y
#    /root/mambaforge/bin/mamba install fastbook -c fastchan -y
#RUN /root/mambaforge/bin/mamba env update -f /tmp/env.yaml -n base --prune
RUN /root/mambaforge/bin/mamba env create -f /tmp/env.yaml
#    rm Mambaforge-$(uname)-$(uname -m).sh

ENV PATH=$PATH:/root/mambaforge/bin
RUN echo "conda activate deep-learning" >> /root/.bashrc


# ==================================================================
# Fast.ai
# ------------------------------------------------------------------

# RUN python3 -m pip install --upgrade pip && \
#     python3 -m pip install --upgrade fastai >=2.6.0 && \
#     python3 -m pip install --upgrade fastbook && \
#     python3 -m pip install --upgrade jupyterlab-git


# ==================================================================
# Config & Startup
# ------------------------------------------------------------------

ENV USER fastai
WORKDIR /notebooks
RUN chmod -R a+w /notebooks
WORKDIR /notebooks

COPY run.sh /run.sh
RUN chmod +x /run.sh

CMD ["/run.sh"]
