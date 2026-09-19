# Topic 6 — Singular Value Decomposition (SVD)

## 1. Why SVD exists

Eigendecomposition needs a square, diagonalizable matrix. Real data matrices are usually
neither. We need something that **works for every matrix, with no exceptions.**

That's SVD.

## 2. The decomposition

> **A = U Σ Vᵀ**

For `A` of size **m × n**:

| Piece | Size | Name | What it is |
|-------|------|------|------------|
| **U** | m × m | left singular vectors | **orthonormal basis for the output space** |
| **Σ** | m × n | singular values | diagonal, entries `σ₁ ≥ σ₂ ≥ … ≥ 0`, **always ≥ 0**, sorted descending |
| **Vᵀ** | n × n | right singular vectors (transposed) | **V** is an orthonormal basis for the **input space** |

**Memory hook:**
- **V = input side** (n = number of columns of A = input dimension)
- **U = output side** (m = number of rows of A = output dimension)
- Σ holds the **stretch factors** connecting them.

Note that it's **V** whose columns are the right singular vectors; the decomposition
contains `Vᵀ`, so the *rows* of `Vᵀ` are those vectors.

## 3. How it generalises eigenvectors

| | Eigen (Topic 4) | SVD (now) |
|---|---|---|
| Relation | `Av = λv` | `Avᵢ = σᵢuᵢ` |
| Meaning | same direction, just scaled | direction is **allowed to rotate** (v → u), but **orthogonal in ⟹ orthogonal out** |

That's the key sentence. Eigenvectors demand the direction survive untouched — too strict,
so it often fails to exist. SVD relaxes it: the direction may rotate, but a set of
**perpendicular input directions still comes out perpendicular**. That weaker demand can
always be met, and it's enough to be useful.

## 4. Why SVD *always* exists (the actual guarantee)

Worth knowing — it's a clean, quotable argument.

- V's columns are the **eigenvectors of `AᵀA`**
- U's columns are the **eigenvectors of `AAᵀ`**
- Singular values are the **square roots of the shared (non-negative) eigenvalues** of either:
  `σᵢ = √λᵢ`

**And the punchline:** `AᵀA` and `AAᵀ` are **symmetric for any matrix A** — square or not,
diagonalizable or not. (Quick proof: `(AᵀA)ᵀ = Aᵀ(Aᵀ)ᵀ = AᵀA`.) The **spectral theorem**
guarantees every symmetric matrix has a full set of real, orthogonal eigenvectors, with no
exceptions.

> Diagonalization can fail on `A` itself. It **never** fails on `AᵀA` or `AAᵀ`.
> That's the whole guarantee.

Also: `AᵀA` is always positive semi-definite (eigenvalues ≥ 0), which is why singular
values are never negative.

This is the shortcut Assignment Q9 uses: `XᵀX = VΣ²Vᵀ`, so eigendecomposing the small
`XᵀX` gives you V and `σ² ` without doing a full SVD by hand.

## 5. Reading an SVD correctly (this is an exam question)

**Setup:** a document-term matrix `C` with **6 documents and 10 terms**, so `C ∈ ℝ⁶ˣ¹⁰`.
Rank-2 approximation `C₂ = U₂Σ₂V₂ᵀ`.

**Dimensions of the truncated pieces (k = 2):**
- `U₂` is **6 × 2** — keep the first k columns of U
- `Σ₂` is **2 × 2** — keep the top k singular values
- `V₂ᵀ` is **2 × 10** — keep the first k rows of Vᵀ

**Always check the shapes multiply out:** `(6×2)(2×2)(2×10) → 6×10` ✓ — same shape as C.
Do this check every time; it catches the error instantly.

**Which vectors live where:**
- **U (left) ↔ document space** — rows of C are documents, so U's columns are directions
  in document space. `U₂` groups **documents** by topic.
- **V (right) ↔ term space** — columns of C are terms, so V's columns are directions in
  term space. `V₂` groups **words** by topic.

