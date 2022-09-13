NAME=hello_world
VERSION=latest

clean:
	@rm -rf bin
	@rm -rf out
	@go clean -cache

bin/main: **/main/main.go
	@go build -ldflags="-s -w" -o $@ $<

out/linux_amd64: **/main/main.go
	@GOOS=linux GOARCH=amd64 go build -ldflags="-s -w" -o $@ $<
	@upx --best --lzma $@

run: bin/main
	@$<

build: out/linux_amd64
	@docker build -t r.danecwalker.com/$(NAME):$(VERSION) -f docker/Dockerfile .
	@rm -rf out

push: build
	@docker push r.danecwalker.com/$(NAME):$(VERSION)