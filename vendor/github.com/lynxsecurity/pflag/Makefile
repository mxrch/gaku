.PHONY: lint test unittest cobratest clean

ifeq (, $(shell which golangci-lint 2> /dev/null))
	$(error Unable to locate golangci-lint! Ensure it is installed and available in PATH before re-running.)
endif

gotest =
ifeq (, $(shell which richgo 2> /dev/null))
	gotest = go test
else
	gotest = richgo test
endif

default: all

all: lint test clean

lint:
	@echo '********** LINT TEST **********'
	golangci-lint run

unittest:
	@echo '********** UNIT TEST **********'
	@$(gotest) -failfast -v -race -cover

cobratest:
	@echo '********** COBRA TEST **********'
	@set -e \
		&& test -d cobra || { git clone https://github.com/spf13/cobra.git && ln -s ../../pflag cobra/pflag ; } \
		&& cd cobra \
		&& go mod edit -replace github.com/spf13/pflag=./pflag \
		&& $(gotest) -v ./...

test: unittest cobratest lint

clean:
	rm -rf cobra
