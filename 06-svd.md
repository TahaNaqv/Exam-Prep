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

## 7b. WORKED EXAMPLES — study these before the drills

---

### Worked Example 1 — reading the dimensions of a truncated SVD

> **Problem:** A matrix `A ∈ ℝ⁹ˣ⁴` (9 documents, 4 terms) is approximated at rank 3:
> `A₃ = U₃Σ₃V₃ᵀ`. Give the dimensions of each factor, and say which factor lives in
> document space and which in term space.

**Step 1 — decode `ℝ⁹ˣ⁴`.** Always **rows × columns**:

```
m = 9 rows    = 9 documents
n = 4 columns = 4 terms
```

**Step 2 — recall the full SVD shapes** for an `m × n` matrix:

```
A    =    U      Σ      Vᵀ
9×4      9×9    9×4    4×4
```

- `U` is **m × m** — it's a basis for the **output** space (the rows, here documents)
- `V` is **n × n** — it's a basis for the **input** space (the columns, here terms)

**Step 3 — apply the truncation rule for `k = 3`.**

> Keep the first **k columns** of U, the top **k singular values**, and the first **k rows**
> of `Vᵀ`.

```
U₃  :  keep first 3 columns of the 9×9 U      →   9 × 3
Σ₃  :  keep top 3 singular values             →   3 × 3
V₃ᵀ :  keep first 3 rows of the 4×4 Vᵀ        →   3 × 4
```

**Step 4 — CHECK by multiplying the shapes.** Do this every time:

```
(9 × 3)(3 × 3)(3 × 4)
    ↑___↑                inner dims match: 3 = 3  ✓
        ↑___↑            inner dims match: 3 = 3  ✓
 ↑_______________↑       result: 9 × 4  ✓  same as A
```

If it doesn't multiply back to A's shape, you've mixed up a dimension.

**Step 5 — which space is which.**

```
U's columns have 9 entries  →  one per DOCUMENT  →  U is document space
V's columns have 4 entries  →  one per TERM      →  V is term space
```

> **Answer:** `U₃` is 9×3, `Σ₃` is 3×3, `V₃ᵀ` is 3×4. **U (left singular vectors) = document
> space; V (right singular vectors) = term space.**

**Memory hook:** **V is the inpu*t* side (n = columns); U is the outp*u*t side (m = rows).**

---

### Worked Example 2 — computing a small SVD by hand

> **Problem:** Find the SVD of `A = [[1, 0], [0, 0], [0, 2]]`.

**Step 1 — note the shape and predict the factor sizes.** A is `3 × 2`:

```
U: 3×3        Σ: 3×2        Vᵀ: 2×2
```

**Step 2 — compute `AᵀA`.** (This is the key move: `V` comes from `AᵀA`.)

`Aᵀ` is A with rows and columns flipped:

```
        ┌            ┐
Aᵀ  =   │ 1   0   0  │        (2 × 3)
        │ 0   0   2  │
        └            ┘
```

```
              ┌            ┐ ┌        ┐     ┌        ┐
AᵀA     =     │ 1   0   0  │ │ 1   0  │  =  │ 1   0  │
              │ 0   0   2  │ │ 0   0  │     │ 0   4  │
              └            ┘ │ 0   2  │     └        ┘
                             └        ┘
```

*(Row 1 · col 1 = `1·1 + 0·0 + 0·0 = 1`; row 2 · col 2 = `0·0 + 0·0 + 2·2 = 4`; the
off-diagonals are 0.)*

**Step 3 — eigenvalues of `AᵀA`.** It's already diagonal, so the eigenvalues are just the
diagonal entries:

```
λ₁ = 4        λ₂ = 1
```

> Always list them **largest first** — singular values must be in descending order.

**Step 4 — singular values are the SQUARE ROOTS.**

```
σ₁ = √4 = 2        σ₂ = √1 = 1
```

```
        ┌        ┐
Σ   =   │ 2   0  │       (3 × 2 — pad with a zero row to match A's shape)
        │ 0   1  │
        │ 0   0  │
        └        ┘
```

