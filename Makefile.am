###############################################################################
## Makefile for Highland Puebla Nahuatl--Zacatlán-Ahuacatlán-Tepetzintla Nahuatl
###############################################################################

LANG1=azz
LANG2=nhi
PREFIX1=$(LANG1)-$(LANG2)
PREFIX2=$(LANG2)-$(LANG1)

BASENAME=apertium-$(PREFIX1)

TARGETS_DIR1 =		\
	$(PREFIX1).rlx.bin	\
	$(PREFIX1).autobil.bin	\
	$(PREFIX1).autolex.bin	\
	$(PREFIX1).autogen.hfst \
	$(PREFIX1).autoseq.bin	\
	$(PREFIX1).revautoseq.bin	\
	$(PREFIX1).t1x.bin	\
	$(PREFIX1).t2x.bin	\
	$(PREFIX1).t3x.bin

TRIMMED_TARGETS_DIR1 = \
	$(PREFIX1).automorf.bin

UNTRIMMED_TARGETS_DIR1 = \
	$(PREFIX1).automorf-untrimmed.bin

TARGETS_DIR2 =		\
	$(PREFIX2).rlx.bin	\
	$(PREFIX2).autobil.bin	\
	$(PREFIX2).autolex.bin	\
	$(PREFIX2).autoseq.bin	\
	$(PREFIX2).revautoseq.bin	\
	$(PREFIX2).autogen.hfst \
	$(PREFIX2).t1x.bin	\
	$(PREFIX2).t2x.bin	\
	$(PREFIX2).t3x.bin

TRIMMED_TARGETS_DIR2 = \
	$(PREFIX2).automorf.bin

UNTRIMMED_TARGETS_DIR2 = \
	$(PREFIX2).automorf-untrimmed.bin

TARGETS_COMMON = $(TARGETS_DIR1) $(TRIMMED_TARGETS_DIR1) \
				 $(TARGETS_DIR2) $(TRIMMED_TARGETS_DIR2)

SOURCES = \
	$(BASENAME).$(PREFIX1).t1x			\
	$(BASENAME).$(PREFIX1).t2x			\
	$(BASENAME).$(PREFIX1).t3x			\
	$(BASENAME).$(PREFIX2).t1x			\
	$(BASENAME).$(PREFIX2).t2x			\
	$(BASENAME).$(PREFIX2).t3x			\
	$(BASENAME).$(PREFIX1).dix			\
	$(BASENAME).$(PREFIX1).lrx			\
	$(BASENAME).$(PREFIX2).lrx

azz-nhi: $(TARGETS_DIR1) $(TRIMMED_TARGETS_DIR1)
nhi-azz: $(TARGETS_DIR2) $(TRIMMED_TARGETS_DIR2)
untrimmed: $(TARGETS_DIR1) $(UNTRIMMED_TARGETS_DIR1) $(TARGETS_DIR2) $(UNTRIMMED_TARGETS_DIR2)

# This include defines goals for install-modes, .deps/.d, autobil.prefixes and .mode files:
@ap_include@

###############################################################################
## Monolingual transducers
###############################################################################

# -------------------
# Highland Puebla Nahuatl analysis:
# -------------------

.deps/$(LANG1).automorf.hfst: $(AP_SRC1)/$(LANG1).automorf.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

$(PREFIX1).automorf-untrimmed.bin: $(AP_SRC1)/$(LANG1).automorf.att.gz
	@$(ZCAT) $< > $@.att
	lt-comp lr $@.att $@
	@rm $@.att

$(PREFIX1).automorf.bin: .deps/$(LANG1).automorf.hfst .deps/$(PREFIX1).autobil.prefixes
	hfst-compose-intersect -1 .deps/$(LANG1).automorf.hfst -2 .deps/$(PREFIX1).autobil.prefixes -o .deps/$(PREFIX1).automorf.trimmed
	hfst-fst2txt -i .deps/$(PREFIX1).automorf.trimmed -o .deps/$(PREFIX1).automorf.trimmed.att
	lt-comp lr .deps/$(PREFIX1).automorf.trimmed.att $@

# -------------------
# Highland Puebla Nahuatl generation:
# ---------------------

$(PREFIX2).autogen.hfst: $(AP_SRC1)/$(LANG1).autogen.hfst
	cp $< $@


$(PREFIX2).autogen.bin: $(AP_SRC1)/$(LANG1).autogen.bin
	cp $< $@

# -------------------
# Zacatlán-Ahuacatlán-Tepetzintla Nahuatl analysis:
# -------------------

.deps/$(LANG2).automorf.hfst: $(AP_SRC2)/$(LANG2).automorf.att.gz .deps/.d
	$(ZCAT) $< | hfst-txt2fst > $@

$(PREFIX2).automorf-untrimmed.bin: $(AP_SRC2)/$(LANG2).automorf.att.gz
	@$(ZCAT) $< > $@.att
	lt-comp lr $@.att $@
	@rm $@.att

