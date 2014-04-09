SHELL = bash


all: tex

tex:
	@pdflatex \
		-output-directory Dist \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape PhdThesis.tex;

bib: tex
	@bibtex Dist/PhdThesis.aux;

figures: dots

dots:
	@for i in `find Source/Figure -type f -name '*.dot'`; do \
		echo -n `basename $$i`'…'; \
		dot2tex -tmath --codeonly --autosize $$i \
			> Dist/Figure/`basename $${i%.dot}.tex`; \
		echo -e '\b, ok'; \
	done

dot:
	@dot2tex -tmath --codeonly --autosize $(FILE) \
		> Dist/Figure/`basename $(FILE:%.dot=%.tex)`;

umls: | External/plantuml.jar
	@for i in `find Source/Figure -type f -name '*.uml'`; do \
		echo -n `basename $$i`'…'; \
		cat $$i \
			| java -jar External/plantuml.jar -pipe -teps \
			| epstopdf --filter --outfile=Dist/Figure/`basename $${i%.uml}.pdf`; \
		echo -e '\b, ok'; \
	done

uml: | External/plantuml.jar
	@cat $(FILE) \
		| java -jar External/plantuml.jar -pipe -teps \
		| epstopdf --filter --outfile=Dist/Figure/`basename $(FILE:%.uml=%.pdf)`;

External/plantuml.jar:
	@curl -L http://downloads.sourceforge.net/project/plantuml/plantuml.jar \
		> External/plantuml.jar;

open:
	@open -g -F Dist/PhdThesis.pdf;

open-new:
	@open -g -F -n Dist/PhdThesis.pdf;

clean:
	@find Dist -type f -maxdepth 1 \
		-not -name '.*' \
		-not -name '*.tex' | xargs rm -f;

distclean:
	@find Dist -type f -not -name '.*' | xargs rm -f

reload: tex open

c: clean
dc: distclean
o: open
O: open-new
r: reload
