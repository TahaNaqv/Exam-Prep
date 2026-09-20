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

## How each topic file is now structured

Read them **in this order within each file** — the worked examples exist so you never hit a
drill not knowing what to do:

```
1. The teaching sections        →  what the idea IS
2. "WORKED EXAMPLES" section    →  the SAME problem types, solved step by step
3. "Drills"                     →  now you try, then check the answers below
```

**Never jump straight to the drills.** Read the worked example, then close the file and
redo that example from scratch on paper. If you can reproduce it, the drill will be easy.
If you can't, reread that one example — not the whole chapter.

## Plan for the remaining time (exam is tomorrow)

You have one day. Triage hard — do **not** try to read everything.

**Priority 1 — the mechanical marks (do these first, they are the most recoverable)**
- File `04` (eigenvalues) — worked examples then drills. **~10 of 45 marks, pure recipe.**
- File `01` (norms) — quick, mechanical.
- File `03` (dot product + projection) — his stated weakest class topic, so likely heavily examined.

**Priority 2**
- File `02` (rank) — learn the *full rank vs full column rank* trap specifically.
- File `07` (energy) — short, formulaic, very likely on the paper.

**Priority 3**
- File `08` (PCA) — do Worked Example 1 at least once end to end.
- File `05` (diagonalization) — mainly the `P⁻¹ vs Pᵀ` test.
- File `06` (SVD) — the dimensions question is the most likely one.

**Last hour tonight:** `11-CHEATSHEET.md` and the 8 traps. **Tomorrow morning:** cheat sheet
only, plus one eigenvalue drill to warm up your hand.

**If you run out of time**, files `04`, `01`, `03` and `07` cover the most marks for the
least effort. Skipping `06` entirely costs you less than fumbling `04`.

## The files

| File | What it is |
|---|---|
| `01`–`08` | One per topic: teaching → **step-by-step worked examples** → 5 drills with full answers |
| `09` | Tukey reflection + a table of "explain in one sentence" answers |
| `10-MOCK-EXAM.md` | 10 fresh questions + 2 bonus, **full solutions**, timed practice |
| `11-CHEATSHEET.md` | One page. Monday morning, read only this |
| **`12-ASSIGNMENT-1-WORKED.md`** | **All 10 assignment questions, every part, worked step by step** |
| **`13-METHOD-RECIPES.md`** | The mechanical sub-skills done slowly — go here if you're stuck on *how* |
| **`14-NOTATION-PRIMER.md`** | **Every symbol decoded** — read this first if the notation is the wall |
| `COVERAGE-AUDIT.md` | Proof of what was read, including the slide images |

**Solved-example count: 25 worked examples + 50 drills + 12 mock questions + all 10
assignment questions — every one with a full solution.**

If you're stuck on a *concept*, use `01`–`08`.
If you're stuck on a *procedure* (how do I solve `(A−λI)v = 0`?), use `13`.
If you want exam-shaped practice, use `12` then `10`.

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
