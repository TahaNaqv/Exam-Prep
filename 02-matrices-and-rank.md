# Topic 2 — Matrices & Rank

## 1. A matrix is a function

Forget "grid of numbers". The lecture's actual definition:

> **A matrix is a function that takes a vector in and gives a vector out.**

`Av` is `A` *applied to* `v`. Same idea as `f: ℝⁿ → ℝᵐ` from Day 1. `A` is what that
function looks like written down.

### The row picture (the mechanical way)

`A = [[2, 1], [3, 4]]`, `v = (5, 2)`

Row 1: (2)(5) + (1)(2) = 12
Row 2: (3)(5) + (4)(2) = 23
→ `Av = (12, 23)`

Each output entry = that row dotted with `v`.

### The column picture (the way that actually explains things)

`A`'s columns are `(2,3)` and `(1,4)`. `v = (5,2)` says: **"take 5 of the first column,
2 of the second."**

    5·(2,3) + 2·(1,4) = (10,15) + (2,8) = (12,23)   ← same answer

> **Av is a linear combination of A's columns, weighted by v's entries.**

In ML notation, with a data matrix `X` and weights `w`:

    Xw = w₁x₁ + w₂x₂ + ⋯ + wₚxₚ

This is Topic 1's "linear combination" doing real work for the first time. **Learn the
column picture.** Everything from here — column space, rank, span, SVD — is built on it.

**Column space** = the set of every output `Av` could possibly produce, over all possible `v`
= everything A's columns can build by linear combination. (Also called the *span* of the columns.)

## 2. What "linear" actually means

A matrix isn't just any function — it's a **linear transformation**:

- **Additivity:** `A(u + w) = Au + Aw`
- **Homogeneity:** `A(cu) = c·Au`

Together: `A(c₁u + c₂w) = c₁Au + c₂Aw`. **A combination of inputs maps to the same
combination of outputs.** That is the definition of linear.

**Not every simple function is linear.** The lecture's counterexample, worth remembering
because "simple" and "linear" are not synonyms:

`f(v) = v + (1,1)` — just a shift.
- `f(u) + f(w) = u + w + (2,2)`
- `f(u + w) = u + w + (1,1)`

Not equal → **a shift is not linear.**

**Why this matters in ML (good exam one-liner):** every neural-network layer is a linear
map (matrix multiply) followed by a **nonlinear** activation. If you stacked linear layers
alone they'd collapse into a single matrix and depth would buy you nothing. The
nonlinearity is the entire reason deep networks work.

## 3. What a matrix does geometrically

Because the map is linear, a straight edge must map to a straight edge (an edge is a
combination of its two endpoints). So to see what a matrix does to a square, **you only
have to check the 4 corners** and join them up.

- A rotation matrix `R = [[cos θ, −sin θ], [sin θ, cos θ]]` turns the square. Same shape,
  same size, just rotated.
- **Composition:** "rotate, then scale" is a single matrix `SR`. Not a new rule — just
  multiplication. **Order matters: `SR ≠ RS` in general.**

## 4. Mechanics you must not fumble

**Matrix multiplication** — inner dimensions must match:
`(m×n)(n×p) → (m×p)`

    [1 2 0]   [2 0]     [4  6]
    [3 1 4] × [1 3]  =  [7  7]
              [0 1]
    (2×3)   ×  (3×2)  →  (2×2)

**Transpose `Aᵀ`** — flip across the diagonal; rows become columns:

    [1 2 3]ᵀ    [1 4]
    [4 5 6]  =  [2 5]
                [3 6]

An `m×n` matrix transposes to `n×m`. `Aᵀ` is not bookkeeping — `AᵀA` and `Aᵀb` are
everywhere in projection, regression and gradients.

## 5. Linear independence

The question: **does this vector add a genuinely new direction, or could I have built it
from the ones I already have?**

- `v₁ = (1,2)`, `v₂ = (2,4)`: is `v₂ = c·v₁`? Yes, `c = 2`. → **dependent.**
  `v₂` adds nothing.
- `v₁ = (1,2)`, `v₃ = (3,1)`: we'd need `c = 3` (first entry) *and* `c = 0.5` (second
  entry) simultaneously. Impossible. → **independent.** A genuinely new direction.

> **A set is linearly dependent exactly when one vector can be written as a linear
> combination of the others.**

**How to test on paper (2 vectors):** try to scale one into the other. If a single
constant works for *every* coordinate, dependent. If not, independent.

## 6. Rank

> **rank(A) = the dimension of A's column space
> = how many of its columns are genuinely independent
> = how much non-redundant information the matrix carries.**

