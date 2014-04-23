SHELL = bash


all: tex

tex: | Dist/Page/Covers.pdf
	@pdflatex \
		-output-directory Dist \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape PhdThesis.tex;

Dist/Page/Covers.pdf:
	@pdflatex \
		-output-directory Dist/Page \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape Source/Page/Covers.tex;

bib: | Dist/PhdThesis.aux
	@bibtex Dist/PhdThesis.aux;

Dist/PhdThesis.aux: tex

figures: dots umls

dots:
	@for i in `find Source/Figure -type f -name '*.dot'`; do \
		echo -n `basename $$i`'…'; \
		dot2tex -fpgf -tmath --codeonly --autosize $$i \
			| sed -e 's/pgfsetcolor{black}/pgfsetcolor{dot}/' \
			      -e 's/pgfsetstrokecolor{strokecol}/pgfsetstrokecolor{dotstroke}/' \
			      -e '/definecolor/ d' \
			> Dist/Figure/`basename $${i%.dot}.tex`; \
		echo -e '\b, ok'; \
	done

dot:
	@dot2tex -fpgf -tmath --codeonly --autosize $(FILE) \
		| sed -e 's/pgfsetcolor{black}/pgfsetcolor{dot}/' \
		      -e 's/pgfsetstrokecolor{strokecol}/pgfsetstrokecolor{dotstroke}/' \
		      -e '/definecolor/ d' \
		> Dist/Figure/`basename $(FILE:%.dot=%.tex)`;

umls: | External/plantuml.jar
	@for i in `find Source/Figure -type f -name '*.uml'`; do \
		echo -n `basename $$i`'…'; \
		cat $$i \
			| java -jar External/plantuml.jar \
				   -S"monochrome=true" \
				   -S"defaultFontName=CMU Serif" \
				   -S"classFontName=CMU Typewriter Text" \
				   -S"classAttributeFontName=CMU Typewriter Text" \
				   -S"packageFontName=CMU Typewriter Text" \
				   -S"packageBackgroundColor=#FFFFFF" \
				   -S"packageBorderColor=#CCCCCC" \
				   -pipe \
				   -teps \
			| epstopdf --filter --outfile=Dist/Figure/`basename $${i%.uml}.pdf`; \
		echo -e '\b, ok'; \
	done

uml: | External/plantuml.jar
	@cat $(FILE) \
		| java -jar External/plantuml.jar \
			   -S"monochrome=true" \
			   -S"defaultFontName=CMU Serif" \
			   -S"classFontName=CMU Typewriter Text" \
			   -S"classAttributeFontName=CMU Typewriter Text" \
			   -S"packageFontName=CMU Typewriter Text" \
			   -S"packageBackgroundColor=#FFFFFF" \
			   -S"packageBorderColor=#CCCCCC" \
			   -pipe \
			   -teps \
		| epstopdf --filter --outfile=Dist/Figure/`basename $(FILE:%.uml=%.pdf)`;

External/plantuml.jar:
	@curl -L http://downloads.sourceforge.net/project/plantuml/plantuml.jar \
		> External/plantuml.jar;

open:
	@open -g -F Dist/PhdThesis.pdf;

open-new:
	@open -g -F -n Dist/PhdThesis.pdf;

clean:
	@echo -e 'Dist\nDist/Page' \
		| xargs -I… find … -type f -maxdepth 1 \
			-not -name '.*' \
			-not -name '*.tex' \
			-not -name '*.pdf' \
		| xargs rm -f;

distclean:
	@find Dist -type f -not -name '.*' | xargs rm -f

reload: tex open

c: clean
dc: distclean
o: open
O: open-new
r: reload
