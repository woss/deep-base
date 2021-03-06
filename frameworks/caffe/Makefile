
GPU_SUPPORT ?= 0

.PHONY: dependencies
dependencies:
	apt-get -y install libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libhdf5-serial-dev protobuf-compiler
	apt-get -y install --no-install-recommends libboost-all-dev
	apt-get -y install libatlas-base-dev
	apt-get -y install libgflags-dev libgoogle-glog-dev liblmdb-dev
	apt-get -y install libdc1394-22-dev libdc1394-22 libdc1394-utils python-opencv python-pip gfortran
	pip install cython
	pip install scikit-image
	pip install protobuf

.PHONY: src
src:
	git clone --recursive https://github.com/BVLC/caffe.git src

.PHONY: build
build:
ifeq ($(GPU_SUPPORT),1)
	cp config/Makefile.config.gpu src/Makefile.config
else
	cp config/Makefile.config.cpu src/Makefile.config
endif
	cd src; make all
	cd src; make pycaffe

.PHONY: install
install:
	cd src; make distribute
	export PYTHONPATH=/workdir/frameworks/caffe/src/python:$$PYTHONPATH
	echo 'export PYTHONPATH=/workdir/frameworks/caffe/src/python:$$PYTHONPATH' >> ~/.bashrc

.PHONY: load_test
load_test:
ifeq ($(GPU_SUPPORT),1)
	LD_LIBRARY_PATH=$(LD_LIBRARY_PATH):/usr/local/cuda/lib64 PYTHONPATH=/workdir/frameworks/caffe/src/python:$(PYTHONPATH) python test/load.py
else
	PYTHONPATH=/workdir/frameworks/caffe/src/python:$(PYTHONPATH) python test/load.py
endif
