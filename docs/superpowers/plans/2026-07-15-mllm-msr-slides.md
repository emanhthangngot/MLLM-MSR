# MLLM-MSR Seminar Slides Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build and verify a Vietnamese 30–40 minute Beamer seminar deck for the MLLM-MSR paper, using the existing autoregressive slide deck as the visual and file-organization template.

**Architecture:** Create an independent `slides/` tree at the repository root. Keep `main.tex` as the composition root, split content into focused section files, copy/adapt the template preamble, and keep all paper crops and diagrams under `slides/img/`. Use the paper as the factual source and the local repository only for implementation cross-checks.

**Tech Stack:** LaTeX Beamer, BibTeX, existing template preamble/settings, `pdflatex` or `latexmk`, Poppler rendering tools, local PNG assets.

## Global Constraints

- Keep layout 16:9, preamble, typography, colors, and file organization aligned with the source template.
- Remove all student/lecturer names from the title slide.
- Write all explanatory slide copy in Vietnamese; preserve standard English model/dataset names.
- Do not present unverified repository details as paper claims; label code-derived observations as implementation details.
- Store every presentation image locally under `slides/img/`; do not depend on external image URLs from `.tex`.
- Finish with no placeholder, TODO, stale source-deck names, or missing citation key.

---

### Task 1: Create the slide project skeleton and adapted preamble

**Files:**
- Create: `slides/main.tex`
- Create: `slides/preamble/packages.tex`
- Create: `slides/preamble/settings.tex`
- Create: `slides/content/0-title.tex`
- Create: `slides/ref/ref.bib`
- Create: `slides/ref/ref.tex`
- Create: `slides/README.md`
- Create: `slides/img/.gitkeep`

**Interfaces:**
- `slides/main.tex` composes every content file and sets `\graphicspath{{img/}}`.
- `slides/content/0-title.tex` defines the title metadata consumed by the title frame.
- `slides/ref/ref.bib` provides every citation key used by content files.

- [ ] **Step 1: Copy the source template structure and remove source-specific content.**

  Copy the source preamble/settings and replace only source-deck-specific paths and title metadata. Keep the document class as `\documentclass[aspectratio=169]{beamer}` and the navigation symbol suppression.

- [ ] **Step 2: Write the new composition root.**

  `slides/main.tex` must include, in order: title, background/problem, method, training, experiments, analysis, implementation notes, conclusion, and references. The title frame must contain only `\titlepage` and no student/lecturer block.

- [ ] **Step 3: Add build instructions and paper citation entries.**

  `slides/README.md` must document running `latexmk -pdf -interaction=nonstopmode main.tex` from `slides/`, and list the arXiv, AAAI proceedings, and repository sources. Add BibTeX entries for the main paper, LLaVA, LLaMA 3, MicroLens, Amazon Reviews 2023, and cited baseline families actually used in the deck.

- [ ] **Step 4: Run a skeleton compile.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: a PDF is produced; if content files are still empty, the compile may contain only title/TOC/reference frames but must have no fatal LaTeX error.

- [ ] **Step 5: Commit the skeleton.**

  Run: `git add slides && git commit -m "feat: scaffold MLLM-MSR seminar slides"`

### Task 2: Extract and prepare local visual assets

**Files:**
- Create: `slides/img/paper_framework.png`
- Create: `slides/img/item_summarizer.png`
- Create: `slides/img/recurrent_preference.png`
- Create: `slides/img/recommender_training.png`
- Create: `slides/img/qualitative_case_01.png`
- Modify: `slides/README.md`

**Interfaces:**
- Content files refer only to these local asset paths and never to remote URLs.

- [ ] **Step 1: Obtain the paper PDF from the official AAAI download or arXiv source.**

  Use the web source already identified in the design spec, then save a local working copy outside the committed slide tree if a local copy is needed for cropping. Do not add the full paper PDF to the repository unless it already exists there.

- [ ] **Step 2: Inspect PDF pages and identify figures/tables.**

  Use `pdftotext` and Poppler page rendering to locate the method/framework figure, recurrent summarization illustration, recommender fine-tuning illustration, and one qualitative example. Record the source page number for each image in `slides/README.md`.

