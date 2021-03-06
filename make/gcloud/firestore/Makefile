PROJECT_ID                            ?= project-id
SERVICE_ACCOUNT                       ?= service-account
SERVICE_ACCOUNT_DISPLAYNAME           ?= "Service Account"
SERVICE_ACCOUNT_EMAIL                 := $(SERVICE_ACCOUNT)@$(PROJECT_ID).iam.gserviceaccount.com
SERVICE_ACCOUNT_KEY                   ?= $(HOME)/.$(PROJECT_ID)/$(SERVICE_ACCOUNT).json
SERVICE_ACCOUNT_KEY_DIR               := $(dir $(SERVICE_ACCOUNT_KEY))
export GOOGLE_APPLICATION_CREDENTIALS ?= $(SERVICE_ACCOUNT_KEY)

.PHONY: firestore.emulator.start
firestore.emulator.start:
	gcloud beta emulators firestore start

.PHONY: firestore.createsa
firestore.createsa:
	gcloud iam service-accounts create $(SERVICE_ACCOUNT) --display-name $(SERVICE_ACCOUNT_DISPLAYNAME)
	gcloud projects add-iam-policy-binding $(PROJECT_ID) \
	   	--member serviceAccount:$(SERVICE_ACCOUNT)@$(PROJECT_ID).iam.gserviceaccount.com \
		--role roles/datastore.user

$(SERVICE_ACCOUNT_KEY):
	gcloud iam service-accounts keys create $(SERVICE_ACCOUNT_KEY) \
		--iam-account $(SERVICE_ACCOUNT)@$(PROJECT_ID).iam.gserviceaccount.com

.PHONY: firestore.createkey
firestore.createkey: $(SERVICE_ACCOUNT_KEY)
	@ls -l $(SERVICE_ACCOUNT_KEY)

.PHONY: firestore.encodekeycontent
firestore.encodekeycontent: $(SERVICE_ACCOUNT_KEY)
	@cat $(SERVICE_ACCOUNT_KEY) | base64 -w 0

.PHONY: firestore.setup
firestore.setup: firestore.createsa firestore.createkey

ifdef SERVICE_ACCOUNT_KEY_CONTENT
$(SERVICE_ACCOUNT_KEY_DIR):
	mkdir -p $(SERVICE_ACCOUNT_KEY_DIR)

firestore.decodekeycontent: $(SERVICE_ACCOUNT_KEY_DIR)
	@echo $(SERVICE_ACCOUNT_KEY_CONTENT) | base64 --decode -w 0 > $(SERVICE_ACCOUNT_KEY)
	ls -l $(SERVICE_ACCOUNT_KEY)
endif
