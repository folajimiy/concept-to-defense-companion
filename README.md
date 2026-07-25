# Computing Projects from Concept to Defense — Companion Repository

Code samples and starter files accompanying *Computing Projects from Concept to
Defense: A Guide to Capstone and Thesis Projects in Software, AI, and
Cybersecurity* (Longe-Folajimi, Deligiannidis & Othman).

## Contents

| Path | What it is | Book reference |
|---|---|---|
| `latex-on-ramp/thesis.tex` | The smallest thesis document that compiles — **identical to the printed listing** | Appendix F.1 |
| `latex-on-ramp/thesis-with-citations.tex` | The minimal thesis extended with natbib citations and cross-references | Appendices F.2, F.4 |
| `latex-on-ramp/references.bib` | The sample bibliography file | Appendix F.4 |
| `latex-on-ramp/build.md` | The build pipeline (latexmk-first) | Appendix F.5 |
| `templates/` | Editable fill-in versions of every Appendix A template: proposal, feasibility triage, literature matrix, risk register, ADR, claims–evidence table, team charter, contribution log, AI-use disclosure, semester plan | Appendix A |

The self-assessment rubrics are in **Appendix B** of the book.

## Using the LaTeX samples

```sh
cd latex-on-ramp
latexmk -pdf thesis.tex     # one command; runs all needed passes
```

Every sample compiles as printed in the book. If a build misbehaves, Appendix
F.6 ("The Errors That Eat Your Last Night") is the debugging guide.