- [ ] **Step 3: Crop at readable resolution.**

  Crop each selected figure with enough surrounding labels to preserve meaning. Prefer PNG at 150–220 dpi or a lossless crop from a rendered page. If a figure is too dense, create a simplified TikZ diagram instead and label it as an interpretation.

- [ ] **Step 4: Validate image dimensions and readability.**

  Run `file slides/img/*.png` and inspect the rendered slide after integration. Reject images whose labels become unreadable at 16:9 slide size.

- [ ] **Step 5: Commit the assets.**

  Run: `git add slides/img slides/README.md && git commit -m "feat: add local MLLM-MSR visual assets"`

### Task 3: Write the motivation, problem definition, and method sections

**Files:**
- Create: `slides/content/1-intro.tex`
- Create: `slides/content/2-problem.tex`
- Create: `slides/content/3-method-overview.tex`
- Create: `slides/content/4-item-summarizer.tex`
- Create: `slides/content/5-preference-summarizer.tex`

**Interfaces:**
- These files provide the narrative and citations consumed by `main.tex`.
- They use the local image assets from Task 2 and define notation consistently for later experiment slides.

- [ ] **Step 1: Add the motivation slides.**

  Explain the gap between text-only LLM recommendation and multimodal sequential recommendation. Use one slide for the concrete user-history example and one slide for the three paper challenges. Avoid claiming that multimodality alone solves temporal modeling.

- [ ] **Step 2: Add the formal problem definition.**

  Define user `u`, ordered history `S_u=[I_u^1,\ldots,I_u^n]`, item text `W`, item image `I`, candidate item, and next-interaction prediction. Include the probability objective in a compact equation.

- [ ] **Step 3: Add the method overview.**

  Present the two-stage preference inference followed by MLLM recommender fine-tuning. Place `paper_framework.png` with a concise verbal walkthrough of data flow.

- [ ] **Step 4: Explain item summarization.**

  Describe separate textual/image summarization prompts, balanced output, and fusion into a unified item description. Show the transformation `image + title -> visual description + text summary -> fused item description`.

- [ ] **Step 5: Explain recurrent preference summarization.**

  Define chunking and recurrent state update: each new item chunk is summarized together with the previous preference state. Include a prompt excerpt and a worked three-step example, keeping prompt text short enough to read.

- [ ] **Step 6: Compile and inspect only the method block.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: all method slides compile, equations fit within margins, and no image overflows occur.

- [ ] **Step 7: Commit the method block.**

  Run: `git add slides/content && git commit -m "feat: explain MLLM-MSR problem and preference pipeline"`

### Task 4: Write recommender training and implementation correspondence

**Files:**
- Create: `slides/content/6-recommender.tex`
- Create: `slides/content/7-training.tex`
- Create: `slides/content/8-implementation.tex`

**Interfaces:**
- These slides reuse notation from Tasks 1–3 and reference `recommender_training.png`.

- [ ] **Step 1: Describe the MLLM recommender input/output.**

  Show the image token plus user/item prompt and the constrained yes/no recommendation output. Explain that this is a supervised next-token prediction setup rather than a conventional ranking head.

- [ ] **Step 2: Add the SFT objective and LoRA details.**

  Include the autoregressive cross-entropy equation from the paper, explain label masking/padding at a high level, and summarize LoRA as the parameter-efficient adaptation used in the repository. Distinguish paper-level strategy from repo hyperparameters such as LLaVA checkpoint, rank, and epoch count.

- [ ] **Step 3: Add paper-to-code mapping.**

  Map concepts to repository files: `image_summary.py`, `preferece_inference_recurrent.py`, `dataset_create.py`, `train_llava_sft.py`, and `test_with_llava_sft.py`. Mark this section “implementation trong repo” and note any path/name mismatch in README commands without silently correcting the paper.

- [ ] **Step 4: Compile and review training slides.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: objective, code mapping, and architecture image are readable without code blocks overflowing.

- [ ] **Step 5: Commit the training block.**

  Run: `git add slides/content && git commit -m "feat: document MLLM recommender training and implementation"`

### Task 5: Write datasets, experiments, results, ablations, and critique

