# Notation Primer — every symbol in this course, decoded

Read this before Topic 3. Most of the difficulty in these lectures is notation, not ideas.
Nothing here is a new concept — it's a dictionary.

---

## 1. Object types — what kind of thing is this?

| Written | Called | What it is | Example |
|---|---|---|---|
| `c`, `k`, `λ`, `σ` | **scalar** | one plain number | `5`, `−2`, `0.5` |
| `v`, `u`, `w`, `x`, `b` (bold lowercase) | **vector** | a list of numbers | `(3, 1)` |
| `A`, `B`, `X`, `U`, `V` (bold uppercase) | **matrix** | a grid of numbers | `[[2,1],[3,4]]` |

The **convention** matters: lowercase = vector, uppercase = matrix, plain letters like `c`
= scalar. When you see `cv` you should immediately read "scalar times vector".

---

## 2. The two kinds of bars ⚠

| Written | Name | Applies to | Meaning |
|---|---|---|---|
| `\|c\|` | absolute value | a **number** | strip the minus sign: `\|−5\| = 5` |
| `‖v‖` | norm | a **vector** | the length of the arrow: `‖(3,−4)‖ = 5` |

**The bars tell you what's inside.** One bar → number. Two bars → vector.

They're the same idea at different sizes: for a 1-entry vector, `‖(−5)‖ = √25 = 5 = |−5|`.
The norm is the generalisation of absolute value from one number to a list.

**Why `‖cv‖ = |c|·‖v‖` needs the absolute value:** with `c = −2`, `v = (3,−4)`, the scaled
vector `(−6, 8)` has length `10`. And `|−2|·5 = 10` ✓, while `(−2)·5 = −10` ✗. **Lengths are
never negative** — the absolute value throws away the flip and keeps only the stretch.

---

## 3. Subscripts vs superscripts

**Subscripts are almost always labels — not operations.**

| Written | Means |
|---|---|
| `c₁`, `c₂` | "the first number", "the second number" — just names |
| `vᵢ` | the i-th **entry** of vector `v` |
| `λ₁`, `λ₂` | the first and second eigenvalue |
| `σ₁, σ₂, …` | the singular values, **ordered largest first** |
| `x₁, x₂, …, xₚ` | the 1st through p-th **columns** of a matrix |
| `Aₖ` | the rank-**k** truncated version of `A` |
| `‖v‖₁`, `‖v‖₂`, `‖v‖∞` | **which norm** — L1, L2, L-infinity |
| `Xc` | `X` **centered** (the c is a label, not multiplication!) |

**Superscripts are operations:**

| Written | Means |
|---|---|
| `v²`, `σ²` | squared |
| `Aᵀ` | **transpose** — flip rows and columns |
| `A⁻¹` | **inverse** — the matrix that undoes `A` |
| `Aⁿ` | `A` multiplied by itself n times |
| `ℝⁿ` | the space of lists of n real numbers |

> ⚠ `Aᵀ` and `A⁻¹` are **different things**. `Aᵀ` always exists (just flip it). `A⁻¹` often
> doesn't. Confusing them is item 4 on the instructor's self-check list.

---

## 4. Greek letters used in this course

| Letter | Name | Used for |
|---|---|---|
| `λ` | lambda | an **eigenvalue** — a stretch factor |
| `σ` | sigma (lowercase) | a **singular value** — always ≥ 0 |
| `Σ` | sigma (uppercase) | **two different things** — see below ⚠ |
| `θ` | theta | an **angle** |
| `Λ` | lambda (uppercase) | a diagonal matrix of eigenvalues |
| `∇` | nabla / del | **gradient** (Module 5, not on this exam) |

### ⚠ `Σ` means two different things — read the context

1. **Summation** when it has indices: `Σᵢ vᵢ` = "add up all the `vᵢ`"
2. **The matrix of singular values** in `A = UΣVᵀ` — a matrix, not a sum

You tell them apart by what's around it. `Σσᵢ²` = "sum of squared singular values" — the
first `Σ` is summation, the `σ`s are the values.

---

## 5. Summation notation

```
Σᵢ vᵢ  =  v₁ + v₂ + v₃ + ⋯
```

"Add up `vᵢ` for every `i`." It's a compact for-loop. Examples from this course:

| Written | Expanded, for `v = (3, 1)`, `w = (1, 2)` |
|---|---|
| `Σ\|vᵢ\|` | `\|3\| + \|1\| = 4` — the L1 norm |
| `√(Σ vᵢ²)` | `√(9 + 1) = √10` — the L2 norm |
| `u·w = Σuᵢwᵢ` | `(3)(1) + (1)(2) = 5` — the dot product |

---

## 6. Set and logic symbols

