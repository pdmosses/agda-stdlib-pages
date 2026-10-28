SOURCES := ../. ../doc ../src
PROTECT := docs/About.md
SERVER := 8020

AGDA-PAGES := ../../agda-pages
include ../../agda-pages/agda-pages.mk

# Assuming BRANCH is V-pages, set REF to V

BRANCH = $(shell git branch --show-current)

REF = $(patsubst %-pages,%, $(BRANCH))

OWNER = $(patsubst %.git,%,$(shell git remote get-url origin))

# Website generation for version V of agda-stdlib/pages:
#
# cd pages
# make properdocs
# make copy-html  # instead of gen-html
# make gen-md
# make serve      # for a local preview
# make deploy VERSION=V

# After testing, the above commands (omitting make serve) could be abbeviated to:
# make agda-stdlib-pages VERSION=V


# Development versions:
# master
# experimental

# Released versions:
# v2.4
# v2.3
# v2.2
# v2.1.1
# v2.1
# v2.0
# v1.7.3
# v1.7.2
# v1.7.1
# v1.7
# v1.6
# v1.5
# v1.4
# v1.3
# v1.2
# v1.1
# v1.0
# v0.17
# v0.16

# `make properdocs` updates properdocs.yml to refer to version V

.PHONY: properdocs
properdocs:
	@$(SD) '(site_url:[ ]+https://).*(.github.io/agda-stdlib/pages/).*$' \
	       '$$1$(OWNER)$$2$(REF)/' properdocs.yml
	@$(SD) '(repo_url:[ ]+https://github.com/).*(/agda-stdlib).*$' \
	       '$$1$(OWNER)$$2/$(BRANCH)' properdocs.yml
	@$(SD) '(repo_name:[ ]+).*(/agda-stdlib).*$' \
	       '$$1$(OWNER)$$2@$(BRANCH)' properdocs.yml

# `make copy-html` copies HTML files for version V from the gh-pages branch
# of agda/agda-stdlib to temp, removing the tags around the body of each file.
# The HTML files for the master version are at the root of the gh-pages branch.

.PHONY copy-html
copy-html: clean
	@git clone --depth 1 --single-branch --branch gh-pages https://github.com/agda/agda-stdlib temp/html
	@[ "$(REF)" == "master" ] && dir=temp/html || dir=temp/html/$(REF); \
	mv $${dir}/*.html temp; \
	rm -rf temp/html; \
	for file in temp/*.html; do \
	    $(SD) '\A[.|\n]*?<body>'  '' $${file}; \
	    $(SD) '</body>[.|\n]*?\z' '' $${file}; \
	done
	@echo "Generated HTML files in temp"

# `make gen-md` generates Markdown source files in docs from the HTML files in temp,
# independently of the version V.
