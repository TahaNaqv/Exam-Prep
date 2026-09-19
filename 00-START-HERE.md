# Sessional 1 — Survival Plan (Sat → Mon)

## What is actually on the exam

I read every lecture, the recap, the reading list and Assignment 1.
The instructor told you the scope twice — once on the assignment cover page, once in the
final self-check list. It is exactly these nine things:

| # | Topic | From |
|---|-------|------|
| 1 | Vectors & norms (L1, L2, L∞), distance | M2 S1 |
| 2 | Matrices as functions, column picture, rank, independence | M2 S2 |
| 3 | Dot product, cosine similarity, orthogonality, projection, residual | M2 S3 |
| 4 | Determinant, trace, eigenvalues, eigenvectors | M3 S1 |
| 5 | Diagonalization A = PDP⁻¹ and its limits | M3 S2 |
| 6 | SVD A = UΣVᵀ — dimensions, roles, why it always exists | M3 S2 |
| 7 | Truncated SVD, energy, Eckart–Young, choosing k | M3 S3 |
| 8 | PCA via SVD (centering, explained variance, sign) | M3 S4 |
| 9 | Tukey reflection (2 marks, pure writing) | Day 1 + assignment Q10 |

**Assignment 1 is the best available mock exam.** The instructor literally wrote:
"The assignment is not a list of examination questions, but it tests the same underlying
concepts and skills." Ten questions, one per topic. Treat it as the syllabus.

## The 2-day plan

**Saturday (today)**
- Topic 1 → 2 → 3 (files 01, 02, 03). These are the foundation; everything else reuses them.
- After each file, do its drills **on paper**. Not in your head. On paper.
- Target: finish by tonight. ~4 hours if you stay honest about the drills.

**Sunday**
- Morning: Topics 4 → 5 (files 04, 05). Eigenvalues are the most mechanical marks
  on the paper — these are free marks once the recipe is automatic.
- Afternoon: Topics 6 → 7 → 8 (files 06, 07, 08). SVD/PCA.
- Evening: file 10 (full mock, timed) — including bonus **B1/B2** on SVD-by-hand and PCA
  scores — then file 11 (one-page cheat sheet).

**Monday morning**
- Read file 11 only. Nothing new. Re-do the two eigenvalue drills to warm up your hand.

## Coverage

All 136 slides, both PDFs and the notebook were read — then a **second pass** extracted the
50 images embedded in the slides, because several worked examples live inside pictures that
text extraction cannot see. See `COVERAGE-AUDIT.md` for the full accounting of what was
checked and what that second pass added (most importantly: **PCA scores**, file 08 §10).

## How to use these files

Read a section, then **close it and redo the worked example on paper from scratch.**
If you can't, you didn't learn it — reread that one section only.

Reading maths without writing it feels like learning and isn't. This is the single
biggest reason people walk out of a maths exam surprised.

## The traps the instructor is hunting for

He listed these himself in the assignment's final self-check. Each one is a place
he expects the class to lose marks. Memorise this list:

1. Rank is **not** about shape. A 2×3 matrix can be full rank.
2. A negative dot product means "angle > 90°", **not** "exactly opposite (180°)".
3. Always verify a projection residual is orthogonal: residual · a = 0.
4. P⁻¹ is **not** Pᵀ unless you have proved the columns are orthonormal.
5. Know the dimensions and roles of U, Σ, Vᵀ cold.
6. Energy = sum of **squared** singular values.
7. Mean-centre **before** computing a covariance matrix.
8. **Interpret** the result in a sentence; don't just report a number.

Marks in this course are for reasoning, not answers — the assignment says
"Unsupported final answers alone will not receive full credit." Expect the same on the exam.
Write a sentence after every calculation saying what it means.