| Written | Read as | Meaning |
|---|---|---|
| `ℝ` | "the reals" | all ordinary numbers |
| `ℝⁿ` | "R-n" | lists of n real numbers — `(3,1) ∈ ℝ²` |
| `∈` | "is in" / "belongs to" | `v ∈ ℝ²` = "v is a 2-entry vector" |
| `C ∈ ℝ⁶ˣ¹⁰` | | C is a matrix with **6 rows, 10 columns** |
| `⟺` or "iff" | "if and only if" | both directions are true |
| `⟹` | "implies" | one direction only |
| `≥`, `≤` | | greater/less than or equal |
| `≈` | | approximately equal |
| `∀` | "for all" | (rare here) |

> **`m × n` is always rows × columns.** `ℝ⁶ˣ¹⁰` = 6 rows, 10 columns. Getting this backwards
> breaks every SVD dimension question.

---

## 7. Operations, in the order you meet them

| Written | Name | What to do |
|---|---|---|
| `u + w` | addition | add entry by entry |
| `cv` | scalar multiplication | multiply every entry by `c` |
| `c₁u + c₂w` | **linear combination** | scale each, then add |
| `u · w` | **dot product** | multiply matching entries, sum them → a **number** |
| `Av` | matrix-vector product | → a **vector** |
| `AB` | matrix product | inner dimensions must match |
| `Aᵀ` | transpose | rows ↔ columns |
| `dist(u,w) = ‖u−w‖` | distance | norm of the difference |
| `projₐ(b)` | projection | closest point to `b` on the line through `a` |

> **Watch the output type.** `u·w` gives a **number**. `Av` gives a **vector**. `‖v‖` gives a
> **number**. Tracking types catches most algebra errors before they happen.

---

## 8. Symbols specific to later topics

| Written | Meaning |
|---|---|
| `Av = λv` | the **eigenvector equation** — A only stretches v |
| `det(A)` | determinant — the area scale factor |
| `tr(A)` | trace — sum of the diagonal |
| `I` | identity matrix — `[[1,0],[0,1]]`, the "do nothing" matrix |
| `A − λI` | subtract λ from each **diagonal** entry only |
| `A = PDP⁻¹` | diagonalization |
| `A = UΣVᵀ` | **SVD** |
| `Aₖ = UₖΣₖVₖᵀ` | rank-k truncated SVD |
| `‖A − Aₖ‖_F` | **Frobenius** norm of the error |
| `x̂` ("x-hat") | an **estimate** — the fitted/approximate answer, not the true one |
| `x̄` ("x-bar") | a **mean/average** |
| `Xc` | centered data (mean subtracted) |

---

## 8b. Words that mean the same thing

The lectures, the books and the assignment all swap between these. They are **not**
different concepts.

| These words | all mean |
|---|---|
| **coefficient** = **weight** = **parameter** | the scalar in front of something, saying how much of it to take |
| **feature** = **variable** = **predictor** = **column** | one measured quantity describing a sample |
| **sample** = **observation** = **data point** = **row** | one thing you measured |
| **norm** = **length** = **magnitude** = **size** | how big a vector is |
| **orthogonal** = **perpendicular** = **at right angles** | dot product is 0 |
| **singular matrix** = **not invertible** = **det = 0** = **not full rank** | a dimension was collapsed |
| **rank-deficient** = **multicollinear** (in data) | redundant columns |
| **explained variance** (PCA) = **energy retained** (SVD) | the same ratio |

**"Coefficient" is worth pinning down** because it shows up in three different places:

- `c₁u + c₂w` — a **linear combination**: `c₁`, `c₂` are coefficients
- `c = (a·b)/(a·a)` — the **projection** coefficient
- `price = w₁·size + w₂·age` — the model's **regression** coefficients (= the weight vector)

Same role every time: *the scalar in front, telling you how much of that thing to use.*
"Zeroing a coefficient" therefore means deleting that term from the model entirely.

---

## 9. Things that look like multiplication but aren't

This trips people up constantly:

| Written | **Not** multiplication — it's... |
|---|---|
| `Xc` | `X` **centered** (subscript c is a label) |
| `f(v)` | `f` **applied to** `v` |
| `Av` | `A` **applied to** `v` (a function call, not a product of numbers) |
| `A²` | `A` times itself — but `AB ≠ BA` in general, so order matters |
| `λI` | this one **is** scalar × matrix — scale the identity |

---

## 10. Quick self-test

Say out loud what each of these means. If you can, you're ready for Topic 3.

1. `‖u − w‖₂`
2. `C ∈ ℝ⁶ˣ¹⁰`
3. `σ₂`
4. `|c|·‖v‖`
5. `AᵀA`
6. `Σᵢ σᵢ²`
7. `Av = λv`
8. `x̂ = (AᵀA)⁻¹Aᵀb`

<details>

1. The Euclidean (straight-line) distance between vectors `u` and `w`.
2. C is a matrix with 6 rows and 10 columns.
3. The second-largest singular value.
4. Absolute value of the scalar `c`, times the length of vector `v`.
5. A-transpose times A — a square, symmetric matrix, always.
6. The sum of all squared singular values — the total **energy**.
7. A applied to v gives back v scaled by λ — v is an eigenvector.
8. The least-squares estimate: the coefficients projecting `b` onto A's column space.

</details>