**Files:**
- Create: `slides/content/9-experiments.tex`
- Create: `slides/content/10-results.tex`
- Create: `slides/content/11-analysis.tex`
- Create: `slides/content/12-limitations.tex`

**Interfaces:**
- Result tables and claims must use the paper PDF as the source and cite the main paper.
- Qualitative slides use `qualitative_case_01.png` and preserve paper labels/metric names.

- [ ] **Step 1: Add datasets and protocol.**

  Summarize MicroLens and Amazon domains, sequence construction, train/validation/test setup, candidate prediction task, baselines, and metrics exactly as reported. Use a compact table rather than prose-heavy paragraphs.

- [ ] **Step 2: Transcribe the main result tables.**

  Reproduce only the rows/columns needed for the presentation, retain metric directionality arrows, bold best values, and add a footnote identifying numbers as reported by the paper. Do not invent missing values from code or secondary sources.

- [ ] **Step 3: Add ablation and modality analysis.**

  Explain what changes when image information, item summarization, recurrent preference summarization, or fine-tuning components are removed. Make each ablation answer one causal question.

- [ ] **Step 4: Add qualitative case and limitations.**

  Show one readable example of history → preference → recommendation, then discuss information loss from image-to-text conversion, inference cost, dependence on generated summaries, and limited evidence for production-scale latency.

- [ ] **Step 5: Compile and inspect experiment slides.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: tables fit on the page, no text is clipped, and every quantitative claim has a citation in the slide or nearby reference.

- [ ] **Step 6: Commit the experiment block.**

  Run: `git add slides/content && git commit -m "feat: add MLLM-MSR experiments and critical analysis"`

### Task 6: Finish references, speaker flow, and documentation

**Files:**
- Modify: `slides/main.tex`
- Modify: `slides/content/0-title.tex`
- Modify: `slides/ref/ref.bib`
- Modify: `slides/ref/ref.tex`
- Modify: `slides/README.md`

- [ ] **Step 1: Add final conclusion and discussion frames.**

  Include three takeaways, what the method contributes beyond text-only sequential recommendation, what remains unresolved, and 3–4 questions suitable for discussion with the advisor.

- [ ] **Step 2: Complete citations and remove stale template content.**

  Run `rg -n "autoregressive|Nhóm 14|CSC14005|Lê Nhựt Nam|TODO|TBD" slides` and remove every stale match except any source URL explicitly documented as the template reference. Run `bibtex`/`latexmk` until citations resolve.

- [ ] **Step 3: Add presenter notes as comments.**

  Add short `% Speaker:` comments above dense slides with the intended explanation and approximate time, without changing visible slide text.

- [ ] **Step 4: Compile the complete deck.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: successful PDF with no undefined citations, overfull boxes requiring content changes, or missing image warnings.

- [ ] **Step 5: Commit the complete content.**

  Run: `git add slides && git commit -m "feat: complete MLLM-MSR seminar deck"`

### Task 7: Render and perform visual verification

**Files:**
- Create: `slides/rendered/` (ignored by `slides/.gitignore`)
- Modify: `slides/.gitignore`

- [ ] **Step 1: Render the PDF pages to PNG.**

  Run: `cd slides && pdftoppm -png -r 120 main.pdf rendered/page`

- [ ] **Step 2: Inspect page montage and high-risk pages.**

  Review the title, framework, equations, dense tables, qualitative case, conclusion, and references pages. Check contrast, margins, image legibility, and that no footer/title collision exists.

- [ ] **Step 3: Fix each visual defect at its source.**

  Reduce copy, split a dense frame, resize a table, or replace a low-resolution crop; do not hide overflow with arbitrary negative spacing.

- [ ] **Step 4: Run final static checks.**

  Run:
  `rg -n "TODO|TBD|placeholder|Nhóm 14|Sinh viên thực hiện|Giảng viên hướng dẫn" slides`

  Expected: no output.

  Run: `git diff --check`

  Expected: no whitespace errors.

- [ ] **Step 5: Recompile after fixes and report evidence.**

  Run: `cd slides && latexmk -pdf -interaction=nonstopmode main.tex`

  Expected: successful final PDF; report the PDF path and number of frames in the handoff.

