$(info make v$(MAKE_VERSION))

.PHONY : all clean reset serve run ls inc incpat incmaj incmin

VENDOR := 0
LEARNING := 0
REWE := 1

MAJOR := $(shell cut -d. -f1 ../VERSION)
MINOR := $(shell cut -d. -f2 ../VERSION)
PATCH := $(shell cut -d. -f3 ../VERSION)

MKDIR := mkdir -p
RMDIR := rm -rf
NCOPY = $(MKDIR) dist/$(dir $2) && cp -t dist/ $1
FCOPY = $(MKDIR) dist/$(dir $2) && cp -t dist/$(dir $2) $1
DCOPY = $(MKDIR) dist/$2 && cp -t dist/ -r $1

SERVE := php -S 0.0.0.0:8002
RENDER := python tools/render.py

include main.mk
include deps.mk

dist/%.html : %.html | dist ; $(RENDER) -o dist/$< $<
dist/%.css : %.css | dist ; $(RENDER) -o dist/$< $<

dist/% : % | dist ; $(call FCOPY,$<,$<)
dist/%/ : % | dist ; $(call DCOPY,$<,$<)

dist : ; $(MKDIR) dist/
build : ; $(MKDIR) build/

clean : ; $(RMDIR) build/
reset : | clean ; $(RMDIR) dist/

serve : all ; cd dist && rm -f index.php && $(SERVE)
run : all dist/index.php ; cd dist && $(SERVE)

ls : all ; @find dist/ -type f | sort

incmaj : ; @echo -n $(shell echo $(MAJOR)+1 | bc).$(MINOR).$(PATCH) > ../VERSION
incmin : ; @echo -n $(MAJOR).$(shell echo $(MINOR)+1 | bc).$(PATCH) > ../VERSION
incpat : ; @echo -n $(MAJOR).$(MINOR).$(shell echo $(PATCH)+1 | bc) > ../VERSION
inc : incpat ; @cat ../VERSION

deploy : ; @../.local/deploy.sh
