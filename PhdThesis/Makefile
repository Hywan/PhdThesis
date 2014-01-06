SHELL = bash


all: tex

tex:
	@pdflatex \
		-output-directory Dist \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape PhdThesis.tex

bib: tex
	@bibtex Dist/PhdThesis.aux

figures: dots

dots:
	@for i in `find Source/Figure -type f -name '*.dot'`; do \
		echo -n `basename $$i`'…'; \
		dot2tex -tmath --codeonly --autosize $$i \
			> Dist/Figure/`basename $${i%.dot}.tex`; \
		echo -e '\b, ok'; \
	done

open:
	@open -g -F Dist/PhdThesis.pdf

clean:
	@find Dist -type f -not -name '.*' | xargs rm -f

reload: tex open

c: clean
o: open
r: reload
