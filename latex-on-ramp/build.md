# Building (Appendix E.3)

Day-to-day, one command does everything:

```sh
latexmk -pdf thesis.tex
```

What latexmk automates — worth understanding for debugging:

```sh
pdflatex thesis   # pass 1: writes .aux; refs still show ?
bibtex   thesis   # reads .aux, writes .bbl (bibliography)
pdflatex thesis   # pass 2: pulls in .bbl, resolves refs
pdflatex thesis   # pass 3: settles page numbers and ToC
```

Use `biber` instead of `bibtex` if you chose `biblatex` — latexmk detects this
and adjusts. Overleaf and most editors run latexmk for you on save.
