.PHONY: all clean
.SECONDARY:

VERSIONS := aurelien-ooms-resume-de-la-these-en-francais.pdf

LTX := pdflatex -interaction=nonstopmode -halt-on-error

BUILDDIR := _build

all: $(VERSIONS)

%.pdf: $(BUILDDIR)/%/paper.pdf
	cp $< $@
	killall -HUP mupdf || true

$(BUILDDIR)/aurelien-ooms-resume-de-la-these-en-francais/src.tex: $(shell texdeps aurelien-ooms-resume-de-la-these-en-francais.tex)

$(BUILDDIR)/%/src.tex:
	mkdir -p $(dir $@)
	cp $< $@

$(BUILDDIR)/%/paper.tex: $(BUILDDIR)/%/src.tex
	cp -t $(dir $@) $(shell texdeps $< | tail +2) $(static) 2>/dev/null || true
	cp $< $@

$(BUILDDIR)/%/paper.pdf: $(BUILDDIR)/%/paper.tex
	cd $(dir $@); \
		$(LTX) paper.tex

clean:
	rm -rf $(BUILDDIR)
	rm -f $(VERSIONS)