Example: `M = [[2,1],[4,2]]`. Column 2 = ½ × column 1. Two columns, but a basis for what
they span needs only **one** vector. So `rank(M) = 1`, even though `M` is 2×2.

### ⚠ THE TRAP — the instructor called this out explicitly

18% of the class got this wrong on the diagnostic, and it's item #1 on his self-check list:

> **Rank has nothing to do with shape.**

- A 2×3 matrix can be **full rank** (rank 2).
- A square 2×2 matrix can be **rank-deficient** (rank 1).
- **Independence is the test. Shape never was.**

### Terminology (get these right, they're worth marks)

- **Max possible rank** of an m×n matrix is `min(m, n)`.
- **Rank-deficient** = `rank(A) < min(m, n)`.
- **Full column rank** = rank equals the *number of columns*. (Needs rank = n, so
  requires m ≥ n — a wide matrix can never have full column rank.)
- **Full row rank** = rank equals the number of rows.
- **Singular matrix** = a square matrix with no inverse (i.e. not full rank).
- **Multicollinearity** = the real-data name for rank deficiency in your feature columns.
  E.g. you add a "total score" column that is the sum of existing columns — one more
  column, zero more rank, zero more information.

These are different! A matrix can be *not* rank-deficient by the `min(m,n)` definition and
still *lack full column rank*. Exam question 2 turns on exactly this distinction.

## 7. Rank-deficiency destroys information

Apply `M = [[2,1],[4,2]]` (rank 1) to every point of a square. Common guess: it shrinks.
What actually happens: **the entire 2D square is flattened onto a single line.**

It isn't shrunk — a whole dimension is **destroyed**. And it's **not reversible**: once
many different input points have been mapped onto the same output point, no matrix can
undo it. That is exactly why:

- `Ax = b` sometimes has **no** clean solution,
- least squares exists at all,
- and `det = 0` ⟺ no inverse (Topic 4).

## 8. The key result that closes the loop

> **`AᵀA` is invertible if and only if `A` has full column rank.**

