NAME = sample
VERSION = 0.0.1

.PHONY: all clean build tag_latest release debug run

all: build

clean:
	@CID=$(shell docker ps -a | awk '{ print $$1 " " $$2 }' | grep $(NAME) | awk '{ print $$1 }'); if [ ! -z "$$CID" ]; then echo "Removing container which reference $(NAME)"; for container in $(CID); do docker rm -f $$container; done; fi;
	@echo "Removing image $(NAME)"
	@if docker images $(NAME) | awk '{ print $$2 }' | grep -q -F $(VERSION); then docker rmi -f $(NAME):$(VERSION); fi
	@if docker images $(NAME) | awk '{ print $$2 }' | grep -q -F latest; then docker rmi -f $(NAME):latest; fi


build: clean
	docker build -t $(NAME):$(VERSION) --rm .

tag_latest:
	@docker tag $(NAME):$(VERSION) $(NAME):latest

release: build tag_latest
	docker push $(NAME)

debug:
	docker run -it $(NAME):$(VERSION)

run:
	@echo "IPAddress =" $$(docker inspect --format '{{.NetworkSettings.IPAddress}}' $$(docker run -d $(NAME):$(VERSION)))