**Does SVD exist even though C isn't square?** **Yes** — that's the entire point of SVD
(see §4). Non-square is not a problem; it's the case SVD was built for.

### ⚠ Sign ambiguity

> If a software package returns a singular vector with **every sign reversed**, that is
> **not an error.**

Flipping the signs of a `uᵢ` and its matching `vᵢ` **together** leaves `σᵢuᵢvᵢᵀ` unchanged,
so the reconstruction is identical. The decomposition is only unique up to these paired
sign flips.

**Practical consequence:** never read meaning into *which side of zero* a point lands on —
only into **which points group together.**

### Why rank-2 reveals two latent topics with no labels

The matrix's co-occurrence structure does the work. Documents about the same subject use
overlapping vocabulary, so their columns are nearly linear combinations of each other —
the matrix has **low effective rank**. SVD finds the few directions carrying the most
variation, and those directions *are* the shared vocabulary patterns. No one labelled
anything; the redundancy in the data was already there and SVD simply exposed it.

**Two caveats the lecture flagged:**
- Raw counts are fine for a toy demo; real systems preprocess first (TF-IDF, stopword
  removal, normalization) or the topics come out mediocre.
- Sign ambiguity again — group membership is meaningful, sign is not.

## 6. Rank, visible as a number

If a matrix is rank-deficient, `Σ` contains **zeros** on the diagonal.

> **rank(A) = the number of non-zero singular values.**

This is Topic 2's rank and Topic 4's `det = 0`, now visible as an actual number instead of
just "no inverse". It's also more useful in practice: real data gives you *tiny* singular
values rather than exact zeros, which tells you the **effective** rank — how much real
information is there, as a matter of degree.

### ⚠ A caution from the backup slides

In a worked example where `A` is symmetric and positive semi-definite (all eigenvalues ≥ 0),
you may find `U = V`. **That is a special case, not the rule.** In general `U ≠ V` — they
are bases for **different spaces** (output vs input), and can't even be the same shape
unless A is square.

## 7. Where SVD shows up

- **The Netflix Prize** — SVD on a users × movies matrix uncovered hidden taste dimensions
  nobody hand-labelled.
- **Latent Semantic Analysis** — the same idea on documents × words; search engines finding
  *topics* rather than keywords.
- **LoRA** — fine-tuning large language models cheaply leans on this same low-rank thinking.
- **Image compression** — Topic 7.

## 8. Drills

**D1.** `C ∈ ℝ⁶ˣ¹⁰`, rank-2 approximation `C₂ = U₂Σ₂V₂ᵀ`.
(a) Give the dimensions of `U₂`, `Σ₂`, `V₂ᵀ`.
(b) Which singular vectors are directions in document space, which in term space?
(c) Does SVD exist even though C isn't square? Why?
(d) A package returns a singular vector with every sign flipped — is that an error?
(e) Why might rank-2 reveal two latent topics with no labels supplied?

**D2.** `A ∈ ℝ⁴ˣ⁷`. Give the full sizes of U, Σ, Vᵀ. What's the maximum possible number of
non-zero singular values?

**D3.** State in one sentence why SVD exists for every matrix while eigendecomposition
doesn't.

**D4.** How do you read the rank of a matrix off its Σ?

**D5.** Write down what `Avᵢ = σᵢuᵢ` says in words, and how it differs from `Av = λv`.

---

### Answers

**D1.**
(a) `U₂`: **6 × 2**. `Σ₂`: **2 × 2**. `V₂ᵀ`: **2 × 10**.
Check: `(6×2)(2×2)(2×10) → 6×10` ✓, matching C.
(b) **U (left singular vectors) → document space** (6 documents = 6 rows).
**V (right singular vectors) → term space** (10 terms = 10 columns).
(c) **Yes.** SVD exists for every matrix with no exceptions. The guarantee comes from
`AᵀA` and `AAᵀ` being symmetric for any A, and the spectral theorem giving every symmetric
matrix a full orthogonal set of real eigenvectors.
(d) **Not an error.** Singular vectors are only defined up to sign: flipping `uᵢ` and `vᵢ`
together leaves `σᵢuᵢvᵢᵀ` and hence the reconstruction unchanged. Only the grouping is
meaningful, not the side of zero.
(e) Documents on the same subject share vocabulary, so C's rows/columns are highly
redundant and its effective rank is low. SVD identifies the directions capturing the most
variation, which are exactly those shared vocabulary patterns — the structure was already
in the co-occurrence data, and SVD exposed it without needing labels.

