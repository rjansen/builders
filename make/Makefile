NAME               := project_name
ROOT_REPO          := github.com/rjansen/$(NAME)
VERSION            := $(notdir $(shell git describe --tags --always))
BUILD              := $(VERSION).$(shell git rev-parse --short HEAD)
MAKEFILE           := $(lastword $(MAKEFILE_LIST))
BASE_DIR           := $(shell cd $(dir $(MAKEFILE)); pwd)
TMP_DIR            := $(BASE_DIR)/tmp
DOCKER_NAME        := e-pedion/$(NAME)
DOCKER_FILE        := ./Dockerfile

.PHONY: clean
clean:
	@echo "$(ROOT_REPO)@$(BUILD) clean"
	-rm -Rf $(TMP_DIR)

$(TMP_DIR):
	mkdir -p $(TMP_DIR)

.PHONY: setup
setup: $(TMP_DIR)
	@echo "$(ROOT_REPO)@$(BUILD) setup"

.PHONY: docker
docker.build:
	@echo "$(ROOT_REPO)@$(BUILD) docker"
	docker build --build-arg UID=$(shell id -u) --build-arg GID=$(shell id -g) \
		         -t $(DOCKER_NAME) -t $(DOCKER_NAME):$(VERSION) -f $(DOCKER_FILE) .

.PHONY: docker.bash
docker.bash:
	@echo "$(ROOT_REPO)@$(BUILD) docker.bash"
	docker run --rm --name $(NAME)-bash --entrypoint bash -it -u $(shell id -u):$(shell id -g) \
			   -v $(BASE_DIR):/go/src/$(ROOT_REPO) $(DOCKER_NAME)

docker.%:
	@echo "$(ROOT_REPO)@$(BUILD) docker.$*"
	docker run --rm --name $(NAME)-run -u $(shell id -u):$(shell id -g) \
    		    -v $(BASE_DIR):/go/src/$(ROOT_REPO) $(DOCKER_NAME) $*

include .make/*
