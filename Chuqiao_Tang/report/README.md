# Report — MTH321 Project 2, Group C

The full written report, 23 pages, with its LaTeX source so that every printed number can be
traced back to the table that produced it.

| | |
|---|---|
| Compiled PDF | [`MTH321_Project2_GroupC_report_v8.pdf`](MTH321_Project2_GroupC_report_v8.pdf) — 23 pages, 2.3 MB |
| Entry point | [`main.tex`](main.tex) |
| Engine | `tectonic -X compile main.tex --outdir build` (pdfLaTeX-compatible preamble) |
| Build check | 0 errors, 0 overfull boxes, 0 underfull boxes, 23 pages |

A clean rebuild from this folder alone reproduces the same 23 pages:

```bash
mkdir -p build
tectonic -X compile main.tex --outdir build
```

## What is in here

| Path | Contents |
|---|---|
| `main.tex` | All prose, tables and float placement for the report |
| `numbers.tex` | Generated macros; every numeric claim in the text and captions reads from here |
| `tables/*.tex` (14) | Table bodies, one file per table, written by the analysis scripts |
| `sections/residual_adaptive.tex` | The residual-based adaptive refinement section |
| `figures_v2/*` (21) | The figures the PDF was built from, in the exact bytes used |
| `figures/xjtlu_logo_blue.pdf` | Cover logo |
| `references.bib` | Bibliography |

## How this version differs from `report/GroupC.pdf` in the repository root

The root copy is the 35-page version of 28 October 2026. This one is the tightened revision that
keeps that version's argument but reorganises it around a single error bar on $C$:

- **One reference solution, one error currency per experiment.** The energy-norm experiment and the
  capacitance experiment are now stated separately, each with its own indicator, reference solution
  and mesh family, instead of being quoted interchangeably.
- **Restored multi-panel figures.** The uniform-versus-graded mesh comparison with the corner
  close-up, the three-round residual-adaptive heat maps on a shared colour scale, and the
  three-way cost comparison all appear, using the original vector artwork where it existed.
- **A new section on residual-based adaptive refinement** (§8): why refinement lands where it does,
  why it cuts degrees of freedom, and why cumulative wall time can still grow.
- **The headline value is quoted as a value, not a $\pm$.** $C_{M7}=120.620$ pF/m, $0.153$ pF/m
  (0.127%) from the extrapolated limit, with the $0.18\%$ domain-truncation error reported
  separately rather than folded into a symmetric interval.

## Caveats

The figure-generating scripts (`make_*_figures.py`, `build_numbers.py`) live in the report's
working directory and are not committed here, matching how `Shuoran_Fu/` treats its own build
scripts; what is committed is their output. The figures under `figures_v2/` are therefore frozen
copies, and re-running the scripts elsewhere may produce byte-different PNGs at the same content.

Copied from the project's working tree on 9 November 2026. SHA-256 prefixes:

| File here | SHA-256 |
|---|---|
| `MTH321_Project2_GroupC_report_v8.pdf` | `6eefc7cee14f59a5` |
| `main.tex` | `881e0f3aac37c717` |
| `numbers.tex` | `a1893c02008722fc` |
