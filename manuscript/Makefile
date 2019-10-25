.PHONY: all manuscript toc

all: manuscript toc

manuscript:
	make -C cover cover.pdf
	make -C contents print.pdf
	mutool merge -o manuscript.pdf cover/cover.pdf contents/print.pdf

toc:
	make -C contents print.pdf
	mutool merge -o toc.pdf contents/print.pdf 11-14