$(PREFIX2).automorf.bin: .deps/$(LANG2).automorf.hfst .deps/$(PREFIX2).autobil.prefixes
	hfst-compose-intersect -1 .deps/$(LANG2).automorf.hfst -2 .deps/$(PREFIX2).autobil.prefixes -o .deps/$(PREFIX2).automorf.trimmed
	hfst-fst2txt -i .deps/$(PREFIX2).automorf.trimmed -o .deps/$(PREFIX2).automorf.trimmed.att
	lt-comp lr .deps/$(PREFIX2).automorf.trimmed.att $@

# -------------------
# Zacatlán-Ahuacatlán-Tepetzintla Nahuatl generation:
# ---------------------

$(PREFIX1).autogen.hfst: $(AP_SRC2)/$(LANG2).autogen.hfst
	cp $< $@


$(PREFIX1).autogen.bin: $(AP_SRC2)/$(LANG2).autogen.bin
	cp $< $@

###############################################################################
## Tagger models
###############################################################################

$(PREFIX1).prob: $(AP_SRC1)/$(LANG1).prob
	cp $< $@

$(PREFIX2).prob: $(AP_SRC2)/$(LANG2).prob
	cp $< $@

###############################################################################
## Disambiguation rules
###############################################################################


$(PREFIX1).rlx.bin: $(AP_SRC1)/$(LANG1).rlx.bin
	cp $< $@


$(PREFIX2).rlx.bin: $(AP_SRC2)/$(LANG2).rlx.bin
	cp $< $@

###############################################################################
## Lexical sequences
###############################################################################

$(PREFIX1).autoseq.bin: $(BASENAME).$(PREFIX1).lsx
	lsx-comp lr $< $@

$(PREFIX2).autoseq.bin: $(BASENAME).$(PREFIX2).lsx
	lsx-comp lr $< $@

$(PREFIX1).revautoseq.bin: $(BASENAME).$(PREFIX2).lsx
	lsx-comp rl $< $@

$(PREFIX2).revautoseq.bin: $(BASENAME).$(PREFIX1).lsx
	lsx-comp rl $< $@

###############################################################################
## Bilingual dictionaries
###############################################################################

$(PREFIX1).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp lr $< $@

$(PREFIX2).autobil.bin: $(BASENAME).$(PREFIX1).dix
	apertium-validate-dictionary $<
	lt-comp rl $< $@

###############################################################################
## Lexical selection rules
###############################################################################

$(PREFIX1).autolex.bin: $(BASENAME).$(PREFIX1).lrx
#	apertium-validate-lrx $<
	lrx-comp $< $@

$(PREFIX2).autolex.bin: $(BASENAME).$(PREFIX2).lrx
#	apertium-validate-lrx $<
	lrx-comp $< $@


###############################################################################
## Structural transfer rules
###############################################################################


$(PREFIX1).t1x.bin: $(BASENAME).$(PREFIX1).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t2x.bin: $(BASENAME).$(PREFIX1).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX1).t3x.bin: $(BASENAME).$(PREFIX1).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@


$(PREFIX2).t1x.bin: $(BASENAME).$(PREFIX2).t1x
	apertium-validate-transfer $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t2x.bin: $(BASENAME).$(PREFIX2).t2x
	apertium-validate-interchunk $<
	apertium-preprocess-transfer $< $@

$(PREFIX2).t3x.bin: $(BASENAME).$(PREFIX2).t3x
	apertium-validate-postchunk $<
	apertium-preprocess-transfer $< $@

###############################################################################
## Postgeneration dictionaries
###############################################################################

$(PREFIX1).autopgen.bin: $(AP_SRC2)/$(LANG2).autopgen.bin
	cp $< $@

$(PREFIX2).autopgen.bin: $(AP_SRC1)/$(LANG1).autopgen.bin
	cp $< $@

###############################################################################
## Distribution
###############################################################################

EXTRA_DIST=	$(SOURCES) modes.xml

###############################################################################
## Installation
###############################################################################

apertium_azz_nhidir=$(prefix)/share/apertium/$(BASENAME)/

# Files which should be installed to $(prefix)/share/apertium/$(BASENAME)/:
apertium_azz_nhi_DATA= $(TARGETS_COMMON) $(SOURCES)


# All modes are created by the same goal, listing several will just give problems with make -j2
noinst_DATA=modes/$(PREFIX1).mode

install-data-local: install-modes
uninstall-local: uninstall-modes

###############################################################################
## Cleanup
###############################################################################

CLEANFILES = $(TARGETS_COMMON) $(UNTRIMMED_TARGETS_DIR1) $(UNTRIMMED_TARGETS_DIR2)
clean-local:
	-rm -rf .deps modes

###############################################################################
## Test
###############################################################################

test: all
	apertium-regtest test
