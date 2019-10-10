GQLGEN_SCRIPT ?= $(BASE_DIR)/scripts/gqlgen.go
GQLGEN_CONFIG ?= $(BASE_DIR)/gqlgen.yml

.PHONY: gqlgen.setup
gqlgen.setup:
	dep ensure

.PHONY: gqlgen
gqlgen:
	GO111MODULE=off go run $(GQLGEN_SCRIPT) -v -c $(GQLGEN_CONFIG)
