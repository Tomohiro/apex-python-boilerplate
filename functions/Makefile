TOP = $(shell cd ../../ && pwd)

help:
	@echo "Please type: make [target]"
	@echo "  all    Run several tasks. (clean/deps/lint/test)"
	@echo "  deps   Install runtime dependencies"
	@echo "  dist   Ship packages to upload to AWS Lambda"
	@echo "  lint   Lint sources"
	@echo "  test   Run tests"
	@echo "  run    Run the function on local environment"
	@echo "  clean  Cleanup artifacts, temporary files"
	@echo "  help   Show this help messages"

all: clean deps lint test

deps:
	@[ ! -d lib ] && mkdir lib || :
	pip install -r requirements-dev.txt -t lib --upgrade

dist: clean
	@[ ! -d lib ] && mkdir lib || :
	pip install -r requirements.txt -t lib

lint:
	@flake8 --config $(TOP)/.flake8 src/

test:
	@tox

run:
	@if [ -d events ]; then\
		for json in `ls events/*.json`; do\
			python-lambda-local -f handle main.py $$json;\
		done;\
	fi;

clean:
	find . -iname '*.pyc' | xargs rm -f
	rm -rf build
	rm -rf .tox
	rm -rf *.egg-info
	rm -rf lib

.PHONY: help deps dist lint test run clean
