.PHONY: all figures clean nuke
.SECONDARY:

#VERSIONS := dev.pdf draft.pdf print.pdf proof.pdf
VERSIONS := print.pdf

LTX := pdflatex -interaction=nonstopmode -halt-on-error

FIGSDIR := figures

BUILDDIR := _build

FIGURES := $(shell sed -E -e 's/:.*$$//' Makefile | grep -E '^figures/[a-z0-9-]*\.pdf$$')

STATIC := $(shell find static -type f)

all: $(VERSIONS)

figures: $(FIGURES)

figures/simplex.pdf: figures/simplex.ipe
figures/simplex.pdf: view=1-1

figures/partition.pdf: figures/figures-3pol.ipe
figures/partition.pdf: view=1-1
figures/intersected-cells.pdf: figures/figures-3pol.ipe
figures/intersected-cells.pdf: view=1-2
figures/ab-pairs.pdf: figures/figures-3pol.ipe
figures/ab-pairs.pdf: view=1-3
figures/zero-set.pdf: figures/figures-3pol.ipe
figures/zero-set.pdf: view=2-1
figures/decomposition.pdf: figures/figures-3pol.ipe
figures/decomposition.pdf: view=2-2
figures/recursion.pdf: figures/figures-3pol.ipe
figures/recursion.pdf: view=2-3
figures/cells.pdf: figures/figures-3pol.ipe
figures/cells.pdf: view=2-4
figures/deltab.pdf: figures/figures-3pol.ipe
figures/deltab.pdf: view=3-1

figures/pappus.pdf: figures/figures-types.ipe
figures/pappus.pdf: view=1-1
figures/pappus-realizable.pdf: figures/figures-types.ipe
figures/pappus-realizable.pdf: view=1-2
figures/pappus-abstract.pdf: figures/figures-types.ipe
figures/pappus-abstract.pdf: view=1-3
figures/perles.pdf: figures/figures-types.ipe
figures/perles.pdf: view=2-1
figures/permutation-0.pdf: figures/figures-types.ipe
figures/permutation-0.pdf: view=3-1
figures/permutation-1.pdf: figures/figures-types.ipe
figures/permutation-1.pdf: view=3-2
figures/duality.pdf: figures/figures-types.ipe
figures/duality.pdf: view=4-1
figures/a-zone-in-the-plane.pdf: figures/figures-types.ipe
figures/a-zone-in-the-plane.pdf: view=5-1

figures/space-analysis.pdf: figures/schematics.ipe
figures/space-analysis.pdf: view=1-1
figures/layout.pdf: figures/schematics.ipe
figures/layout.pdf: view=2-1
figures/trace.pdf: figures/schematics.ipe
figures/trace.pdf: view=3-1
figures/new-trace.pdf: figures/schematics.ipe
figures/new-trace.pdf: view=4-1

figures/bottom-vertex-triangulation.pdf: figures/arrangements.ipe
figures/bottom-vertex-triangulation.pdf: view=1-1

figures/longstory.pdf: figures/art.ipe
figures/longstory.pdf: view=1-1
figures/geometry.pdf: figures/art.ipe
figures/geometry.pdf: view=2-1
figures/algorithms.pdf: figures/art.ipe
figures/algorithms.pdf: view=3-1
figures/data-structures.pdf: figures/art.ipe
figures/data-structures.pdf: view=4-1

figures/landscape.pdf: figures/landscape.ipe
figures/landscape.pdf: view=1-1

figures/%.pdf:
	ipetoipe -pdf -export -view $(view) $^ $@


%.pdf: $(BUILDDIR)/%/paper.pdf
	cp $< $@
	killall -HUP mupdf || true

#$(BUILDDIR)/dev/src.tex: $(shell texdeps dev.tex)
$(BUILDDIR)/dev/paper.tex: static=$(STATIC)
#$(BUILDDIR)/draft/src.tex: $(shell texdeps draft.tex)
$(BUILDDIR)/draft/paper.tex: static=$(STATIC)
$(BUILDDIR)/print/src.tex: $(shell texdeps print.tex)
$(BUILDDIR)/print/paper.tex: static=$(STATIC)
#$(BUILDDIR)/proof/src.tex: $(shell texdeps proof.tex)
$(BUILDDIR)/proof/paper.tex: static=$(STATIC)

$(BUILDDIR)/%/src.tex:
	mkdir -p $(dir $@)
	flatex --include_sty $< $@

$(BUILDDIR)/%/src-shake.tex: $(BUILDDIR)/%/src.tex
	texshake < $<  > $@

$(BUILDDIR)/%/src-trim.tex: $(BUILDDIR)/%/src.tex
	textrim < $< > $@

$(BUILDDIR)/%/paper.bib: $(BUILDDIR)/%/src-trim.tex
	texcites < $< | \
		bibfilter $(shell sed -E -n 's/\\bibliography\{(.*)\}/\1/p' $<).bib \
		> $(dir $@)paper.bib

$(BUILDDIR)/%/paper.tex: $(BUILDDIR)/%/src.tex $(BUILDDIR)/%/paper.bib
	cp -t $(dir $@) $(shell texdeps $< | tail +2) $(static) 2>/dev/null || true
	sed -E -e 's:\\bibliography\{.*\}:\\bibliography{paper}:g' \
		   -e 's:\{figures/:{:g' $< > $@

$(BUILDDIR)/%/paper.pdf: $(BUILDDIR)/%/paper.tex
	cd $(dir $@); \
		$(LTX) paper.tex; \
		bibtex paper; \
		$(LTX) paper.tex; \
		$(LTX) paper.tex

clean:
	rm -rf $(BUILDDIR)
	rm -f $(VERSIONS) $(VERSIONS:.pdf=.zip)

nuke: clean
	rm -f $(FIGURES)
