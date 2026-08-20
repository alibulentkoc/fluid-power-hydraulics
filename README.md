# Fluid Power Systems Engineering — Mech & Hyd Sys

A hands-on fluid power course built around **one real machine**: a precision hydraulic lift platform that raises a two-tonne load to a commanded height and holds it to within ±1 mm. Every lesson settles one real engineering decision about that platform.

The learner-facing site is built with **MkDocs Material**. This repository also carries the production directives the lessons are built to, the curriculum governance framework, the Trainer-integration design, the simulation engine, and the build/verification tooling.

---

## Repository layout

```
docs/                     The deployable learner site (MkDocs)
  index.md                Course home
  course-machine.md       The machine every lesson builds
  assets/                 Course-machine figure
  javascripts/            MathJax config (pymdownx.arithmatex generic)
  module01/ … module11/   11 modules, 52 lessons (complete)
    NN_<lesson>.md        Twelve-part lesson
    assets/*.svg          Reviewed figures
    demos/*.html          Standalone interactive demonstrations
    quizzes/*.html        Auto-graded knowledge checks

standards/                Production directives the lessons must satisfy
  GLOBAL_CURRICULUM_PRODUCTION_STANDARD.md
  VISUAL_STANDARD_v1.md
  COURSE_MACHINE.md       Continuity spine (the machine + module thread)

framework/                Curriculum governance (competency, assessment,
                          certification, credit, learner journey, phase reports)

architecture/             Trainer-integration + engine design specs
                          (canonical parameters, stage specs, units boundary)

trainer-integration/      Patches to apply to the separate Trainer repo
                          (the Trainer itself is NOT modified here)

tools/                    Build + verification pipeline
  build-preview.py        Assemble a self-contained review preview
  prerender-math.mjs      LaTeX -> plain inline SVG (no client MathJax)
  clean-preview.py        Strip CDN/mermaid, fix nesting
  rasterize-embed.py      Figure + formulas -> PNG <img> data-URIs
  gates/gate-*.mjs        Per-lesson Definition-of-Done gates

previews/                 Self-contained single-file review artifacts
                          (figure + formulas embedded as PNG; for offline review)

governance/
  master_progress.md      The decision ledger
```

---

## Build and preview the site locally

```bash
pip install -r requirements.txt
mkdocs serve            # live preview at http://127.0.0.1:8000
mkdocs build            # static site into ./site
```

Before deploying, set `site_url` in `mkdocs.yml` to your GitHub Pages URL.

## Deploy to GitHub Pages

A workflow is provided at `.github/workflows/deploy-docs.yml` that builds and publishes the site on every push to `main`. Enable **Settings → Pages → Source: GitHub Actions** once. Or deploy manually:

```bash
mkdocs gh-deploy
```

---

## How lessons are produced and verified

Lessons are authored as Markdown and held to the directives in `standards/`. Each lesson ships a reviewed SVG figure, a standalone interactive demo, and an auto-graded quiz. Two artifacts are generated per lesson:

- **The site lesson** (`docs/`): Markdown with `![](assets/…svg)` figures, `\[ … \]` math (rendered by MathJax), and mermaid diagrams — all rendered by MkDocs in the browser.
- **A self-contained review preview** (`previews/`): a single HTML file in which the figure and every formula are embedded as **PNG `<img>` data-URIs**, so it renders identically in any viewer with no external dependencies. Built via `tools/build-preview.py → prerender-math.mjs → clean-preview.py → rasterize-embed.py`.

Every lesson is checked by a **Definition-of-Done gate** (`tools/gates/`) that runs the demo and quiz, validates the worked-example structure, the math, the figure/port discipline, and the self-contained preview. Gates run on Node with `jsdom`:

```bash
cd tools && npm i jsdom     # one-time
node gates/gate-m2l1.mjs
```

The whole site also passes `mkdocs build --strict` (no broken links or missing files), and every lesson's coding-exercise runs clean.

---

## Status

**Complete — all 11 modules, 52 lessons.** Each lesson carries a reviewed figure, an interactive demo, and an auto-graded quiz.

| Module | Title | Status |
|--------|-------|--------|
| 01 | Introduction to Fluid Power | ✅ Complete (4 lessons) |
| 02 | Fluid Power Components | ✅ Complete (4 lessons) |
| 03 | Fluid Fundamentals | ✅ Complete (4 lessons) |
| 04 | Actuators | ✅ Complete (5 lessons) |
| 05 | Power Units | ✅ Complete (5 lessons) |
| 06 | Valves & Control | ✅ Complete (5 lessons) |
| 07 | Circuits | ✅ Complete (5 lessons) |
| 08 | Electrohydraulic Control | ✅ Complete (5 lessons) |
| 09 | Modeling & Simulation | ✅ Complete (5 lessons) |
| 10 | Control Systems | ✅ Complete (5 lessons) |
| 11 | The Live Digital Model | ✅ Complete (5 lessons) |

The Trainer platform (`github.com/alibulentkoc/hydraulic-trainer-platform`) is a separate repository; its architecture and contracts are unchanged. Anything under `trainer-integration/` is a patch to apply there deliberately, not an in-place modification.

---

## License

The curriculum content is licensed under **CC BY 4.0** — see the [`LICENSE`](LICENSE) file. If you use or adapt this course, please cite it via [`CITATION.cff`](CITATION.cff).
