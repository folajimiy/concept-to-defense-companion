# Extended LaTeX Guide

The printed Appendix F is a **survival guide**: a document that compiles, the
four mechanics you cannot avoid, the build pipeline, and the errors that eat
your last night. This file carries the material that was deliberately kept out
of print — the fuller worked examples, and the tooling notes that change faster
than a book can.

Everything here is optional. If your document compiles and your references
resolve, you do not need this file.

---

## 1. Cross-references, in full

```latex
\chapter{Design}\label{ch:design}
\section{Architecture}\label{sec:arch}

As shown in Chapter~\ref{ch:design}, the system has three tiers.
See Section~\ref{sec:arch} for details.
Table~\ref{tab:results} reports the outcome on page~\pageref{tab:results}.
```

Both references render with the right numbers and renumber themselves when
chapters move. Notes:

- **Label prefixes.** `ch:`, `sec:`, `fig:`, `tab:`, `eq:`, `lst:`, `app:`. A
  label's type should be obvious from its name.
- **The tilde** in `Chapter~\ref{...}` is a non-breaking space: it keeps
  "Chapter" and its number on the same line.
- **`\autoref`** (needs `hyperref`) prints the word for you — `\autoref{ch:design}`
  gives "Chapter 3". Convenient, but the word it chooses depends on the class;
  check the output before relying on it throughout.
- **`\pageref`** gives the page number. Useful in a long appendix, rarely
  otherwise.
- **`cleveref`** (`\cref`) is the power-user option: it handles ranges and lists
  ("Figures 2–4") automatically. Worth it in a long thesis, overkill in a short
  report.

## 2. Floats, in full

```latex
\begin{figure}[t]                 % [t] top, [b] bottom, [h] here-ish, [p] own page
  \centering
  \includegraphics[width=0.8\linewidth]{figures/arch.pdf}
  \caption{System architecture. Describe what the reader should notice.}
  \label{fig:arch}                % label AFTER caption, always
\end{figure}

\begin{table}[t]
  \centering
  \caption{Results on the held-out test set.}
  \label{tab:results}
  \begin{tabular}{lrr}
    \toprule
    Model & Accuracy & F1 \\
    \midrule
    Baseline & 0.71 & 0.68 \\
    Ours     & 0.84 & 0.83 \\
    \bottomrule
  \end{tabular}
\end{table}
```

- **`\label` must follow `\caption`.** Before it, the label captures the wrong
  counter and your reference points somewhere surprising.
- **Caption position by convention:** above tables, below figures.
- **`booktabs`** (`\toprule`/`\midrule`/`\bottomrule`) instead of `\hline`. No
  vertical rules — they add ink and remove clarity.
- **Vector vs raster:** PDF for diagrams and plots (sharp at any size); PNG for
  screenshots and other inherently raster images. Never JPEG for a diagram.
- **Float placement is not a bug.** If a figure lands a page later than you
  wrote it, that is LaTeX optimizing the page. Fight it only at the very end,
  and prefer `[t]`/`[b]` over `[h!]`.
- **`\linewidth` not fixed dimensions.** `width=0.8\linewidth` survives a
  margin change; `width=12cm` does not.

## 3. Citations, in full

```latex
% --- references.bib ---
@article{lecun2015deep,
  author  = {LeCun, Yann and Bengio, Yoshua and Hinton, Geoffrey},
  title   = {Deep learning},
  journal = {Nature},
  year    = {2015},
  volume  = {521},
  number  = {7553},
  pages   = {436--444},
  doi     = {10.1038/nature14539}
}
```

```latex
\usepackage[round]{natbib}        % preamble

Deep learning reshaped the field \citep{lecun2015deep}.  % (LeCun et al., 2015)
\citet{lecun2015deep} argue that\ldots                   % LeCun et al. (2015)

\bibliographystyle{plainnat}      % just before \end{document}
\bibliography{references}         % no .bib extension
```

- **natbib vs biblatex.** natbib is the classic route and what most publisher
  classes assume (this book's Springer class among them). biblatex + biber is
  more flexible and easier to customize. Pick one — mixing them fails in
  confusing ways.
- **Protect capitals** in titles with braces: `{DNA}`, `{LaTeX}`, `{FAIR}`.
  Otherwise many styles will down-case them.
- **Long digit strings** (article numbers like `e1005510`) can be mangled by
  styles that insert thousands separators. Break them with an empty group:
  `pages = {e1005{}510}`.
- **Per-chapter bibliographies** (what this book uses, because Springer requires
  each eBook chapter to be self-contained): `chapterbib`, plus a
  `\bibliography` in each chapter file, and one bibtex run per chapter `.aux`.

## 4. Build tooling notes

The printed appendix covers latexmk and the manual four-pass sequence. Extras:

- **`latexmkrc`** in the project root configures latexmk per project (output
  directory, engine, `-pvc` continuous preview).
- **No Perl?** latexmk needs it. On Windows, MiKTeX ships latexmk but not Perl —
  either install Strawberry Perl, or drive `pdflatex`/`bibtex` yourself from a
  script (this book's `build.ps1`/`build.sh` do exactly that, and are worth
  copying as a pattern).
- **Output directory.** `-output-directory=build` keeps aux files out of your
  source tree, but you must pre-create subdirectories matching any `\include`
  paths, or bibtex and makeindex will not find the `.aux` files.
- **Which engine.** `pdflatex` is the default and fastest. `lualatex`/`xelatex`
  handle Unicode and system fonts natively — needed for non-Latin scripts, and
  the usual fix for stubborn encoding errors. Your template may dictate the
  choice; follow it.
- **Version control.** Commit the `.tex`, `.bib`, and figure *sources*. Ignore
  `.aux`, `.log`, `.toc`, `.bbl`, `.blg`, `.out`, `.synctex.gz`, and the build
  directory. See this repository's `.gitignore` for a working list.

## 5. Editor setup (the fastest-aging section)

Deliberately brief, because this is where advice rots:

- **Overleaf** — nothing to install, runs latexmk on save, easy collaboration.
  The default choice if your institution provides it.
- **VS Code + LaTeX Workshop** — good local option; build-on-save, SyncTeX
  jump-to-source, integrated PDF preview.
- **TeXShop / TeXworks / TeXstudio** — bundled with the major distributions,
  perfectly adequate.
- **Distributions** — TeX Live (all platforms), MiKTeX (Windows, installs
  packages on demand), MacTeX (macOS).

Whichever you choose: enable **build on save** and **SyncTeX**, and stop there.
Time spent configuring an editor is not time spent writing a thesis.

## 6. Diagnosing a build that used to work

1. Read the **first** error, not the last.
2. Delete the aux files (`build/`, or `*.aux *.toc *.bbl`) and rebuild clean —
   a surprising share of "impossible" errors are stale aux state.
3. Bisect: comment out half the `\include` lines, rebuild, repeat.
4. Minimal reproduction: copy the failing construct into a fresh minimal
   document. If it fails there, you have something searchable.
5. Search the exact error text on `tex.stackexchange.com`. Someone has hit it.
