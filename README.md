# Computing Projects from Concept to Defense — Companion Repository

Code samples and starter files accompanying *Computing Projects from Concept to
Defense: A Guide to Capstone Projects and Theses in Software, AI,
Cybersecurity, and Related Fields* (Longe-Folajimi, Deligiannidis & Othman).

## Contents

| Path | What it is | Book reference |
|---|---|---|
| `latex-on-ramp/thesis.tex` | The smallest thesis document that compiles — **identical to the printed listing** | Appendix E.1 |
| `latex-on-ramp/thesis-with-citations.tex` | The minimal thesis extended with natbib citations and cross-references | Appendix E.2 |
| `latex-on-ramp/references.bib` | The sample bibliography file | Appendix E.2 |
| `latex-on-ramp/build.md` | The build pipeline (latexmk-first) | Appendix E.3 |
| `latex-on-ramp/extended-guide.md` | **The material deliberately kept out of print**: full worked examples of cross-references, floats, and citations; build tooling notes; editor setup; how to diagnose a build that used to work | extends Appendix E |
| `templates/` | Editable fill-in versions of every Appendix A template: proposal, feasibility triage, literature matrix, risk register, ADR, claims–evidence table, team charter, contribution log, AI-use disclosure, semester plan | Appendix A |
| `templates/master-checklists.md` | **Printable runbook**: every end-of-chapter checklist compiled into one per-stage tick-list (52 items, 8 stages). Lives here rather than in print, so the book does not carry the same lists twice | end-of-chapter checklists |
| `figures/` | All eight book figures as 600 dpi PNGs (lifecycle ribbon, claims–evidence chain, semester plan, architecture views, vertical slices, evaluation-method selector, intro–conclusion mirror, defense time dial) — free to reuse in slides and course materials under the repository license | Figures 1.1–8.1 |

The self-assessment diagnostics are in **Appendix B** of the book.

## Using the LaTeX samples

```sh
cd latex-on-ramp
latexmk -pdf thesis.tex     # one command; runs all needed passes
```

Every sample compiles as printed in the book. If a build misbehaves, Appendix
F.6 ("The Errors That Eat Your Last Night") is the debugging guide.
