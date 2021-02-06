FROM nvidia/cuda:11.0.3-cudnn8-runtime-ubuntu18.04

ENV com.nvidia.cuda.version 11.0.3
ENV com.nvidia.volumes.needed nvidia_driver
ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV DEBIAN_FRONTEND noninteractive
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64
ENV NCCL_DEBUG=INFO
ENV HOROVOD_GPU_ALLREDUCE=NCCL

# Install Common Dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    # SSH and RDMA
    libmlx4-1 \
    libmlx5-1 \
    librdmacm1 \
    libibverbs1 \
    libmthca1 \
    libdapl2 \
    dapl2-utils \
    openssh-client \
    openssh-server \
    iproute2 && \
    # Others
    apt-get install -y \
    build-essential \
    bzip2 \
    libbz2-1.0 \
    systemd \
    git \
    wget \
    cpio \
    libsm6 \
    libxext6 \
    libxrender-dev \
    libssl1.0.0 \
    linux-image-aws \ 
    linux-image-azure \ 
    linux-image-generic \
    linux-image-kvm \
    linux-image-lowlatency \
    linux-image-virtual \
    linux-image-gke \
    linux-image-oem \
    slapd \
    perl \ 
    ca-certificates \
    fuse && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Conda Environment
ENV MINICONDA_VERSION 4.7.12.1
ENV PATH /opt/miniconda/bin:$PATH
RUN wget -qO /tmp/miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh && \
    bash /tmp/miniconda.sh -bf -p /opt/miniconda && \
    conda clean -ay && \
    rm -rf /opt/miniconda/pkgs && \
    rm /tmp/miniconda.sh && \
    find / -type d -name __pycache__ | xargs rm -rf

RUN pip install torch==1.7.0+cu110 -f https://download.pytorch.org/whl/torch_stable.html

RUN apt-get update && \
    apt-get install -y cuda-nvcc-11-0 cuda-libraries-dev-11-0 libcusparse-dev-11-0 libcublas-dev-11-0 && \
    pip install -v --install-option="--cpp_ext" --install-option="--cuda_ext" 'git+https://github.com/nvidia/apex.git' && \
    find / -type d -name __pycache__ | xargs rm -rf && \
    apt-get clean -y && \
    rm -rf /var/lib/apt/lists/*

# Open-MPI installation
ENV OPENMPI_VERSION 3.1.2
RUN mkdir /tmp/openmpi && \
    cd /tmp/openmpi && \
    wget https://download.open-mpi.org/release/open-mpi/v3.1/openmpi-${OPENMPI_VERSION}.tar.gz && \
    tar zxf openmpi-${OPENMPI_VERSION}.tar.gz && \
    cd openmpi-${OPENMPI_VERSION} && \
    ./configure --enable-orterun-prefix-by-default && \
    make -j $(nproc) all && \
    make install && \
    ldconfig && \
    rm -rf /tmp/openmpi

