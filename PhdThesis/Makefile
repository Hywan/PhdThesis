all: tex

tex:
	@pdflatex \
		-output-directory Dist/ \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape PhdThesis.tex

bib: tex
	@bibtex Dist/PhdThesis.aux

open:
	@open -g -F Dist/PhdThesis.pdf

clean:
	@rm -f Dist/*

reload: tex open

c: clean
o: open
r: reload
