RG cuda=nvidia/cuda
ARG cuda_version=8.0-runtime
from ${cuda}:${cuda_version}

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ADD https://repo.continuum.io/miniconda/Miniconda3-latest-Linux-x86_64.sh /opt/miniconda.sh
ENV PATH "$PATH:/opt/miniconda/bin"
COPY ./entrypoint.sh /code/entrypoint.sh
COPY ./requirements.txt /code/requirements.txt

RUN apt-get update --fix-missing &&\
    apt-get install --no-install-recommends -y bzip2 \
                                               git \
                                               ca-certificates\
                                               libcupti-dev &&\
    /bin/sh /opt/miniconda.sh -b -p /opt/miniconda &&\
    rm /opt/miniconda.sh &&\
    /opt/miniconda/bin/conda install -y -c conda-forge -c soumith --file /code/requirements.txt &&\
    /opt/miniconda/bin/pip install --ignore-installed --upgrade \
                                   https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow_gpu-1.3.0-cp36-cp36m-linux_x86_64.whl \
                                   git+https://github.com/pytorch/text.git &&\
    /opt/miniconda/bin/pip install --no-deps git+https://github.com/pytorch/tnt.git &&\
    apt-get remove -y bzip2 ca-certificates git &&\
    rm -rf /var/lib/apt/lists/* &&\
    apt-get clean -y &&\
    /opt/miniconda/bin/conda clean -y --all

COPY ./entrypoint.sh /code
WORKDIR /code/notebooks

RUN chmod +x /code/entrypoint.sh

ENTRYPOINT ["/code/entrypoint.sh"]
