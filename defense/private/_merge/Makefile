.PHONY: all clean

BITMAPS=src/ksum-algorithm-bitmaps.ipe src/3pol-algorithm-bitmaps.ipe src/order-type-encoding-bitmaps.ipe src/3sum-encoding-bitmaps.ipe
PAGES=src/ksum-algorithm-pages.ipe src/3pol-algorithm-pages.ipe src/order-type-encoding-pages.ipe src/3sum-encoding-pages.ipe

all: merged.ipe

src/bitmaps.ipe: $(BITMAPS)
	cat $^ > $@

src/pages.ipe: $(PAGES)
	cat $^ > $@

merged.ipe: src/begin.ipe src/bitmaps.ipe src/pages.ipe src/end.ipe
	cat $^ > $@

clean:
	rm merged.ipe src/bitmaps.ipe src/pages.ipe
