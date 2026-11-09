# Chuqiao Tang — Project Contributions

**MTH321 Project 2 · Group C**
Student ID: **2360692**

My contribution to the repository is the presentation and one piece of verification code. The deck
in this folder replaces the 13-slide first draft: new theme, its own set of eleven figures, its own
generated macro file, and a slide order built around the error bar on $C$ rather than around the
method. The code is a boundary-condition audit that checks the Dirichlet elimination the solver
uses against a textbook row replacement.

## Work and evidence

| My work | What to inspect |
|---|---|
| Presentation: the 15-slide narrative, from "one number and how wrong it is" to the two conclusions | [Slide source](../新版ppt/main.tex), [archived PDF](presentation/Presentation_Chuqiao_Tang.pdf) |
| Slide figures: geometry, verification, P1/P2 order, grading, truncation, engineering values | [Copies in this folder](presentation/figures), [originals](../新版ppt/figures) |
| Single-sourced numbers: every figure and the report share one generated macro file | [numbers.tex](presentation/numbers.tex) |
| Boundary-condition audit: Dirichlet elimination vs row replacement, with mutants | [Audit script](code/boundary_row_audit.py), [saved results](code/results/boundary_row_audit.json) |

It is a rebuild rather than a restyle. The earlier draft used the stock Madrid theme and 12 frames;
this one has a custom title bar, 14 frames, and a figure set drawn specifically for the slide width,
so the panels are legible without shrinking the text. Every number on it comes from the same macro
file the report reads.

## The presentation

15 pages, 14 content frames, Beamer, compiled with `pdflatex`. The argument runs:

1. **The answer, and its error.** $C$ and $Z_0$ with the distance to the extrapolated limit stated
   on the same slide, so the audience knows the size of the claim before seeing any machinery.
2. **Model and method.** Cross-section, the equation, weak form, $P_1$ elements, and the 23
   analytic checks the implementation passes before it is used for anything.
3. **The wrong way to quote an error.** Reading the M6$\to$M7 change as the error inflates it by
   $(2^{p}-1)$, here $1.2$ at the observed $p=1.16$; a shrinking change bounds nothing on its own.
   The distance from the extrapolated limit is $0.127\%$, so the $0.1\%$ target is missed.
4. **Two results that do not behave as expected.** Quadratic elements do not buy an order, because
   the corner dominates; corner grading restores it, and the frame after that shows where the extra
   elements actually go.
5. **The two errors that compete with discretisation.** The linear solver is not one of them
   ($6.3\times10^{-3}$ relative to the discrete solution); the $8H$ box is ($0.18\%$, the same size
   as the M7 error).
6. **What follows.** $C$ with its error bar, $\varepsilon_{\rm eff}$ and $Z_0$ twice as accurate
   as $C$, and two conclusions.

The eleven figures are drawn from the stored result tables by the presentation build script,
which also writes `numbers.tex`, so a slide cannot show a number the report does not. That script
is part of the presentation working directory and is not committed here; what the repository holds
is its outputs, in [`新版ppt/figures/`](../新版ppt/figures). The copies under
`presentation/figures/` are the exact bytes the archived PDF was built from.

## Boundary-row audit

The solver imposes the Dirichlet condition by *elimination*: rows and columns of the fixed nodes
are dropped and the known values move to the right-hand side,
$K_{ff}u_f = -K_{fd}g_d$. Nothing in the repository checked that this equals the textbook system
$A[i,:]=e_i,\ b[i]=g_i$. Every existing analytic test also uses a single non-zero electrode, so a
sign error in the shift $-K_{fd}g_d$ would cancel out of most of them.

The audit compares three independent constructions on the same matrix — elimination written out as
the solver does it, row replacement assembled as an explicit $n\times n$ system, and the shipped
solver shifted by a constant — and then repeats the comparison against the closed-form
piecewise-linear field of a series capacitor.

| Check | Layered box (117 nodes, 18 fixed) | Microstrip (12,078 nodes, 125 fixed) |
|---|---|---|
| elimination vs row replacement, $\max\lvert\phi_E-\phi_T\rvert/\max\lvert g\rvert$ | $8.1\times10^{-16}$ | $7.8\times10^{-15}$ |
| free-row residual of the eliminated system | $2.0\times10^{-14}$ | $2.5\times10^{-14}$ |
| shipped solver vs row replacement | $9.6\times10^{-16}$ | — |
| row-replacement solution vs closed form | $1.1\times10^{-15}$ | — |

Eleven checks, all `PASS` in [the saved run](code/results/boundary_row_audit.json).

Four mutants show the test is not vacuous: dropping the shift gives $0.90$~V, flipping its sign
gives $1.81$~V, and leaving the Dirichlet row as a $K$ row gives $4.6\times10^{16}$. One result is
worth stating because it is negative: dropping the ground columns from the shift moves the layered
box by $0.33$~V but the microstrip by only $7.8\times10^{-15}$, because the shipped ground
electrode is at $0$~V. **The microstrip configuration cannot detect that bug at all** — the
layered box can, and that is the reason the layered case is in the test.

Run it from the repository root:

```bash
python code_hardened/boundary_row_audit.py
```

## Files in this folder

These are copies. The working files remain where the project keeps them, so later team edits do
not erase this record. SHA-256 prefixes are of the copied bytes.

| File here | Copied from | SHA-256 |
|---|---|---|
| `presentation/main.tex` | [`新版ppt/main.tex`](../新版ppt/main.tex) | `319bbe0fc947577c` |
| `presentation/numbers.tex` | [`新版ppt/numbers.tex`](../新版ppt/numbers.tex) | `e956abc802b7fd76` |
| `presentation/Presentation_Chuqiao_Tang.pdf` | [`新版ppt/新版PPT.pdf`](../新版ppt/新版PPT.pdf) | `bc10ea7170ba80d0` |
| `presentation/figures/*.png` (11) | `新版ppt/figures/` | as archived |
| `code/boundary_row_audit.py` | [`code_hardened/boundary_row_audit.py`](../code_hardened/boundary_row_audit.py) | `3acd6e48a2c087b8` |
| `code/results/boundary_row_audit.json` | [`code_hardened/results/`](../code_hardened/results/boundary_row_audit.json) | `c7b4b630df5c9ed2` |

The archived PDF is the version uploaded on 4 November 2026, 15 pages. The deck in
`新版ppt/` was used for the group's rehearsal; later cosmetic revisions to the working file are
not represented here.

## Repository activity

| Commit | Scope |
|---|---|
| `e0a34f1` | Presentation: layout, all slide figures and the compiled PDF |
| `ccd2f37` | Boundary-row audit: script and saved results |
