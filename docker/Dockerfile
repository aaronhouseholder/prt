FROM python:3.11

# Install OS dependencies
RUN DEBIAN_FRONTEND=noninteractive \
    apt-get update && apt-get install -y \
        build-essential \
	cmake \
        gfortran \
        vim \
	libopenmpi-dev \
	python3-scipy \
	python3-numpy \
	python3-matplotlib \
	python3-progressbar \
	ipython3 \
	libblas-dev \
	liblapack-dev \
	libatlas-base-dev \
	git \
    && rm -rf /var/lib/apt/lists/* \
    && apt-get clean

WORKDIR /app
RUN git clone https://github.com/JohannesBuchner/MultiNest \
    && git clone https://github.com/JohannesBuchner/cuba/

WORKDIR /app/MultiNest/build
RUN cmake .. && make

WORKDIR /app/cuba
RUN ./configure && ./makesharedlib.sh 

WORKDIR /app 

# Install Python dependencies
RUN pip3 install --upgrade pip --no-cache-dir \
    && pip3 install --upgrade --no-cache-dir numpy \
    && pip3 install --upgrade --no-cache-dir \
        petitRADTRANS \
        jupyter \
	pymultinest \
	cython \
	species

# Set input data path for petitRADTRANS
# Note: must be the same path that volume is mounted to in `compose.yml`
ENV pRT_input_data_path="/app/input_data"
ENV LD_LIBRARY_PATH=/app/MultiNest/lib:/app/cuba/directory/:$LD_LIBRARY_PATH

ENTRYPOINT [ "/bin/bash" ]

