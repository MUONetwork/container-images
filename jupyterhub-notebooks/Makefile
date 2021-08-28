.PHONY: help

SHELL:=bash
PACKAGE:=jupyter_codeserver_proxy
BASEDIR=${PWD}
TAG=$(shell date +"%Y.%m.%d")


help:
	@echo "Generate and push Jupyterhub images for MUON"
	@echo "================="
	@echo
	@grep -E '^[a-zA-Z0-9_%/-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

build-all: build-package build-images ## build the package and the image

build-package: ## build the python package
	printf "\033[96m======\n%s : %s\n====== \033[0m \n" "Building python dist whl package"; \
	python setup.py bdist_wheel

build-images: ## build the latest docker images
	@for f in $(shell ls ${BASEDIR} ); do \
		if [[ $${f} == *"notebook" ]]; then \
			printf "\033[96m======\n%s : %s\n====== \033[0m \n" "Building docker image for" "$${f}"; \
			docker build -t mohitsharma44/$${f}:${TAG} -f ${BASEDIR}/$${f}/Dockerfile .; \
		fi; \
	done;

clean: ## remove python package target dirs
	rm -rf ./build
	rm -rf ./dist
	rm -rf "./$(PACKAGE).egg-info"

push-images: ## upload the docker images
	@for f in $(shell ls ${BASEDIR} ); do \
		if [[ $${f} == *"notebook" ]]; then \
			printf "\033[96m======\n%s : %s\n====== \033[0m \n" "Pushin docker image for" "$${f}"; \
			docker push mohitsharma44/$${f}:${TAG}; \
			docker push mohitsharma44/$${f}:latest; \
		fi; \
    done;