**Step 5 — eigenvectors of `AᵀA` give V.** Use the ordinary Topic 4 recipe: for each λ,
solve `(AᵀA − λI)v = 0`. Writing `M = AᵀA`:

*For λ = 4* — subtract 4 from each diagonal entry:

```
            ┌                ┐     ┌            ┐
M − 4I  =   │ 1−4      0     │  =  │ −3     0   │
            │  0      4−4    │     │  0     0   │
            └                ┘     └            ┘

Row 1:  −3v₁ + 0·v₂ = 0   →   v₁ = 0
Row 2:   0·v₁ + 0·v₂ = 0   →   0 = 0, no information  →  v₂ is FREE
```

Take `v₂ = 1`  →  **`v = (0, 1)`**.  Check: `M(0,1) = (0, 4) = 4·(0,1)` ✓

*For λ = 1* — subtract 1 from each diagonal entry:

```
            ┌                ┐     ┌            ┐
M − 1I  =   │ 1−1      0     │  =  │  0     0   │
            │  0      4−1    │     │  0     3   │
            └                ┘     └            ┘

Row 1:   0 = 0, no information  →  v₁ is FREE
Row 2:  3v₂ = 0   →   v₂ = 0
```

Take `v₁ = 1`  →  **`v = (1, 0)`**.  Check: `M(1,0) = (1, 0) = 1·(1,0)` ✓

**The shortcut you can use next time.** A diagonal matrix scales each axis independently and
does nothing else:

```
┌        ┐ ┌   ┐     ┌      ┐
│ 1   0  │ │ x │  =  │ 1x   │      x-direction stretched by 1
│ 0   4  │ │ y │     │ 4y   │      y-direction stretched by 4
└        ┘ └   ┘     └      ┘
```

> **For a diagonal matrix, the eigenvalues ARE the diagonal entries, and each one's
> eigenvector is the coordinate axis it sits on:**
> - the **first** diagonal entry goes with the x-axis, `(1, 0)`
> - the **second** diagonal entry goes with the y-axis, `(0, 1)`

**Why, in pictures.** Watch three arrows pass through `M = [[1,0],[0,4]]`:

```
(1, 0) ──M──▶ (1, 0)     along the x-axis: same direction, ×1   →  EIGENVECTOR, λ=1
(0, 1) ──M──▶ (0, 4)     along the y-axis: same direction, ×4   →  EIGENVECTOR, λ=4
(1, 1) ──M──▶ (1, 4)     diagonal: 45° becomes 76°  — ROTATED   →  NOT an eigenvector
```

`(1,0)` lives *entirely* in the x-direction and M only scales x, so nothing else can happen
to it. `(1,1)` has a foot in both directions, and they're scaled by **different** amounts
(x by 1, y by 4) — so the arrow tips upward. That tipping is rotation, which disqualifies it.

**The one-line proof**, for any `M = [[d₁,0],[0,d₂]]`:

```
M(1,0) = (d₁·1 + 0·0,  0·1 + d₂·0) = (d₁, 0) = d₁·(1,0)   ✓  λ = d₁
M(0,1) = (d₁·0 + 0·1,  0·0 + d₂·1) = (0, d₂) = d₂·(0,1)   ✓  λ = d₂
```

`Mv = λv` straight from the definition — nothing to solve.

**⚠ Now the labelling, which looks backwards but isn't:**

```
λ₁ = 4  (the LARGEST)  →  v₁ = (0, 1)   ← the SECOND axis
λ₂ = 1                 →  v₂ = (1, 0)   ← the FIRST axis
```

Two different numbering schemes are colliding:

| Subscript on | Means |
|---|---|
| `λ₁`, `σ₁`, `v₁` | **rank order** — largest first, because SVD requires `σ₁ ≥ σ₂ ≥ ⋯` |
| position (1,1), (2,2) | **location** in the matrix |

So `4` is the largest eigenvalue → it is labelled `λ₁` → its eigenvector is `v₁`. And `4`
happens to sit at position (2,2) → so it pairs with the **second** axis, `(0,1)`. Hence
`v₁ = (0,1)`.

