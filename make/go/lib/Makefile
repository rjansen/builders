LIB                ?= sample
LIB_PATH           := $(LIB)
REPO               := $(ROOT_REPO)/$(LIB_PATH)
BIN_DIR            := $(BASE_DIR)/$(LIB_PATH)
BIN_NAME           := $(TMP_DIR)/$(LIB_PATH)
GOVERSION          := 1.13.1
export GO111MODULE := on
PKGS               := $(shell go list ./...)
TEST_PKGS          := $(if $(TEST_PKGS),$(addprefix $(REPO)/,$(TEST_PKGS)),$(PKGS))
TESTS              ?= .
TEST_VERBOSITY     ?= $(if $(DEBUG),standard-verbose,short-verbose)
COVERAGE_FILE      := $(TMP_DIR)/$(subst /,_,$(LIB_PATH)).coverage
COVERAGE_HTML      := $(TMP_DIR)/$(subst /,_,$(LIB_PATH)).coverage.html

.PHONY: clearcache
clearcache:
	@echo "$(ROOT_REPO)@$(BUILD) clearcache"
	-$(HOME)/.cache/go-build
	-$(HOME)/gopath/pkg/mod
	-rm -Rf *.coverage.*

.PHONY: vendor
vendor:
	@echo "$(REPO)@$(BUILD) vendor"
	go mod verify && go mod vendor

.PHONY: vet
vet:
	@echo "$(REPO)@$(BUILD) vet"
	go vet $(TEST_PKGS)

.PHONY: lint
lint:
	@echo "$(REPO)@$(BUILD) lint"
	golint $(TEST_PKGS)

.PHONY: test
test:
	@echo "$(REPO)@$(BUILD) test $(REPO) - $(TEST_PKGS)"
	gotestsum -f $(TEST_VERBOSITY) -- -v -race -count 1 -run $(TESTS) $(TEST_PKGS)

.PHONY: itest
itest:
	@echo "$(REPO)@$(BUILD) itest"
	gotestsum -f $(TEST_VERBOSITY) -- -tags="integration" -v -race -count 1 -run $(TESTS) $(TEST_PKGS)

.PHONY: bench
bench:
	@echo "$(REPO)@$(BUILD) bench"
	gotestsum -f $(TEST_VERBOSITY) -- -bench=. -run="^$$" -benchmem $(TEST_PKGS)

.PHONY: coverage
coverage: $(TMP_DIR)
	@echo "$(REPO)@$(BUILD) coverage"
	@touch $(COVERAGE_FILE)
	gotestsum -f $(TEST_VERBOSITY) -- -tags="integration" -v -race -count 1 -run $(TESTS) \
		-covermode=atomic -coverpkg=$(PKGS) -coverprofile=$(COVERAGE_FILE) $(TEST_PKGS)

.PHONY: coverage.text
coverage.text: coverage
	@echo "$(REPO)@$(BUILD) coverage.text"
	go tool cover -func=$(COVERAGE_FILE)

.PHONY: coverage.html
coverage.html: coverage
	@echo "$(REPO)@$(BUILD) coverage.html"
	go tool cover -html=$(COVERAGE_FILE) -o $(COVERAGE_HTML)
	@open $(COVERAGE_HTML) || google-chrome $(COVERAGE_HTML) || google-chrome-stable $(COVERAGE_HTML)
