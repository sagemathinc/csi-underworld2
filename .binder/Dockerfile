FROM ubuntu:focal
MAINTAINER https://github.com/underworldcode/

ENV DEBIAN_FRONTEND=noninteractive LANG=C.UTF-8 LC_ALL=C.UTF-8 PYTHONUNBUFFERED=0

ARG NB_USER
ARG NB_UID
ENV USER ${NB_USER}
ENV HOME /home/${NB_USER}

RUN adduser --disabled-password \
    --gecos "Default user" \
    --uid ${NB_UID} \
    ${NB_USER}

WORKDIR ${HOME}

RUN apt-get update -qq && \
    apt-get install -yq --no-install-recommends \
        bash-completion \
        build-essential \
        git \
        python3-minimal \
        python3-dev \
        python3-pip \
        libxml2-dev \
        xorg-dev \
        ssh \
        curl \
        libfreetype6-dev \
        libpng-dev \
        libxft-dev \
        xvfb \
        freeglut3 \
        freeglut3-dev \
        libgl1-mesa-dri \
        libgl1-mesa-glx \
        python3-tk \
        rsync \
        vim \
        less \
        xauth \
        swig \
        gdb \
        python3-dbg \
        cmake \
        python3-setuptools \
        wget \
        ca-certificates \
        petsc-dev \
        gfortran \
        htop && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*


# Install setuptools and wheel first, needed by plotly
RUN pip3 config --global set global.cache-dir false && \
    pip3 install -U pip         && \
    pip3 install -U setuptools  && \
    pip3 install -U wheel       && \
    pip3 install --no-cache-dir    \
        packaging \
        appdirs \
        numpy \
        pandas \
        jupyter \
        plotly \
        matplotlib \
        pillow \
        pyvirtualdisplay \
        ipython \
        ipyparallel \
        pint \
        sphinx \
        sphinx_rtd_theme \
        sphinxcontrib-napoleon \
        mock \
        scipy \
        tabulate \
        mpi4py \
        scons \
        h5py \
        lavavu \
        yapf

RUN pip3 install git+https://github.com/underworldcode/underworld2@v2.10_release
RUN pip3 install UWGeodynamics

# environment variable will internally run xvfb when uw.visualisation is imported,
# see /opt/underworld2/visualisation/__init__.py
ENV UW_USE_XVFB 1

USER ${NB_USER}
