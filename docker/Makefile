VERSION=1.0-3.5.5
PROJECT=rodrigomakoto

all: build

build:
	docker build --pull -t ${PROJECT}/kubernetes-zookeeper:${VERSION} .

push: build
	docker push ${PROJECT}/kubernetes-zookeeper:${VERSION}

.PHONY: all build push
