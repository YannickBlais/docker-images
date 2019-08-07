# to build: docker build -t nvdarkrai .

FROM nvcr.io/nvidia/tensorflow:19.03-py3

ARG _PY_SUFFIX=3.6
ARG PYTHON=python${_PY_SUFFIX}
ARG PIP=pip${_PY_SUFFIX}

# See http://bugs.python.org/issue19846
ENV LANG C.UTF-8

# Install python 3.6
RUN apt-get update -y
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:jonathonf/python-${_PY_SUFFIX}
RUN apt-get update && apt-get install -y \
    ${PYTHON} \
    python3-pip

RUN curl https://bootstrap.pypa.io/get-pip.py | ${PYTHON}
RUN pip --no-cache-dir install --upgrade \
    pip \
    setuptools

# Some TF tools expect a "python" binary
RUN ln -s $(which ${PYTHON}) /usr/local/bin/python 

RUN apt-get update && apt-get install -y \
    ${PYTHON}-dev

RUN pip --no-cache-dir install \
    Pillow==5.3.0 \
    h5py==2.8.0 \
    keras_applications \
    keras_preprocessing \
    matplotlib==3.0.0 \
    mock \
    numpy==1.15.3 \
    scipy==1.1.0 \
    sklearn \
    pandas

# TF build scripts/options updates
COPY nvbuild.sh /opt/tensorflow
COPY nvbuildopts /opt/tensorflow
COPY bazel_build.sh /opt/tensorflow
COPY protobuf.cmake /opt/tensorflow/tensorflow/contrib/cmake/external/
COPY BUILD /opt/tensorflow/tensorflow 

# build Tensorflow
WORKDIR /opt/tensorflow
RUN ./nvbuild.sh --python3.6