**D2.** `U`: **4 × 4**, `Σ`: **4 × 7**, `Vᵀ`: **7 × 7**. Check `(4×4)(4×7)(7×7) → 4×7` ✓.
Maximum non-zero singular values = `min(4,7) = **4**` (= max possible rank).

**D3.** Eigendecomposition needs A itself to have a full set of independent eigenvectors,
which can fail; SVD instead uses the eigenvectors of `AᵀA` and `AAᵀ`, which are symmetric
for **every** A and so are always fully diagonalizable by the spectral theorem.

**D4.** **rank(A) = the number of non-zero singular values on Σ's diagonal.** (With real
data, near-zero values indicate the *effective* rank.)

**D5.** `Avᵢ = σᵢuᵢ`: A takes the i-th right singular vector and maps it onto the i-th
left singular vector, stretched by `σᵢ` — the **direction is allowed to rotate** (`v → u`),
but orthogonal inputs still produce orthogonal outputs. `Av = λv` is stricter: it demands
the direction come out **unchanged**, only rescaled — which is why it often doesn't exist.

---

## 10. Computing a small SVD by hand (the lecture's backup worked example)

You are unlikely to be asked for a full SVD by hand, but the lecture showed one and it
makes `A = UΣVᵀ` concrete instead of abstract. The recipe follows directly from §4.

### The recipe

1. Compute `AᵀA`.
2. Find its **eigenvalues** `λᵢ` and **eigenvectors** — those eigenvectors are **V's columns**.
3. **`σᵢ = √λᵢ`**, sorted largest first. Put them on Σ's diagonal.
4. Get U from `uᵢ = Avᵢ / σᵢ` (only for the non-zero σ; the rest are filled in to complete
   an orthonormal basis).
5. **Normalise everything** — U and V must have unit-length columns.

### Worked example: `A = [[1,1],[1,1]]`

**Step 1.** `AᵀA = [[2,2],[2,2]]`

**Step 2.** `tr = 4`, `det = 4 − 4 = 0` → `λ² − 4λ = 0` → **λ = 4 and λ = 0**.
- λ=4: `[[−2,2],[2,−2]]` → `v₂ = v₁` → `(1,1)`, normalised **`(1/√2)(1,1)`**
- λ=0: `[[2,2],[2,2]]` → `v₂ = −v₁` → `(1,−1)`, normalised **`(1/√2)(1,−1)`**

**Step 3.** `σ₁ = √4 = 2`, `σ₂ = √0 = 0` → **`Σ = [[2,0],[0,0]]`**

**Step 4.** `u₁ = Av₁/σ₁ = A(1,1)/(√2·2) = (2,2)/(2√2) = (1/√2)(1,1)` ✓

**Result:**

    U = V = (1/√2) [ 1   1 ]  ,   Σ = [ 2  0 ]
                   [ 1  −1 ]          [ 0  0 ]

### The two things to actually take away

1. **Σ has a zero** — the SVD is exposing that A is **rank-deficient** (`det(A) = 0`) as an
   explicit number, rather than just "no inverse". Topic 2's rank and Topic 4's determinant,
   now readable off the diagonal. **rank(A) = 1** = number of non-zero singular values.

2. ⚠ **`U = V` here only because this A is symmetric and positive semi-definite** (all its
   eigenvalues are ≥ 0). **This is a special case, not the rule.** In general U and V are
   bases for **different spaces** — output vs input — and for a non-square A they aren't even
   the same size. Never assume `U = V`.
