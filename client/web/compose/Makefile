.PHONY: dep test build release upload


YARN_FLAGS            ?= --non-interactive --no-progress --silent --emoji false
YARN                   = yarn $(YARN_FLAGS)

REPO_NAME 						?= corteza-webapp-compose

BUILD_FLAVOUR         ?= corteza
BUILD_FLAGS           ?= --production
BUILD_DEST_DIR         = dist
BUILD_TIME            ?= $(shell date +%FT%T%z)
BUILD_VERSION         ?= $(shell git describe --tags --abbrev=0)

BUILD_NAME             = $(REPO_NAME)-$(BUILD_VERSION)

RELEASE_NAME           = $(BUILD_NAME).tar.gz
RELEASE_EXTRA_FILES   ?= README.md LICENSE CONTRIBUTING.md DCO
RELEASE_PKEY          ?= .upload-rsa

dep:
	$(YARN) install

test:
	$(YARN) lint
	$(YARN) test:unit

build:
	export BUILD_VERSION=${BUILD_VERSION} && $(YARN) build $(BUILD_FLAGS)

release:
	@ echo $(RELEASE_NAME)
	@ cp $(RELEASE_EXTRA_FILES) $(BUILD_DEST_DIR)
	@ tar -C $(BUILD_DEST_DIR) -czf $(RELEASE_NAME) $(dir $(BUILD_DEST_DIR))

upload:
	AWS_ACCESS_KEY_ID=$(AWS_ACCESS_KEY_ID) AWS_SECRET_ACCESS_KEY=$(AWS_SECRET_ACCESS_KEY) aws s3 cp *.tar.gz s3://$(S3_BUCKET)/ --region $(S3_REGION)

