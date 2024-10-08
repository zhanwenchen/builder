SHELL=/usr/bin/env bash

DESIRED_CUDA ?= 11.8
PACKAGE_NAME ?= magma-cuda118
CUDA_ARCH_LIST ?= -gencode arch=compute_80,code=sm_80 -gencode arch=compute_86,code=sm_86

DOCKER_RUN = set -eou pipefail; docker run --rm -i \
	-v $(shell git rev-parse --show-toplevel):/builder \
	-w /builder \
	-e DESIRED_CUDA=${DESIRED_CUDA} \
	-e PACKAGE_NAME=${PACKAGE_NAME} \
	-e CUDA_ARCH_LIST="${CUDA_ARCH_LIST}" \
	"pytorch/conda-builder:cuda${DESIRED_CUDA}" \
	magma/build_magma.sh

.PHONY: all
all: magma-cuda126
all: magma-cuda124
all: magma-cuda121
all: magma-cuda118

.PHONY:
clean:
	$(RM) -r magma-*
	$(RM) -r output

.PHONY: magma-cuda126
magma-cuda126: DESIRED_CUDA := 12.6
magma-cuda126: PACKAGE_NAME := magma-cuda126
magma-cuda126:
	CMAKE_CXX_FLAGS="-march=native" CMAKE_C_FLAGS="-march=native" DESIRED_CUDA=12.6 PACKAGE_NAME=magma-cuda126 CUDA_ARCH_LIST="-gencode arch=compute_86,code=sm_86" ./build_magma.sh

.PHONY: magma-cuda124
magma-cuda124: DESIRED_CUDA := 12.4
magma-cuda124: PACKAGE_NAME := magma-cuda124
magma-cuda124:
	$(DOCKER_RUN)

.PHONY: magma-cuda121
magma-cuda121: DESIRED_CUDA := 12.1
magma-cuda121: PACKAGE_NAME := magma-cuda121
magma-cuda121:
	$(DOCKER_RUN)

.PHONY: magma-cuda118
magma-cuda118: DESIRED_CUDA := 11.8
magma-cuda118: PACKAGE_NAME := magma-cuda118
magma-cuda118: CUDA_ARCH_LIST += -gencode arch=compute_37,code=sm_37
magma-cuda118:
	$(DOCKER_RUN)
