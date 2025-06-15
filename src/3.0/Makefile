# makefile for v3.0
# TOP contains the relative path of this Makefile
TOP := $(dir $(lastword $(MAKEFILE_LIST)))
TOOL_DIR := $(TOP)../tool

#TODO overfull, underfull rules...

Lang=$(notdir $(CURDIR))
FILE_MINI=grisbi-manuel
FILE=00-$(FILE_MINI)-$(Lang)
FILE_OUTPUT=$(FILE_MINI)_$(VERSION)-$(Lang)
# to have the same format as the 'Example' files in sourceforge.net
VERSION=3.0
INCLUDES:=$(wildcard [0-3][0-9]-grisbi-manuel-*.tex) $(wildcard tool/*.pl) Makefile

.PHONY: pdf

all:
	@echo '********************************************************'
	@echo '* BUILDING THE IMAGES PDF VERSION'
	@echo '********************************************************'
	$(MAKE) clean
	$(MAKE) pdf
	@echo '********************************************************'
	@echo '* BUILDING THE IMAGES HTML VERSION'
	@echo '********************************************************'
	$(MAKE) clean
	$(MAKE) html_pandoc


pdf: $(FILE).pdf
	$(MAKE) pdf_indexglossary
	$(MAKE) $^
	test -d pdf || mkdir pdf
	@echo See copy in pdf/
	cp $(FILE).pdf pdf/$(FILE_OUTPUT).pdf

pdf_indexglossary: $(FILE).pdf
	texindy -C utf8 $(FILE).idx
	makeglossaries $(FILE).glo
	@echo Remove pdf file
	rm $(FILE).pdf
	@echo Compile again in pdf by make pdf to get index and glossary.

html_pandoc:
	rm -rf html
	mkdir html
	pandoc --embed-resources --standalone --toc --output=html/$(FILE_OUTPUT).html $(FILE).tex

$(FILE).pdf: $(FILE).tex $(INCLUDES)
	@echo Generating $@...
	pdflatex --interaction=batchmode $(FILE).tex || \
	  (grep -A5 '^! ' $(FILE).log; exit 1)
	pdflatex --interaction=batchmode $(FILE).tex
	pdflatex --interaction=batchmode $(FILE).tex
	thumbpdf $(FILE).pdf
	pdflatex --interaction=batchmode $(FILE).tex
	grep Warning $(FILE).log || true
	@echo one.

clean:
	@echo -n Cleaning intermediate files...
	rm -f $(FILE).pdf
	#la commande rm -f *.{aux,log,out,toc,tpt,bak,haux,lof,htoc,idx,ind,xdy,glg,glo,gls} ne fonctionne pas
	rm -f *.aux *.bak *.glg *.glo *.gls *.idx *.ind *.lof *.log *.out *.toc *.tpt *.xdy
	@echo "  done."

distclean: clean
	@echo -n Cleaning result files...
	rm -f $(FILE){-img,-txt,}.{pdf,html,shtml}
	rm -Rf html pdf
	@echo "  done."
