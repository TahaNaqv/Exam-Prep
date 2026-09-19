# Coverage Audit — what was read, and what the second pass found

You asked whether every lecture and every topic is genuinely covered. Here is the honest
accounting, including a gap my first pass had.

## Every file in the course folder was read

| File | Slides | Status |
|---|---|---|
| `AI-5002-Day1-STUDENT-COPY.pptx` | 15 | ✅ read (intro, course logistics, 3 hooks, notation, roadmap) |
| `AI-5002-Module2-Session1-LIVE-STUDENT.pptx` | 20 | ✅ read → Topic 1 |
| `AI-5002-Module2-Session2-LIVE-STUDENT.pptx` | 18 | ✅ read → Topic 2 |
| `AI-5002-Module2-Session3-LIVE-STUDENT.pptx` | 20 | ✅ read → Topic 3 |
| `Module3-Session1-Slides-STUDENT-COPY.pptx` | 16 | ✅ read → Topic 4 |
| `Module3-Session2-Slides-STUDENT-COPY.pptx` | 14 | ✅ read → Topics 5, 6 |
| `Module3-Session3-Slides-STUDENT-COPY.pptx` | 17 | ✅ read → Topic 7 |
| `Module3-Session4-Slides-STUDENT-COPY.pptx` | 16 | ✅ read → Topic 8 |
| `recap.pdf` | 2 pp | ✅ read → Topic 9 |
| `MSF-Module2-Textbook-Reading.pdf` | 2 pp | ✅ read (reading list only — no new content) |
| `Assignments/Assignment-1-...pdf` | 5 pp | ✅ read → mock exam structure |
| `py-notebook/module2_...ipynb` | — | ✅ read (mirrors M2 S1–S3; no new theory) |

**Total: 136 slides + 9 PDF pages + 1 notebook.** Backup/reference slides included.

## The gap in the first pass — and how it was closed

My first pass extracted slide **text**. It turns out **42 slides carry embedded images**,
and on several of those the mathematical content is *inside the image*, invisible to text
extraction. The affected slides were titled things like "Worked Example", "Your Turn", and
"PCA by Hand" — exactly the high-value ones.

I extracted all 50 embedded media files and visually inspected the mathematical ones.

### What the images confirmed (already correct in the notes)

- **M3-S1 worked example** = `A = [[4,1],[2,3]]`, λ = 5, 2, eigenvectors (1,1) and (1,−2) —
  identical to what Topic 4 teaches, and identical to Assignment Q5.
- **M3-S1 det/trace relation** = `det = ad − bc = λ₁λ₂`, `tr = a + d = λ₁ + λ₂` — matches Topic 4.
- **M3-S3 truncation error** = σ = (4,3,1), errors √10 ≈ 3.16 and 1.00 — matches Topic 7 and
  Assignment Q8.
- **M3-S2 diagonalization diagram** = the "apply P⁻¹ → apply D → apply P" three-step story —
  matches Topic 5.
- **M3-S3 compression arithmetic** = 786,432 → 89,650 values, ≈8.77× — matches Topic 7.

### What the images added (notes have been updated)

| Found in | What it was | Where it's now covered |
|---|---|---|
| M3-S1 slide 11 | The real "Your Turn" matrix `A = [[5,4],[1,2]]` (slides gave only the answer λ=6,1) | **04**, new §10 |
| M3-S2 slide 13 | A full **SVD computed by hand**: `A=[[1,1],[1,1]]` → `U=V=(1/√2)[[1,1],[1,−1]]`, `Σ=diag(2,0)` | **06**, new §10 |
| M3-S3 slide 16 | Truncation shown on a **diagonal** matrix — makes "drop the small ones" literal | **07**, new §10 |
| M3-S3 slide 17 | Storage-saved framing: **≈88.6%** saved, storing ~11.4% | **07**, new §11 |
| **M3-S4 slide 15** | **PCA scores** — projecting points onto PC1 to get the numbers you actually plot | **08**, new §10 |
| M3-S4 slide 14 | Covariance stated as `1/(n−1)` with `C = QΛQᵀ`, `Λ = Σ²/(n−1)` — the **assignment uses 1/n** | **08**, new §11 |

**The one substantive gap was PCA scores.** My first pass taught how to *find* the principal
components but skimped on *projecting the data onto them* — which is the step that actually
produces the 2D chart PCA exists to make. That's now a full worked example in file 08, plus
mock question **B2**.

Two bonus questions (**B1** SVD by hand, **B2** PCA scores) were added to the end of
`10-MOCK-EXAM.md` to drill the new material.

## Topic-by-topic coverage against the assignment's stated scope

The assignment cover page lists the assessed scope. Mapping it:

| Assessed scope | File |
|---|---|
| vectors and norms | 01 |
| matrices and rank | 02 |
| dot products and projections | 03 |
| determinants and eigenvalues | 04 |
| diagonalization | 05 |
| singular value decomposition | 06 |
| rank-k approximation | 07 |
| principal component analysis | 08 |
| the reflective Tukey question | 09 |

**Nine for nine.** Nothing in the assessed scope is missing, and nothing in the notes is
outside it.

## What is deliberately *not* covered

These appear in the material but are explicitly marked out of scope by the instructor —
don't spend Sunday on them:

- **3×3+ determinants by cofactor expansion** — M3-S1 backup says "not required for this
  course's core assessment".
- **Gram–Schmidt / QR** — M2-S3 says "named, not derived"; the reading list marks §3.5 /
  §5.3–5.4 as "optional enrichment".
- **Eckart–Young proof** — "never required", possible optional bonus only.
- **Power iteration** — backup slide, context only.
- **Full Bayes / attention / later modules** — Day 1 previews of Modules 6+, not on this exam.

The two proofs flagged as *possible bonus* marks are both in the notes anyway: the
dot-product formula equivalence (file **03**, §1) and the Eckart–Young statement (file
**07**, §2).