Fitting a linear model requires inverting `AᵀA` (you'll see why in Topic 3). So:
redundant/collinear features → not full column rank → `AᵀA` singular → **no unique fit.**

You are expected to be able to argue "is `AᵀA` invertible?" **without computing a
determinant** — just check whether the columns of A are independent.

Useful companion fact: `rank(AᵀA) = rank(A)`.

## 8b. WORKED EXAMPLES — study these before the drills

---

### Worked Example 1 — computing `Av` (both methods, in full)

> **Problem:** `A = [[2, 1], [3, 4]]`, `v = (5, 2)`. Compute `Av` by rows, then again by
> columns.

**Step 0 — decode the notation.** In `[[2, 1], [3, 4]]`, **each inner bracket is one row**:

```
        col1  col2
       ┌           ┐
row1   │  2    1   │
row2   │  3    4   │
       └           ┘
```

And `v` stands **upright** when you compute:

```
     ┌   ┐
v =  │ 5 │
     │ 2 │
     └   ┘
```

> Write `v` vertically on paper. Most errors in this operation come from leaving it flat.

**Step 1 — check the dimensions FIRST.** `A` is 2×2, `v` is 2×1.

```
(2 × 2)(2 × 1)
    ↑___↑        these must match  →  2 = 2  ✓
 ↑_______↑       these give the answer's shape  →  2×1
```

So the answer will be a **vector with 2 entries**. Knowing this before you start means you
notice immediately if you produce something else.

**Step 2 — row method: one row of A gives one entry of the answer.**

*Entry 1, from row 1 = `[2, 1]`:*

```
   row 1  →  [ 2 ,  1 ]
               ×     ×        multiply aligned pairs
   v      →  [ 5 ,  2 ]

   2 × 5 = 10
   1 × 2 =  2
          ──────
            12
```

*Entry 2, from row 2 = `[3, 4]`.* Note `v` does **not** change — only the row moves:

```
   row 2  →  [ 3 ,  4 ]
               ×     ×
   v      →  [ 5 ,  2 ]

   3 × 5 = 15
   4 × 2 =  8
          ──────
            23
```

**Step 3 — assemble the answer.**

```
       ┌    ┐
Av  =  │ 12 │     =  (12, 23)
       │ 23 │
       └    ┘
```

Two entries, as Step 1 predicted ✓

> ⚠ **The answer is a vector, not a number.** Do not add 12 and 23 together.

**Step 4 — column method: read A downwards instead.**

```
col1 = (2, 3)        col2 = (1, 4)
```

> Careful: column 1 is `(2, 3)` read **down**. Row 1 is `(2, 1)` read **across**. Different
> vectors — this is the most common mix-up.

`v = (5, 2)` now reads as an instruction: **"take 5 of column 1, plus 2 of column 2."**

```
Av = 5·(2, 3)  +  2·(1, 4)
   = (10, 15)  +  (2, 8)
   = (10+2, 15+8)
   = (12, 23)          ← identical ✓
```

**Step 5 — why they always agree.** Write `v = 5e₁ + 2e₂`. Linearity gives

```
Av = A(5e₁ + 2e₂) = 5(Ae₁) + 2(Ae₂)
```

and `Ae₁` is exactly column 1, `Ae₂` is exactly column 2. So `Av` **must** be a weighted sum
of A's columns — guaranteed for every matrix, not a coincidence of these numbers.

> **Answer:** `Av = (12, 23)`. The column picture is the one to remember: **`Av` is a linear
> combination of A's columns, weighted by v's entries.**

---

### Worked Example 2 — rank and a dependence relation

> **Problem:** `B = [[1, 2, 3], [2, 4, 6]]`. Find rank(B), give a dependence relation, and
> state whether B is rank-deficient and whether it has full column rank.

**Step 1 — write out the columns.** Read **downwards**:

```
c₁ = (1, 2)      c₂ = (2, 4)      c₃ = (3, 6)
```

**Step 2 — test each pair for dependence.** For two vectors, ask: *is one a scalar multiple
of the other?* Check whether the ratio is the same in every coordinate.

```
c₂ vs c₁:   2/1 = 2   and   4/2 = 2   →  same ratio  →  c₂ = 2·c₁    DEPENDENT
c₃ vs c₁:   3/1 = 3   and   6/2 = 3   →  same ratio  →  c₃ = 3·c₁    DEPENDENT
```

**Step 3 — count what's genuinely independent.** Every column is a multiple of `c₁`. So a
basis for what they span needs just **one** vector.

```
rank(B) = 1
```

**Step 4 — give a dependence relation.** Any one of these is a valid answer:

```
c₂ = 2·c₁        or        c₃ = 3·c₁        or        c₃ = c₁ + c₂
```

*(Check the third: `(1,2) + (2,4) = (3,6)` ✓)*

**Step 5 — rank-deficient?** B is 2×3, so the maximum possible rank is
`min(m, n) = min(2, 3) = 2`. We found rank 1.

```
1 < 2   →  YES, B is rank-deficient
```

**Step 6 — full column rank?** Full column rank means rank = **number of columns** = 3. We
have rank 1.

```
1 ≠ 3   →  NO, B does not have full column rank
```

> **Answer:** rank 1; `c₂ = 2c₁` (for instance); rank-deficient **yes**; full column rank **no**.

---

### Worked Example 3 — the trap: full rank vs full column rank

> **Problem:** `A = [[1, 0, 2], [0, 1, 3]]`. Same four questions.

**Step 1 — columns:** `c₁ = (1,0)`, `c₂ = (0,1)`, `c₃ = (2,3)`.

**Step 2 — independence.** `c₁` and `c₂` are clearly independent — neither is a multiple of
the other (`(1,0)` has a zero where `(0,1)` doesn't).

**Step 3 — can `c₃` be built from them?** Look for `c₃ = a·c₁ + b·c₂`:

```
a·(1,0) + b·(0,1) = (a, b)     and we want (2, 3)
→  a = 2,  b = 3
→  c₃ = 2c₁ + 3c₂     ✓  DEPENDENT
```

**Step 4 — rank.** Two independent columns, third redundant → **rank(A) = 2**.

**Step 5 — rank-deficient?** Max possible is `min(2,3) = 2`. We have rank 2.

```
2 is NOT less than 2   →  NO, A is NOT rank-deficient — it is FULL RANK
```

**Step 6 — full column rank?** Needs rank = 3 columns. We have 2.

```
→  NO, A does NOT have full column rank
```

> ⚠ **This is the trap the instructor is hunting for.** The same matrix is **full rank**
> *and* **lacks full column rank**. These are different questions with opposite answers.
> A wide matrix (more columns than rows) can *never* have full column rank — you cannot fit
> 3 independent vectors into ℝ².

**Step 7 — is `AᵀA` invertible? (without a determinant)** Use the rule:

> `AᵀA` is invertible **if and only if** A has full column rank.

A does **not** have full column rank (Step 6), so **`AᵀA` is singular — not invertible.**
Supporting detail: `AᵀA` is 3×3, but `rank(AᵀA) = rank(A) = 2 < 3`.

---

### Worked Example 4 — is this function linear?

> **Problem:** Decide whether `f(v) = 4v` and `g(v) = v + (3, 0)` are linear.

**Step 1 — know the test.** A function is linear when **both** hold for all inputs:

```
additivity:    f(u + w) = f(u) + f(w)
homogeneity:   f(cu)    = c·f(u)
```

To prove linear you must show both. **To prove NOT linear you need only one failure.**

**Step 2 — test `f(v) = 4v`.**

```
additivity:    f(u + w) = 4(u + w) = 4u + 4w
               f(u) + f(w) = 4u + 4w          ✓  equal

homogeneity:   f(cu) = 4(cu) = 4cu
               c·f(u) = c(4u) = 4cu           ✓  equal
```

> **`f` is linear.**

**Step 3 — test `g(v) = v + (3, 0)`.** Compute each side separately and compare.

```
left side:   g(u + w) = (u + w) + (3, 0)  =  u + w + (3, 0)

right side:  g(u) + g(w) = [u + (3,0)] + [w + (3,0)]
                         =  u + w + (6, 0)
```

```
u + w + (3,0)   ≠   u + w + (6,0)
```

The shift got applied **twice** on the right and **once** on the left.

> **`g` is NOT linear** — additivity fails. A shift is simple, but simple ≠ linear.

**Step 4 — the ML point.** Every neural-network layer is a matrix multiply (linear) followed
by a **nonlinear** activation. Stack linear layers alone and they collapse into one single
matrix, so depth would buy nothing. The nonlinearity is the entire reason depth works.

---

### Worked Example 5 — rank in REAL data (multicollinearity)

Same idea as Examples 2 and 3, but the matrix arrives as a **dataset** instead of a grid of
abstract numbers. The method is identical — only the packaging changed.

> **Problem:** A dataset records `exam_math`, `exam_physics`, and `total_score`, where
> `total_score = exam_math + exam_physics`. What is the rank of this 3-column matrix at most,
> what is the problem called, and why does it matter?

**Step 1 — recognise that a dataset IS a matrix.** Rows = students, columns = features:

```
            math    physics    total
         ┌                             ┐
stu 1    │  40        30        70     │
stu 2    │  55        25        80     │
stu 3    │  60        35        95     │
         └                             ┘
```

**Step 2 — write out the columns.** Read **downwards**, as always:

```
c₁ = (40, 55, 60)      ← math
c₂ = (30, 25, 35)      ← physics
c₃ = (70, 80, 95)      ← total
```

**Step 3 — test for a dependence relation.** With three columns, look for one being built
from the others (not just a multiple of one):

```
c₁ + c₂ = (40+30, 55+25, 60+35) = (70, 80, 95)  =  c₃   ✓
```

```
c₃ = c₁ + c₂       →   DEPENDENT
```

> This is the definition straight from §5: *a set is linearly dependent exactly when one
> vector can be written as a linear combination of the others.* Here the coefficients are
> just 1 and 1.

**Step 4 — count the rank.**

```
c₁  →  genuinely new                       ✓ counts
c₂  →  genuinely new (not a multiple of c₁) ✓ counts
c₃  →  entirely built from c₁ and c₂        ✗ adds nothing
```

```
rank = 2      (despite there being 3 columns)
```

**Step 5 — why "at most".** We can prove `c₃` is redundant from the *definition* of
`total_score`, whatever the numbers happen to be. We cannot prove `c₁` and `c₂` are
independent without checking the actual data — if every student happened to score the same
in both subjects, the rank would fall to 1.

> **"At most 2"** is the claim you can defend: we know for certain that **at least one**
> column is redundant.

**Step 6 — name the problem.**

> **Multicollinearity** — the real-data name for rank deficiency in a feature matrix.

**Step 7 — why it matters. Make the failure concrete.**

Fit a model using all three columns:

```
prediction = w₁·math + w₂·physics + w₃·total
```

Substitute `total = math + physics`:

```
= w₁·math + w₂·physics + w₃·(math + physics)
= (w₁ + w₃)·math  +  (w₂ + w₃)·physics
```

**Only the sums `(w₁ + w₃)` and `(w₂ + w₃)` affect the prediction.** So:

| `w₁` | `w₂` | `w₃` | `w₁+w₃` | `w₂+w₃` | prediction |
|---|---|---|---|---|---|
| 2 | 3 | 0 | 2 | 3 | identical |
| 0 | 1 | 2 | 2 | 3 | identical |
| 5 | 6 | −3 | 2 | 3 | identical |
| −8 | −7 | 10 | 2 | 3 | identical |

> **All of these are equally good fits**, and there are infinitely many more. The data cannot
> tell the model how to divide credit between `total` and its two ingredients. That is what
> "**no unique solution**" means — not failure, but infinitely many tied answers, which makes
> the individual coefficients meaningless.

**Step 8 — state the chain (this is the marks-earning sentence).**

```
c₃ = c₁ + c₂
      ↓
X does not have full column rank
      ↓
XᵀX is singular — not invertible
      ↓
x̂ = (XᵀX)⁻¹Xᵀb  breaks:  no unique solution
```

> **Answer:** rank at most **2**; the problem is **multicollinearity**; it makes `XᵀX`
> singular so the least-squares fit has no unique solution.

**Step 9 — spot it in the wild.** The same trap, differently dressed:

| Pattern | Example |
|---|---|
| A **total** of other columns | `total = math + physics` |
| The **same quantity in two units** | `height_cm` and `height_m` (`c₂ = 0.01·c₁`) |
| A **linear rescaling** | °C and °F (`F = 1.8C + 32`) |
| **Percentages** summing to 100 | `%A + %B + %C = 100` |
| **Dummy variable trap** | one-hot columns where the last = 1 − (sum of others) |

Your instructor's Session 2 phrasing: *"a composite score column built from existing columns
adds a column but zero rank."*


### What to notice across all five

- **Row picture** = read across, one row → one output entry.
- **Column picture** = read down, `v`'s entries are the amounts of each column.
- **Rank** = how many columns are genuinely independent. **Never about shape.**
- **Full rank** and **full column rank** are different claims — check both separately.
- A **dataset is just a matrix**; "multicollinearity" is only rank deficiency with a
  real-world name on it.

Now do the drills.


## 9. Drills

**D1.** `A = [[1,0,1],[0,1,1]]`, `z = (2,−1,3)`.
(a) Compute `Az` row-by-column.
(b) Recompute it as a linear combination of A's columns.
(c) What is rank(A)? Give one dependence relation among the columns.
(d) Is A rank-deficient in the `rank < min(m,n)` sense? Does it have full *column* rank?
(e) Without a determinant: is `AᵀA` invertible?

**D2.** Is `f(v) = 3v` linear? Is `f(v) = v + (2,0)`? Prove each in one line.

**D3.** `B = [[1,2],[2,4]]`. Find rank(B). What does B do to a square, geometrically?

**D4.** True or false, with a reason: "A 3×5 matrix must be rank-deficient."

**D5.** A dataset has columns `height_cm`, `height_m`, `weight`. What is the rank of that
3-column matrix at most, and what's the real-data name for the problem?

---

### Answers

**D1.**
(a) Row 1: (1)(2)+(0)(−1)+(1)(3) = 5. Row 2: (0)(2)+(1)(−1)+(1)(3) = 2. → `Az = (5, 2)`.
(b) `2·(1,0) + (−1)·(0,1) + 3·(1,1) = (2,0) + (0,−1) + (3,3) = (5,2)` ✓ same.
(c) Columns are `(1,0)`, `(0,1)`, `(1,1)`. The first two are independent, and
`col₃ = col₁ + col₂`. So **rank(A) = 2**.
(d) A is 2×3, so `min(m,n) = 2`, and rank = 2. **Not rank-deficient** — it is full rank.
But it has **3 columns and rank 2**, so it does **not** have full column rank.
*(This is the trap: full rank ≠ full column rank.)*
(e) `AᵀA` is 3×3 but `rank(AᵀA) = rank(A) = 2 < 3`. A needs full column rank for `AᵀA`
to be invertible, and it doesn't have it. **`AᵀA` is singular — not invertible.**

**D2.** `f(v) = 3v` is linear: `f(u+w) = 3(u+w) = 3u + 3w = f(u)+f(w)` ✓ and
`f(cu) = 3cu = c·f(u)` ✓.
`f(v) = v + (2,0)` is **not**: `f(u+w) = u+w+(2,0)` but `f(u)+f(w) = u+w+(4,0)`. Not equal.

**D3.** `col₂ = 2·col₁`, so **rank = 1**. It flattens the whole square onto the single
line spanned by `(1,2)` — a dimension is destroyed and the map is not invertible.

**D4.** **False.** Max rank is `min(3,5) = 3`, and a 3×5 matrix can perfectly well have
rank 3 (full rank). Shape is not the test; independence is. *(It can never have full
**column** rank, though — 5 columns can't be independent in ℝ³.)*

**D5.** `height_m = height_cm / 100`, so those two columns are dependent. Rank is **at
most 2**. The name is **multicollinearity** (rank deficiency in the feature matrix), and
it makes `XᵀX` singular, so the regression has no unique solution.
