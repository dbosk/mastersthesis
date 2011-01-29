#.include pub.mk

DOCUMENTS=	thesis.pdf
DOCUMENT?=	thesis
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
.PHONY: all all-print clean struct print view run


.tex.pdf:
	${PDFTEX} ${TEXFLAGS} $<
	-${BIBTEX} ${<:.tex=}
	-${MAKEINDEX} ${<:.tex=.idx}
	while (${PDFTEX} ${TEXFLAGS} $<; \
		grep "Rerun to get cross" ${<:.tex=.log}) \
		do true; done
	${PDFTEX} ${TEXFLAGS} $<
.tex.dvi:
	${TEX} ${TEXFLAGS} $<
	-${BIBTEX} ${<:.tex=}
	-${MAKEINDEX} ${<:.tex=.idx}
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
	@rm -f *.log *.aux *.toc *.core *.o *~ *.bbl *.blg *.ind *.ilg \
		*.dvi *.ps *.out ${DOCUMENTS} *.idx

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

