#.include pub.mk

DOCUMENTS=	kompendium.pdf
DOCUMENT?=	kompendium
SOURCES=	kompendium kursplanering intro frontmatter backmatter \
			heltalen naturliga reella logik mangder talsystem preface
PAPER?=		a4

TEX=		latex
PDFTEX=		pdflatex
DVIPS=		dvips
TEXFLAGS=	
MAKEINDEX=	makeindex
BIBTEX=		bibtex
PDFVIEW?=	evince
PRINT?=		lpr


.SUFFIXES: .tex .pdf .ps .dvi .print
.PHONY: all all-print clean struct print view run clean-all


.tex.pdf:
	${PDFTEX} ${TEXFLAGS} $<
	-${BIBTEX} ${<:.tex=}
	-${MAKEINDEX} ${<:.tex=.idx}
	-${MAKEINDEX} ${<:.tex=.nlo} -s nomencl.ist -o ${<:.tex=.nls}
	while (${PDFTEX} ${TEXFLAGS} $<; \
		grep "Rerun to get cross" ${<:.tex=.log}) \
		do true; done
	${PDFTEX} ${TEXFLAGS} $<
.tex.dvi:
	${TEX} ${TEXFLAGS} $<
	-${BIBTEX} ${<:.tex=}
	-${MAKEINDEX} ${<:.tex=.idx}
	-${MAKEINDEX} ${<:.tex=.nlo} -s nomencl.ist -o ${<:.tex=.nls}
	while (${TEX} ${TEXFLAGS} $<; \
		grep "Rerun to get cross" ${<:.tex=.log}) \
		do true; done
	${TEX} ${TEXFLAGS} $<
.dvi.ps:
	${DVIPS} -t ${PAPER} -o $@ $<

.ps.print:
	${PRINT} $<


all: ${DOCUMENTS}
all-print: ${DOCUMENTS:.pdf=.print}

clean:
	@rm -f \
		${DOCUMENT}.log \
		${DOCUMENT}.aux \
		${DOCUMENT}.toc \
		${DOCUMENT}.bbl \
		${DOCUMENT}.blg \
		${DOCUMENT}.ind \
		${DOCUMENT}.ilg \
		${DOCUMENT}.dvi \
		${DOCUMENT}.ps \
		${DOCUMENT}.out \
		${DOCUMENT}.pdf \
		${DOCUMENT}.idx \
		${DOCUMENT}.nls \
		${DOCUMENT}.nlo \
		${DOCUMENT}.lof \
		${DOCUMENT}.lot \
		*.core *.o *~ *.out 
clean-all:
	@for d in ${SOURCES}; do \
		${MAKE} DOCUMENT=$$d clean; \
		done

struct:
	@for f in ${DOCUMENTS}; do \
		touch $$f; \
		done

print: ${DOCUMENT}.print

view: ${DOCUMENT}.pdf
	${PDFVIEW} ${DOCUMENT}.pdf

run: view

todo:
	@grep -e TODO -e todo -e XXX * | grep -v '^Makefile:'
