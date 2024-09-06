APP_NAME 		:= harbor-bedrock.namecheapcloud.net/infrastructure/monarx-events
TAG 			:= $$(git log -1 --pretty=%h)
IMG 			:= ${APP_NAME}:${TAG}

.PHONY: help
help: ## show help
	@grep -E '^[a-zA-Z_-]+:.*?## .*$$' $(MAKEFILE_LIST) | sort | awk 'BEGIN {FS = ":.*?## "}; {printf "\033[36m%-30s\033[0m %s\n", $$1, $$2}'

.PHONY: build
build: ## build docker image
	@docker buildx build --platform linux/amd64 -t ${IMG} .

.PHONY: enter
enter: ## start a container and enter it
	@docker run --rm -it $(IMG) sh

.PHONY: start
start: build enter ## build an image and enter the container

.PHONY: stop
stop: ## stop a container
	@docker stop $(IMG)

.PHONY: login
login: ## login to docker registry
	@docker login harbor-bedrock.namecheapcloud.net

.PHONY: push
push: ## push the image to registry
	@docker push ${IMG}

.PHONY: test
test: ## run tests
	@cd test && go test -v ./...

.PHONY: all
all: login build push
