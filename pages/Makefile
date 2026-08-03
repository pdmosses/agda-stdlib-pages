SOURCES := ../. ../doc ../src
PROTECT := docs/About.md

AGDA-PAGES := ../../agda-pages
include ../../agda-pages/agda-pages.mk

EVERYTHING := ../index.agda ../Everything.agda ../EverythingSafe.agda

.PHONY: everything
everything: $(EVERYTHING)
	@cd ..; \
	cp .github/tooling/* .; \
	cabal run GenerateEverything; \
	./index.sh; \
	echo "Generated Agda files in ."
