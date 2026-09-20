# Topic 8 — Principal Component Analysis (PCA)

> **PCA is just SVD on centered data. Same math, one preprocessing step.**

That single sentence is the whole topic. Everything else is detail.

## 1. The problem PCA solves

You have 50 measurements per customer. You want to plot customers on a 2D chart to see
patterns. **You can't plot 50 dimensions.** What do you do?

You can't just pick 2 of the 50 columns — you'd throw away 48. Instead, find **new
directions** (combinations of all 50 features) that carry as much of the variation as
possible, and plot those.

> **The direction of maximum variance is the eigenvector of the covariance matrix with the
> largest eigenvalue. That's the first principal component (PC1).**
>
> PC2 is the next direction of most variance **orthogonal to PC1**, and so on.

**Why variance?** Variance = spread = how much the data points actually differ from each
other. A direction where all points look identical tells you nothing; a direction where
they spread out is where the information lives.

## 2. The procedure

    Center the data  →  Xc  →  SVD  →  UΣVᵀ  →  principal components

1. **Center**: subtract the column mean from each column, giving `Xc`. (Every column of
   `Xc` now has mean 0.)
2. **SVD**: `Xc = UΣVᵀ`.
3. **Read off:**
   - **V's columns = the principal component directions** (the new axes, in terms of the
     original features)
   - **σᵢ² = the variance captured** by component i (up to a constant scale factor)
   - **UΣ = the scores** — your data points' coordinates in the new PC system (what you plot)

**Why SVD instead of eigendecomposing the covariance matrix?** Same answer, but SVD of the
centered data is **more numerically stable**, and it's the exact tool the module spent
three sessions building. Many textbooks teach the covariance route first; it is not a
different method.

### ⚠ Why centering is mandatory

Variance is defined as spread **around the mean**. If you don't subtract the mean, the
first "principal component" just points at where the cloud of data sits relative to the
origin, not at how it's spread out — you'd measure position instead of variation. Item #7
on the instructor's self-check list.

### ⚠ The practical gotcha: centering isn't always enough

If features are on **very different scales** — income in dollars (values ~50,000) vs age
in years (values ~40) — income's raw variance dwarfs age's for reasons of **units alone**,
and PC1 will just point along income no matter what the data means.

> **Fix: standardise** — center, **then divide each column by its standard deviation**.
> Usually the right move when feature scales differ.

### ⚠ Sign ambiguity carries over

Principal components, like singular vectors generally, are defined only **up to a sign
flip**. A component and its negation represent the **same direction**. If your plot comes
out mirrored compared to a classmate's, neither of you is wrong.

## 3. Worked example (Assignment Q9 — do this one until it's automatic)

    X = [ 1  1 ]
        [ 2  3 ]
        [ 3  2 ]
        [ 4  4 ]

**Step 1 — mean and centering.**
Column means: `(1+2+3+4)/4 = 2.5`, `(1+3+2+4)/4 = 2.5`. Mean vector `= (2.5, 2.5)`.

    Xc = [ −1.5  −1.5 ]
         [ −0.5   0.5 ]
         [  0.5  −0.5 ]
         [  1.5   1.5 ]

*(Check: each column now sums to 0. Always verify this — it catches arithmetic slips.)*

**Step 2 — the shortcut `XcᵀXc = VΣ²Vᵀ`.**
Rather than a full SVD by hand, eigendecompose the small 2×2 matrix `XcᵀXc`. Its
eigenvectors are **V** (the PC directions) and its eigenvalues are **σ²**.

    XcᵀXc = [ Σx²   Σxy ]
            [ Σxy   Σy² ]

- `Σx² = 2.25 + 0.25 + 0.25 + 2.25 = 5`
- `Σy² = 2.25 + 0.25 + 0.25 + 2.25 = 5`
- `Σxy = 2.25 + (−0.25) + (−0.25) + 2.25 = 4`

    XcᵀXc = [ 5  4 ]
            [ 4  5 ]

**Eigenvalues** (use the Topic 4 shortcut): `tr = 10`, `det = 25 − 16 = 9`.
`λ² − 10λ + 9 = 0` → `(λ−9)(λ−1) = 0` → **λ₁ = 9, λ₂ = 1**

**Eigenvectors:**
- λ=9: `XcᵀXc − 9I = [[−4,4],[4,−4]]` → `v₂ = v₁` → **(1, 1)**, normalised `(1/√2)(1,1)`
- λ=1: `XcᵀXc − 1I = [[4,4],[4,4]]` → `v₂ = −v₁` → **(1, −1)**, normalised `(1/√2)(1,−1)`

