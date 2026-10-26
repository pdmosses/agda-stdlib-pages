SOURCES := ../. ../doc ../src
PROTECT := docs/About.md
SERVER := 8020

AGDA-PAGES := ../../agda-pages
include ../../agda-pages/agda-pages.mk

# Website generation:
#
# cd pages
# make index
# make check
# make web
# make serve
# make deploy VERSION=...

# Optionally:
# make clean-index

# `make index` generates Agda files in the repository root:
#  - Everything.agda
#  - EverythingSafe.agda
#  - index.agda

.PHONY: index
index:
	@cd ..; \
	cp .github/tooling/* .; \
	cabal run GenerateEverything; \
	./index.sh; \
	echo "Generated Agda files in root directory"

# `make clean-index` removes the copied tooling files
# and the files created by GenerateEverything:

.PHONY: clean-index
clean-index:
	@cd ..; \
	rm $$(ls .github/tooling/*); \
	rm Everything.agda EverythingSafe.agda

# `make properdocs` updates properdocs.yml to refer to the current repo branch

BRANCH = $(shell git branch --show-current)

OWNER = $(patsubst %.git,%,$(shell git remote get-url origin))

.PHONY: properdocs
properdocs:
	@$(SD) '(site_url:[ ]+https://).*(.github.io/agda-stdlib/pages/).*$' \
	       '$$1$(OWNER)$$2$(BRANCH)/' properdocs.yml
	@$(SD) '(repo_url:[ ]+https://github.com/).*(/agda-stdlib).*$' \
	       '$$1$(OWNER)$$2$(BRANCH)' properdocs.yml
	@$(SD) '(repo_name:[ ]+).*(/agda-stdlib).*$' \
	       '$$1$(OWNER)$$2@$(BRANCH)' properdocs.yml
