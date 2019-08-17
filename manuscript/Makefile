.PHONY: manuscript

manuscript:
	make -C cover cover.pdf
	make -C contents print.pdf
	mutool merge -o manuscript.pdf cover/cover.pdf contents/print.pdf