*(They're orthogonal — as they must be, since `XcᵀXc` is symmetric.)*

**Step 3 — PC1.** The eigenvector with the larger eigenvalue: **PC1 = (1,1)/√2**, the
45° diagonal.

**Does it match the data?** Yes. Plot the four points (1,1), (2,3), (3,2), (4,4): they
run up the diagonal from bottom-left to top-right. The direction of greatest spread is
clearly along `(1,1)`. **Always write this sentence** — the instructor's self-check item #8
is "have I interpreted the calculations rather than only reporting them".

**Step 4 — variance explained.**
- PC1: `9 / (9+1) = **90%**`
- PC2: `1 / (9+1) = **10%**`

So a single number per point (its position along the diagonal) retains 90% of the
structure of this 2-column dataset.

**Step 5 — the covariance route (Q9 part 5).**
The covariance matrix is `Σcov = (1/n)·XcᵀXc` (some texts use `1/(n−1)`). Multiplying a
matrix by a scalar **does not change its eigenvectors** — it multiplies every eigenvalue by
that scalar.

> **The principal component directions are identical.** The eigenvalues are scaled by
> `1/n`: here `9/4 = 2.25` and `1/4 = 0.25` (or `9/3 = 3` and `1/3` with the `n−1`
> convention).

Crucially, the **proportions are unchanged**: `2.25/2.5 = 90%`, exactly as before. The
scaling cancels in the ratio. That's why both routes give the same PCA.

**Step 6 — the connection to "energy" (Q9 part 6).**
The **eigenvalues of `XcᵀXc` play exactly the role that the squared singular values `σᵢ²`
played in Topic 7** — because they literally *are* the squared singular values
(`XcᵀXc = VΣ²Vᵀ`).

> "Explained variance" in PCA **is** "proportion of energy retained" in truncated SVD.
> Same quantity, two vocabularies, because PCA *is* SVD on centered data.

## 4. Demo: the Iris dataset (know the story, it's a likely discussion question)

150 flowers, 4 measurements each, 3 species. Run PCA on the measurements **without ever
telling it the species.**

**Result:** projected to 2D, **one species splits off cleanly; the other two overlap.**

> That overlap is **not a failure of PCA.** It's an honest reflection of the data: those
> two species really are more similar to each other on these four measurements.

If asked to comment on a PCA plot, that's the mature answer — distinguish "the method
failed" from "the method correctly reported that the groups overlap."

## 5. Choosing k — you already know this (Topic 7, unchanged)

1. **Cumulative variance** — smallest k crossing 90% or 95%.
2. **Elbow method** — where the **scree plot** (eigenvalues/variance vs component number)
   visibly flattens.
3. **Domain constraints** — "I need to visualise this, so k = 2 or 3, full stop."
4. **Error vs compression** — no free lunch.

*(A **scree plot** is just the bar/line chart of variance explained per component. Know the
word.)*

## 6. Where PCA is used

- **Exploratory data analysis** — the first thing many data scientists do with a new
  high-dimensional dataset, before any modelling.
- **Feature engineering** — feed principal components into a downstream model, especially
  when the original features are correlated (recall multicollinearity, Topic 2).
- **Visualisation** — the standard way to "see" a high-dimensional dataset at all.

## 7. When PCA is the wrong tool

1. **Sensitive to outliers** — one extreme point can dominate a principal component
   (variance is a squared quantity, so outliers count heavily).
2. **Assumes linear relationships** — genuinely nonlinear structure needs **t-SNE** or
   **UMAP** instead.
3. **Requires numeric features** — categorical data must be encoded first, and naive
   one-hot encoding interacts awkwardly with PCA's variance-based logic.

## 8. The whole module in one line

> **Session 1:** special directions exist → **Session 2:** every matrix has them (SVD) →
> **Session 3:** best possible summary (Eckart–Young) → **Session 4:** here's how it's used (PCA).
>
> **Find the structure. Keep the best part. Use it.**

## 8b. WORKED EXAMPLES — study these before the drills

---

### Worked Example 1 — complete PCA by hand, all six steps

> **Problem:** Four data points as rows of `X`:
> `(2,2), (4,6), (6,4), (8,8)`.
> Find the mean, `Xc`, `XcᵀXc`, its eigenvalues and eigenvectors, PC1, the variance
> explained, and the score of each point on PC1.

**Step 1 — compute the mean of each COLUMN.**

Stack the data and average down each column separately:

```
        x     y
      ┌         ┐
      │  2   2  │
X  =  │  4   6  │
      │  6   4  │
      │  8   8  │
      └         ┘

x-mean = (2 + 4 + 6 + 8)/4 = 20/4 = 5
y-mean = (2 + 6 + 4 + 8)/4 = 20/4 = 5

mean = (5, 5)
```

> **Columns, not rows.** You're finding the average of each *feature*.

**Step 2 — center: subtract the mean from every row.**

```
row 1:  (2−5, 2−5) = (−3, −3)
row 2:  (4−5, 6−5) = (−1,  1)
row 3:  (6−5, 4−5) = ( 1, −1)
row 4:  (8−5, 8−5) = ( 3,  3)

        ┌           ┐
Xc  =   │ −3    −3  │
        │ −1     1  │
        │  1    −1  │
        │  3     3  │
        └           ┘
```

**Step 3 — CHECK: every column must now sum to zero.**

```
column 1:  −3 − 1 + 1 + 3 = 0   ✓
column 2:  −3 + 1 − 1 + 3 = 0   ✓
```

> If a column doesn't sum to zero, your mean or your subtraction is wrong. **Do this check
> every time** — it catches the error before it poisons everything downstream.

**Step 4 — compute `XcᵀXc`.** For 2 columns it always has this shape:

```
            ┌                 ┐
XcᵀXc  =    │  Σx²      Σxy   │
            │  Σxy      Σy²   │
            └                 ┘
```

where the sums run down the **centered** columns. Work them out one at a time:

```
Σx²  = (−3)² + (−1)² + (1)² + (3)²  =  9 + 1 + 1 + 9  =  20

Σy²  = (−3)² + ( 1)² + (−1)² + (3)² =  9 + 1 + 1 + 9  =  20

Σxy  = (−3)(−3) + (−1)(1) + (1)(−1) + (3)(3)
     =    9     +   (−1)  +  (−1)   +   9
     =  16
```

```
            ┌            ┐
XcᵀXc  =    │  20    16  │
            │  16    20  │
            └            ┘
```

**Step 5 — eigenvalues.** Use the Topic 4 shortcut `λ² − tr·λ + det = 0`:

```
tr  = 20 + 20 = 40
det = (20)(20) − (16)(16) = 400 − 256 = 144

λ² − 40λ + 144 = 0
```

Two numbers multiplying to 144 and adding to 40: **36 and 4**.

```
(λ − 36)(λ − 4) = 0   →   λ₁ = 36,  λ₂ = 4
```

> **Handy shortcut for this shape:** whenever the matrix looks like `[[a, b], [b, a]]`, the
> eigenvalues are simply `a + b` and `a − b`. Here `20 + 16 = 36` and `20 − 16 = 4` ✓ — and
> the eigenvectors are **always** `(1,1)` and `(1,−1)`. This shape appears constantly in PCA
> questions.

**Step 6 — eigenvectors.**

```
λ = 36:   XcᵀXc − 36I = [ −16   16 ]   →  −16v₁ + 16v₂ = 0  →  v₂ = v₁   →  (1, 1)
                        [  16  −16 ]

λ = 4:    XcᵀXc −  4I = [  16   16 ]   →   16v₁ + 16v₂ = 0  →  v₂ = −v₁  →  (1, −1)
                        [  16   16 ]
```

Check they're orthogonal: `(1,1)·(1,−1) = 1 − 1 = 0` ✓ (guaranteed — `XcᵀXc` is symmetric).

**Step 7 — PC1 is the eigenvector with the LARGER eigenvalue.**

```
λ₁ = 36 is larger   →   PC1 = (1, 1)
```

As a **unit** vector (divide by `‖(1,1)‖ = √2`):

```
PC1 = (1/√2, 1/√2) ≈ (0.707, 0.707)
```

**Step 8 — interpret. Do not skip this.**

> Plotting `(2,2), (4,6), (6,4), (8,8)` shows the points running from bottom-left to
> top-right along the 45° diagonal, with the middle two sitting slightly off it. The
> direction of greatest spread is clearly `(1,1)` — **PC1 matches the data.**

**Step 9 — variance explained = each eigenvalue ÷ the total.**

```
total = 36 + 4 = 40

PC1:  36/40 = 0.90  →  90%
PC2:   4/40 = 0.10  →  10%
```

**Step 10 — scores: project each centered point onto PC1.**

> **score = (centered point) · (unit PC1)**

Since PC1 `= (1/√2)(1,1)`, the dot product is just `(x + y)/√2`:

```
(−3, −3)  →  (−3 + −3)/√2 = −6/√2 = −3√2 ≈ −4.243
(−1,  1)  →  (−1 +  1)/√2 =  0/√2 =   0
( 1, −1)  →  ( 1 + −1)/√2 =  0/√2 =   0
( 3,  3)  →  ( 3 +  3)/√2 =  6/√2 =  3√2 ≈  4.243
```

**Step 11 — interpret the scores.**

> Each 2-D point is now a single number giving its position along the direction of greatest
> spread, and that one number retains **90%** of the total variation.
>
> Notice points 2 and 3 both score **0**: PC1 cannot distinguish them at all, because they
> differ **only** along PC2 — the direction carrying the other 10%.

> **Answers:** mean `(5,5)`; `XcᵀXc = [[20,16],[16,20]]`; `λ = 36, 4`; eigenvectors `(1,1)`,
> `(1,−1)`; `PC1 = (1,1)/√2`; variance 90% / 10%; scores `−3√2, 0, 0, 3√2`.

---

### Worked Example 2 — the covariance-matrix route

> **Problem:** Using the same data, compute `Σcov = (1/n) XcᵀXc` and explain how its
> eigenvectors and eigenvalues relate to those found above.

**Step 1 — divide by n = 4.**

```
              1   ┌            ┐      ┌          ┐
Σcov   =     ───  │  20    16  │  =   │  5     4 │
              4   │  16    20  │      │  4     5 │
                  └            ┘      └          ┘
```

**Step 2 — its eigenvalues.** Using the `[[a,b],[b,a]]` shortcut: `5 + 4 = 9` and `5 − 4 = 1`.

```
λ = 9  and  λ = 1
```

**Step 3 — compare with Step 5 above.**

```
before (XcᵀXc):   36  and  4
now   (Σcov):      9  and  1

36/4 = 9        4/4 = 1        →  every eigenvalue divided by n = 4
```

**Step 4 — the eigenvectors.**

```
still (1, 1) and (1, −1)   —   UNCHANGED
```

**Step 5 — why.** If `Mv = λv`, then `(cM)v = c(Mv) = (cλ)v`.

> **Multiplying a matrix by a constant leaves its eigenvectors untouched and multiplies
> every eigenvalue by that constant.**

**Step 6 — and the proportions?**

```
9 / (9 + 1) = 90%          1 / (9 + 1) = 10%
```

> **Identical to before.** The constant cancels in the ratio.

> **Answer:** the principal component directions are **exactly the same**; the eigenvalues
> are each **divided by n**; the **variance-explained proportions are unchanged**. This is
> why the covariance route and the SVD route give the same PCA.
>
> *(Note: your lecture slides use `1/(n−1)` while Assignment Q9 writes `1/n`. The argument
> is identical either way and the proportions are unaffected — just state which you used.)*

---

### Worked Example 3 — why centering (and sometimes standardising) matters

> **Problem:** A dataset has `income` in dollars (values around 50,000) and `age` in years
> (values around 40). What goes wrong if you run PCA after centering only?

**Step 1 — what centering achieves.** It makes each column have mean 0, so variance measures
**spread around the mean** rather than distance from the origin. Without it, PC1 would point
at where the data sits, not at how it varies. **Centering is mandatory.**

**Step 2 — what centering does NOT fix.** Look at typical *spreads* after centering:

```
income deviations:   ±10,000      squared:  ~100,000,000
age deviations:      ±10          squared:  ~100
```

**Step 3 — see the consequence.** PCA maximises variance, and income's variance is about a
**million times larger** — purely because dollars are small units and there are many of them.

> PC1 will point almost exactly along the income axis, and will tell you nothing except
> "income has big numbers in it." Age is drowned out for reasons of **units**, not
> importance.

**Step 4 — the fix: standardise.**

> **Center, then divide each column by its standard deviation.** Every feature then has
> variance 1, and PCA compares their *patterns of variation* rather than their *unit sizes*.

**Step 5 — when each is appropriate.**

> - Features already in the **same units** on a comparable scale (e.g. the four Iris
>   measurements, all in cm) → centering alone is fine.
> - Features in **different units** (dollars vs years vs counts) → **standardise**.

---

### What to notice across all three

The PCA procedure never changes:

```
1. mean of each COLUMN
2. subtract it  →  Xc        (CHECK: columns sum to 0)
3. XcᵀXc = [[Σx², Σxy], [Σxy, Σy²]]
4. eigenvalues via λ² − tr·λ + det = 0     (shortcut: [[a,b],[b,a]] → a±b)
5. PC1 = eigenvector of the LARGEST eigenvalue, made unit length
6. variance explained = λᵢ / Σλ
7. scores = (centered point) · (unit PC)
8. SAY WHAT IT MEANS
```

Now do the drills.


## 9. Drills

**D1.** Full Assignment Q9 from scratch on paper, all six parts, without looking.

**D2.** `X` rows: `(2,0), (0,2), (4,4), (2,2)`. Find the mean, `Xc`, and `XcᵀXc`.

**D3.** Why must you center before PCA? Why is centering sometimes not enough?

**D4.** PCA eigenvalues are `(6, 3, 1)`. Variance explained by each? Smallest k for ≥90%?

**D5.** Your PCA plot is a mirror image of a classmate's, on the same data. Who's wrong?

---

### Answers

**D1.** Step by step:

*(i) Mean and centering.* Column means `(1+2+3+4)/4 = 2.5` and `(1+3+2+4)/4 = 2.5`, so the
mean is `(2.5, 2.5)`. Subtracting it row by row:
`Xc` rows = `(−1.5,−1.5), (−0.5, 0.5), (0.5,−0.5), (1.5, 1.5)`.
Check: each column sums to 0 ✓

*(ii) `XcᵀXc`.* `Σx² = 2.25+0.25+0.25+2.25 = 5`; `Σy² = 5` likewise;
`Σxy = 2.25−0.25−0.25+2.25 = 4`. So `XcᵀXc = [[5,4],[4,5]]`.
`tr = 10`, `det = 25−16 = 9` → `λ² − 10λ + 9 = 0` → `(λ−9)(λ−1) = 0` → **λ = 9, 1**.
λ=9: `[[−4,4],[4,−4]]` → `v₂ = v₁` → **(1,1)**.  λ=1: `[[4,4],[4,4]]` → `v₂ = −v₁` → **(1,−1)**.
(Orthogonal ✓, as required for a symmetric matrix.)

*(iii) PC1.* The eigenvector for the larger eigenvalue: **PC1 = (1,1)**, unit form
`(1/√2)(1,1)`. It matches the data — the four points run bottom-left to top-right along the
45° diagonal, which is visibly the direction of greatest spread.

*(iv) Variance explained.* Total `= 9+1 = 10` → PC1 **90%**, PC2 **10%**.

*(v) Covariance route.* `Σcov = (1/4)XcᵀXc`. Scaling by a constant leaves eigenvectors
unchanged and multiplies eigenvalues by that constant, so the **directions are identical**
and the eigenvalues become `9/4 = 2.25` and `1/4 = 0.25`. Proportions are **unchanged**
(`2.25/2.5 = 90%`) because the constant cancels in the ratio.

*(vi) Link to energy.* The eigenvalues of `XcᵀXc` play the role the squared singular values
`σᵢ²` played in Topic 7 — and they literally **are** `σ²`, since `XcᵀXc = VΣ²Vᵀ`.
"Explained variance" and "energy retained" are the same quantity.

*(Full narrative version with the reasoning spelled out: §3 above, and
`12-ASSIGNMENT-1-WORKED.md` Q9.)*

**D2.** Means: `(2+0+4+2)/4 = 2`, `(0+2+4+2)/4 = 2`. Mean `= (2,2)`.
`Xc` rows: `(0,−2), (−2,0), (2,2), (0,0)`. *(Columns sum to 0 ✓)*
`Σx² = 0+4+4+0 = 8`; `Σy² = 4+0+4+0 = 8`; `Σxy = 0+0+4+0 = 4`.
`XcᵀXc = [[8,4],[4,8]]`. *(Eigenvalues 12 and 4 → 75% / 25%, PC1 = (1,1)/√2.)*

**D3.** Variance means spread **around the mean**, so without centering the first component
points at where the data sits relative to the origin rather than at how it varies — you'd
be measuring position, not variation. Centering is not enough when features have very
different **scales**, because a feature measured in large units (income in dollars) has a
much larger raw variance than one in small units (age in years) purely because of units, so
it dominates PC1 for no meaningful reason. The fix is to **standardise**: center, then
divide each column by its standard deviation.

**D4.** Total `= 10`. **60%, 30%, 10%**. Cumulative: 60%, 90%, 100%. → **k = 2** (reaches
exactly 90%).

**D5.** **Neither.** Principal components are defined only up to a sign flip — a component
and its negation are the same direction — so a mirrored plot is an expected ambiguity of
the method, not an error. Only the grouping of points is meaningful, never which side of
zero they fall on.

---

## 10. The step I under-covered: PROJECTING the data (scores)

Finding PC1 tells you the **new axis**. It does **not** yet give you the 2D chart you
wanted. The missing step is projecting each data point onto that axis.

> **score of a point = (centered point) · (unit PC direction)**

That single number is the point's coordinate along PC1 — the thing you actually plot.
In matrix form, all scores at once: **`scores = Xc · V`**, equivalently **`UΣ`**.

This is just Topic 3's projection — the coefficient `c = (a·b)/(a·a)`, with `a·a = 1`
because PC directions are unit vectors, so it collapses to a plain dot product. **Nothing
new.** PCA = find the best directions (SVD) + project onto them (Topic 3).

### The lecture's worked example (Module 3 Session 4, "PCA by Hand")

Points: `(2,−2), (4,0), (1,7), (8,4), (10,6)`

**Mean:** `((2+4+1+8+10)/5, (−2+0+7+4+6)/5) = **(5, 3)**`

**Centered:** `(−3,−5), (−1,−3), (−4,4), (3,1), (5,3)`
*(Check: each column sums to 0 ✓)*

**`XcᵀXc`:**
`Σx² = 9+1+16+9+25 = 60`; `Σy² = 25+9+16+1+9 = 60`; `Σxy = 15+3−16+3+15 = 20`

    XcᵀXc = [ 60  20 ]      tr = 120, det = 3600 − 400 = 3200
            [ 20  60 ]      λ² − 120λ + 3200 = 0 → λ = 80, 40

**PC1 direction** (λ=80): `(1,1)` normalised = **`(1/√2, 1/√2) ≈ (0.707, 0.707)`** ✓

**Variance explained:** `80/120 = **66.7%** (PC1)`, `40/120 = **33.3%** (PC2)`

**Scores on PC1** — dot each centered point with `(0.707, 0.707)`:

| Centered point | Computation | Score |
|---|---|---|
| (−3,−5) | (−3−5)/√2 = −8/√2 | **−4√2 ≈ −5.66** |
| (−1,−3) | (−1−3)/√2 = −4/√2 | **−2√2 ≈ −2.83** |
| (−4, 4) | (−4+4)/√2 = 0 | **0** |
| ( 3, 1) | (3+1)/√2 = 4/√2 | **2√2 ≈ 2.83** |
| ( 5, 3) | (5+3)/√2 = 8/√2 | **4√2 ≈ 5.66** |

**Interpretation:** five 2-D points are now five single numbers, ordered along the direction
of greatest spread. That one number per point retains **66.7%** of the total variation.
The point at (1,7) scores **0** — it sits right on the perpendicular through the mean, so
it carries no information about position along PC1 at all.

## 11. ⚠ Covariance convention — lecture vs assignment

They genuinely differ, so read the question carefully and **state which you're using**:

- **The lecture** (Module 3 Session 4 backup) uses `C = (1/(n−1)) XcᵀXc`, with
  `C = QΛQᵀ` — Q's columns are the principal components and **`Λ = Σ²/(n−1)`**.
- **Assignment Q9 part 5** explicitly writes `Σcov = (1/n) XcᵀXc`.

`1/(n−1)` is the *sample* covariance (the standard statistical convention); `1/n` is the
*population* version. **It makes no difference to PCA**, and that's the point of the question:

> Scaling a matrix by any positive constant leaves its **eigenvectors completely unchanged**
> and multiplies every eigenvalue by that constant. So the **principal component directions
> are identical either way**, and because the constant cancels in the ratio
> `λᵢ / Σλⱼ`, the **variance-explained proportions are identical too.**

For the lecture example above with n=5: `C = (1/4)[[60,20],[20,60]] = [[15,5],[5,15]]`,
eigenvalues `20` and `10` — still **66.7% / 33.3%** ✓. Same directions, same proportions,
different raw numbers.

**Exam answer:** same eigenvectors; eigenvalues divided by `n` (or `n−1`); proportions
unchanged.
