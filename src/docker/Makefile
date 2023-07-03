SHELL := /bin/bash
MAKELEVEL := docker

all: docker
# include ../../.makeinfo
include ../../.makecommands
include ../../.makeproject
-include .makeproject

TARGET ?= $(PROJECT).$(MODULE).$(COMPONENT)
TIMESTAMP ?= $(shell date +%Y%m%d%H%M%S)
GITHASH ?= $(shell ( git rev-parse HEAD 2>/dev/null || ( $(ECHO) 'unknown' ; exit 0 ) ) ) 
_GITHASH := $(shell ( $(ECHO) "$(GITHASH)" | sed 's/^ *//; s/  *$$//; s/  */\\|/g') )
# MODULES := $(shell find . -maxdepth 1 -mindepth 1 -type d -name "Dockerfile*" | sed 's/^..//' )

# include ../../.makeset
# include ../../.makesrc
include ../../.makebuild
include ../../.makedocker
# include ../../.makekubernetes

STEP_COMPONENTS := ms-measure ms-random requestlogger fe-weatherstation 

.PHONY: all docker docker-clean clean docker-distclean distclean $(STEP_COMPONENTS)

all clean docker-ls docker-clean docker:
	for dir in $(STEP_COMPONENTS) ; do \
	    TIMESTAMP="$(TIMESTAMP)" \
	    GITHASH="$(GITHASH)" \
	    CUSTOMER="$(CUSTOMER)" \
	    PROJECT="$(PROJECT)" \
		make -C  "$$dir" $@ ; \
	done

