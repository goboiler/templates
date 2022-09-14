# NAME=hello_world
VERSION=latest
SRC=cmd
OS=linux darwin
ARCH=amd64 arm64

%: cmd/**/%.go 
	@for o in $(OS); do \
		for a in $(ARCH); do \
			GOOS=$$o GOARCH=$$a go build -o bin/$@/$$o-$$a $^; \
		done; \
	done

	@upx --best --lzma bin/$@/*
	@upx -t bin/$@/*

	@echo FROM --platform=linux/amd64 scratch > bin/$@/Dockerfile
	@echo COPY ./linux-amd64 /  >> bin/$@/Dockerfile
	@echo ENTRYPOINT [ "/linux-amd64" ] >> bin/$@/Dockerfile

build:	main \
				service1
	@for o in $^; do \
		docker build -t r.danecwalker.com/$$o:$(VERSION) bin/$$o; \
	done

all: build

clean:
	rm -rf bin
	go clean --cache