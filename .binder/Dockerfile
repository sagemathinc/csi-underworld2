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

RUN apt-get update -qq && \
    apt-get install -yq --no-install-recommends \
        bash-completion \
        build-essential \
        git \
        python3-minimal \
        python3-dev \
        python3-pip \
        libxml2-dev \
        ssh \
        curl \
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

COPY --from=underworldcode/base:lavavu /usr/local /usr/local
COPY --from=underworldcode/base:lavavu /opt/installed.txt  /opt/installed.txt
RUN apt-get update \
&&  DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends $(awk '{print $1'} /opt/installed.txt) \
&&  apt-get clean \
&&  rm -rf /var/lib/apt/lists/*

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
        yapf

# the following are required to have lavavu use our mesa build
ENV LV_LIB_DIRS=/usr/local/lib/x86_64-linux-gnu/
ENV LV_INC_DIRS=/usr/local/include/
ENV LV_OSMESA=1
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib/x86_64-linux-gnu/
RUN pip install  -vvv --no-binary :all: lavavu

RUN pip3 install git+https://github.com/underworldcode/underworld2@v2.10_release
RUN pip3 install UWGeodynamics

WORKDIR ${HOME}

USER ${NB_USER}

RUN git clone https://github.com/underworldcode/underworld2