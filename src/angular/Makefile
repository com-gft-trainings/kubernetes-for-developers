SHELL := /bin/bash
MAKELEVEL:=SRC/GO

-include ../../.makeinfo
include ../../.makeproject
include ../../.makecommands
-include ../../.makeset

TIMESTAMP ?= $(shell date +%Y%m%d%H%M%S)
GITHASH ?= $(shell ( git rev-parse HEAD 2>/dev/null || ( $(ECHO) 'unknown' ; exit 0 ) ) ) 
_GITHASH := $(shell ( $(ECHO) "$(GITHASH)" | sed 's/^ *//; s/  *$$//; s/  */\\|/g') )
# MODULES := $(shell find ./src -maxdepth 1 -mindepth 1 -type d | sed 's/^..//' | grep -v '^src/common$$' | grep -v '^src/loadsucker$$' )
MODULES := fe-weatherstation

-include ../../.makesrc
include ../../.makebuild
include ../../.makedocker
# include ../../.makekubernetes

.DEFAULT_GOAL=all
.PHONY: clean docker docker-local dist distclean bin info _buildinfo _dockerinfo clustersetup clusterclean $(MODULES)

docker-ls: _docker-ls-all
docker-clean: _docker-clean-all
docker-clean-all: _docker-clean-all
docker-distclean: _docker-clean-all

DOCKER_TARGET=all
dist: all
all: DOCKER_TARGET=all
all: $(MODULES)
docker: DOCKER_TARGET=docker
docker: $(MODULES)
docker-local: DOCKER_TARGET=docker-local
docker-local: $(MODULES)
$(MODULES):
	@$(MAKE) -C $@ $(DOCKER_TARGET)

buildinfo: _buildinfo
	@for i in $(MODULES) ; do \
	    if [[ ! -d "$$i" ]] ; then continue ; fi ; \
			make -C "$$i" buildinfo ; \
	done

clean distclean ls:
	@for i in $(MODULES) ; do \
	    if [[ ! -d "$$i" ]] ; then continue ; fi ; \
			$(MAKE) -C "$$i" $@ ; \
	done