**The eigenvector of the biggest eigenvalue is always `v₁`, wherever that eigenvalue sat in
the matrix.** If `AᵀA` had been `[[4,0],[0,1]]`, you'd get `v₁ = (1,0)` and `v₂ = (0,1)` —
same method, subscripts merely lining up with positions by coincidence.

*Why sorting matters at all:* truncation keeps "the first k" singular values. Without the
descending order, "the first 2" would keep an arbitrary pair rather than the two most
important — and the whole of Topic 7 depends on that.

```
        ┌        ┐                    ┌        ┐
V   =   │ 0   1  │            Vᵀ  =   │ 0   1  │
        │ 1   0  │                    │ 1   0  │
        └        ┘                    └        ┘
```

**Step 6 — get U from `uᵢ = Avᵢ / σᵢ`.**

```
u₁ = A v₁ / σ₁ = A(0,1) / 2

     A(0,1):  row1 = (1)(0)+(0)(1) = 0
              row2 = (0)(0)+(0)(1) = 0
              row3 = (0)(0)+(2)(1) = 2       →  (0, 0, 2)

     divide by σ₁ = 2   →   u₁ = (0, 0, 1)
```

```
u₂ = A v₂ / σ₂ = A(1,0) / 1  =  (1, 0, 0) / 1  =  (1, 0, 0)
```

*(The third column of U is any unit vector orthogonal to these two — here `(0, 1, 0)` — to
complete the basis.)*

**Step 7 — check the singular values are non-negative and sorted.**

```
σ = (2, 1)   both ≥ 0  ✓   descending  ✓
```

**Step 8 — read off the rank.**

> **rank(A) = the number of non-zero singular values = 2.**

> **Answer:** `σ = (2, 1)`, `V = [[0,1],[1,0]]`, `u₁ = (0,0,1)`, `u₂ = (1,0,0)`, rank 2.

**The recipe, condensed:**

```
1. AᵀA
2. its eigenvalues λᵢ  and eigenvectors  →  eigenvectors are V's columns
3. σᵢ = √λᵢ, sorted largest first
4. uᵢ = Avᵢ / σᵢ
5. normalise everything
```

---

### Worked Example 3 — the sign-flip question

> **Problem:** Two students compute the SVD of the same matrix. Student A gets
> `u₂ = (0.6, −0.8)` and `v₂ = (1, 0)`. Student B gets `u₂ = (−0.6, 0.8)` and `v₂ = (−1, 0)`.
> Has someone made an error?

**Step 1 — see what changed.** Student B's `u₂` and `v₂` are both Student A's, **negated**.

**Step 2 — check what the reconstruction uses.** Each component contributes `σᵢ uᵢ vᵢᵀ`:

```
Student A:  σ₂ · ( u₂ )( v₂ )ᵀ
Student B:  σ₂ · (−u₂ )(−v₂ )ᵀ  =  σ₂ · (−1)(−1) · u₂ v₂ᵀ  =  σ₂ · u₂ v₂ᵀ
```

**The two minus signs multiply to `+1`.** The contribution is **identical**.

**Step 3 — conclude.**

> **No error.** Singular vectors are unique only **up to sign**: flipping `uᵢ` and `vᵢ`
> **together** leaves the reconstruction bit-for-bit unchanged. Different software (or the
> same software on different runs) may legitimately return either version.
>
> **Practical consequence:** never interpret *which side of zero* a point falls on. Only the
> **grouping** of points carries meaning.

> ⚠ The signs must flip **as a pair**. Flipping only `uᵢ` would change the answer and *would*
> be an error.

---

### What to notice across all three

- **Shapes first, always.** `m×n` → `U` is m×m, `V` is n×n, and truncation keeps k of each.
  Multiply the shapes back to check.
- **U = output/rows. V = input/columns.** For documents × terms: U = documents, V = terms.
- **V from `AᵀA`, `σ = √λ`, then `u = Av/σ`.**
- **Sign flips in pairs are meaningless**, not mistakes.

Now do the drills.


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
