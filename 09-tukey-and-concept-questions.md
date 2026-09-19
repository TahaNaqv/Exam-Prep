# Topic 9 — Tukey & the "Why does this course exist?" Question

This is only 2 marks on the assignment, but it's the cheapest writing on the paper and a
version of it may well appear on the exam. It needs zero calculation — just that you
understood the framing.

## 1. The Day 1 argument (the course's position)

From the recap document, in order:

- A machine understands **electrical signals**, nothing else. The **transistor** — a tiny
  electrical switch — is the invention that made computing possible: switches give logic,
  logic gives everything else.
- **Binary** is the first bridge: a mathematical model of "on/off" that maps cleanly onto
  the physical hardware. It's the first of many **layers of abstraction** between circuit
  physics and the problems we care about.
- **Closure**: does an operation always keep you inside the set you started in? Often not.
  In computing this shows up as **overflow** — two individually valid numbers whose sum
  falls outside the representable range of the data type, producing a wrapped or nonsensical
  result. The same abstract closure problem, in a system you've already debugged.
- **The thesis:** scaling anything reliably — a machine, a model, a system, across millions
  of users and devices — requires **formal tools that guarantee consistent behaviour**,
  rather than ad hoc solutions that happen to work once. Linear algebra, calculus and
  probability are in this course because they're **necessary, not traditional.**

### The three Day 1 illustrations

| Hook | Point | Where it lands |
|------|-------|----------------|
| Image compression: 768 → 50 components | Real data has far less *true complexity* than its raw size suggests | **SVD** (Module 3) |
| Disease test: 99% accurate, positive result → only ~50% chance of illness | **Intuition is unreliable** about uncertainty | **Bayes' theorem** (later module) |
| ChatGPT resolving what "it" refers to | Attention is literally vectors, matrices and probability | Final module |

**Worth knowing the Bayes arithmetic**, it's memorable and might be asked informally:
out of 10,000 people, 100 have the disease and the test flags **99** of them. Among the
9,900 healthy people, a 1% false-positive rate flags about **99** more. So of ~198 positive
tests, **only about half are genuine** — because the healthy population is so much larger
that even a small false-positive rate produces a flood of false alarms.

## 2. Tukey's position (1962, "The Future of Data Analysis")

John Tukey argued that **data analysis is a broader empirical discipline than mathematical
statistics alone** — that **practical judgement matters as much as formal technique**.
Data analysis is closer to a science than to a branch of pure mathematics: it involves
looking at data, exploring it, choosing methods, and exercising judgement about a messy
real problem — not only deriving theorems.

(Recommended reading: pages 1–7, through the end of Section 4. The remaining ~60 pages are
1960s-specific techniques you don't need.)

## 3. How to answer "how do these two views relate?"

**The trap** is to treat them as opposed and pick a side. They aren't opposed —
they're **complementary**, and saying so is the point.

**The relationship, in substance:**
- Formal tools give you **guarantees, reliability and scale**. Without them you can't know
  *why* something works, or trust that it keeps working on new data or at a million users.
- Judgement tells you **which tool to reach for, whether the assumptions actually hold, and
  what the numbers mean** in context. Without it, you compute correct answers to the wrong
  question.
- **Neither is sufficient alone.** Formal technique without judgement is rigorous
  irrelevance; judgement without formal technique is unscalable guesswork.

**You can point at this course's own material as evidence**, which is what makes a strong
answer rather than a generic one. Any one of these works:
- L1 vs L2 "nearest neighbour": the mathematics gives **two** correct answers. Deciding
  which ruler is right for *your* problem is judgement, not theorem.
- Choosing k in truncated SVD or PCA: Eckart–Young guarantees the approximation is optimal
  *for a given k*, but nothing in the mathematics tells you what k should be — that's
  cumulative energy targets, elbows and domain constraints, i.e. judgement.
- PCA on Iris: the method correctly reports that two species overlap. Recognising that as
  an honest property of the data rather than a failure of the method is interpretation.
- Standardising before PCA: the algebra runs fine on unstandardised data and gives a
  meaningless PC1. Only judgement about units catches it.

**Structure for 2–3 sentences:**
1. State the course's position (formal tools are necessary for reliability at scale).
2. State Tukey's (data analysis is empirical; judgement matters as much as technique).
3. Reconcile them with **one concrete example from this module**.

⚠ Write this in **your own words**. It's a reflective question — a generic paragraph reads
as generic and the marks are for your own thinking. Also note assignment instruction 5:
cite external sources, and **disclose if an AI tool helped you understand a concept**. This
file is such a tool; say so if you use it.

## 4. Other "explain in a sentence" questions likely on the paper

Practise saying each of these out loud. One or two sentences, no calculation.

| Question | Core of the answer |
|---|---|
| Why can changing the norm change "nearest"? | L2 squares differences, so one large deviation is penalised more than several small ones summing to the same total. "Nearest" depends on the ruler, not just the data. |
| Why is a shift not linear? | `f(u+w) = u+w+(1,1)` but `f(u)+f(w) = u+w+(2,2)`. Additivity fails. |
| Why does rank ≠ shape? | Rank counts **independent** columns. A 2×3 matrix can have 2 independent columns (full rank); a 2×2 can have 1 (rank-deficient). |
| Why is the perpendicular the closest point? | The point, its projection and any other subspace point form a right triangle where the hypotenuse runs to the other point; the hypotenuse is always longest. |
| Why does `det = 0` mean no inverse? | Area is scaled to zero, so a dimension is collapsed; many inputs map to one output and can't be told apart afterwards. |
| Why does SVD always exist? | `AᵀA` and `AAᵀ` are symmetric for **any** A, and the spectral theorem gives every symmetric matrix a full orthogonal eigenbasis. |
| Why does PCA need centering? | Variance is spread **around the mean**; uncentered, PC1 points at the data's position rather than its variation. |
| Why do neural nets need nonlinear activations? | Stacked linear layers collapse into a single matrix, so depth would add nothing. |
| Why cosine similarity over raw dot product? | The raw dot product grows with magnitude, so a user who rates everything highly scores well against everyone. Cosine divides magnitude out and compares direction only. |
| Why is a flipped-sign singular vector not an error? | Flipping `uᵢ` and `vᵢ` together leaves `σᵢuᵢvᵢᵀ` unchanged, so the reconstruction is identical. |
