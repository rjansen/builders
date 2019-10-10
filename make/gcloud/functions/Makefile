FUNCTION_NAME              ?= $(MODULE)
FUNCTION_LIST              := function
FUNCTION_RUNTIME           := go111
FUNCTION_ENTRYPOINT        ?= Handler
FUNCTION_TIMEOUT           ?= 60
FUNCTION_MEMORY            ?= 128
define FUNCTION_ENVIRONMET =
VERSION=$(VERSION)\
PROJECT_ID=$(PROJECT_ID)
endef
comma  := ,
space  :=
space  +=
FUNCTION_ENVIRONMET  := $(subst $(space),$(comma),$(FUNCTION_ENVIRONMET))

.PHONY: deploy
ifneq ($(filter $(MODULE),$(FUNCTION_LIST)),)
deploy: vendor
	gcloud beta functions deploy $(FUNCTION_NAME) --source $(MODULE_DIR) --runtime $(FUNCTION_RUNTIME) \
	   	--entry-point $(FUNCTION_ENTRYPOINT) --timeout $(FUNCTION_TIMEOUT) --memory $(FUNCTION_MEMORY) \
		--trigger-http --service-account $(SERVICE_ACCOUNT_EMAIL) --set-env-vars $(FUNCTION_ENVIRONMET)
else
deploy:
	$(error "err_nondeployable_module: module_name=$(MODULE) avaiable_list=[$(FUNCTION_LIST)]")
endif
.PHONY: deploy
ifneq ($(filter $(MODULE),$(FUNCTION_LIST)),)
deploy: vendor
	gcloud beta functions deploy $(FUNCTION_NAME) --source $(MODULE_DIR) --runtime $(FUNCTION_RUNTIME) \
	   	--entry-point $(FUNCTION_ENTRYPOINT) --timeout $(FUNCTION_TIMEOUT) --memory $(FUNCTION_MEMORY) \
		--trigger-http --service-account $(SERVICE_ACCOUNT_EMAIL) --set-env-vars $(FUNCTION_ENVIRONMET)
else
deploy:
	$(error "err_nondeployable_module: module_name=$(MODULE) avaiable_list=[$(FUNCTION_LIST)]")
endif
