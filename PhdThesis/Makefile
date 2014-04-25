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

summary:
	@pdflatex \
		-output-directory Dist/Page \
		-output-format pdf \
		-interaction errorstopmode \
		-halt-on-error \
		-shell-escape Source/Page/Summary.tex;

bib-summary: | Dist/Page/Summary.aux
	@bibtex Dist/Page/Summary.aux;

Dist/Page/Summary.aux: summary

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
				   -S"defaultFontName=CMU Serif" \
				   -S"classArrowColor=#b58900" \
				   -S"classAttributeFontName=CMU Typewriter Text" \
				   -S"classBackgroundColor=#fdf6e3" \
				   -S"classBorderColor=#b58900" \
				   -S"classFontColor=#002b36" \
				   -S"classFontName=CMU Typewriter Text" \
				   -S"packageBackgroundColor=#fdf6e3" \
				   -S"packageBorderColor=#657b83" \
				   -S"packageFontName=CMU Typewriter Text" \
				   -S"stereotypeABackgroundColor=#eee8d5" \
				   -S"stereotypeCBackgroundColor=#eee8d5" \
				   -S"stereotypeEBackgroundColor=#eee8d5" \
				   -S"stereotypeIBackgroundColor=#eee8d5" \
				   -pipe \
				   -teps \
			| epstopdf --filter --outfile=Dist/Figure/`basename $${i%.uml}.pdf`; \
		echo -e '\b, ok'; \
	done

uml: | External/plantuml.jar
	@cat $(FILE) \
		| java -jar External/plantuml.jar \
			   -S"defaultFontName=CMU Serif" \
			   -S"classArrowColor=#b58900" \
			   -S"classAttributeFontName=CMU Typewriter Text" \
			   -S"classBackgroundColor=#fdf6e3" \
			   -S"classBorderColor=#b58900" \
			   -S"classFontColor=#002b36" \
			   -S"classFontName=CMU Typewriter Text" \
			   -S"packageBackgroundColor=#fdf6e3" \
			   -S"packageBorderColor=#657b83" \
			   -S"packageFontName=CMU Typewriter Text" \
			   -S"stereotypeABackgroundColor=#eee8d5" \
			   -S"stereotypeCBackgroundColor=#eee8d5" \
			   -S"stereotypeEBackgroundColor=#eee8d5" \
			   -S"stereotypeIBackgroundColor=#eee8d5" \
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
