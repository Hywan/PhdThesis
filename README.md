My PhdThesis. This is a “subtree” of a private SVN repository.

# Compile

How to compile the document? Deadly simple:

    $ cd PhdThesis
    $ make bib && make && make
    $ make open # on Darwin

A PDF is produced in `PhdThesis/Dist/`.

To compile the “print” version:

    $ FORMAT=.print make bib && make && make
    $ FORMAT=.print make open

# Requirements

dot 2.28, not higher (dot2tex has strange issues else).

# Note

This document is in french and the property of Ivan Enderlin © 2011-2014.