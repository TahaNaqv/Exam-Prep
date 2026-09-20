# Topic 1 — Vectors & Norms

## 1. What a vector actually is

A vector is a list of numbers. That's it. `v = (5, 2)`.

The lecture made a point of saying it's **three things at once**, and the exam may ask
you to talk about this, so hold all three:

- **A list** — `(5, 2)`. What the computer stores.
- **A point** — the location (5, 2) on a plane. What a data point *is*.
- **An arrow** — from the origin out to that point. This is the view you need whenever
  you talk about *length*, *distance*, or *angle*.

None is more correct. You pick the view that matches the question.

**Why data scientists care:** one row of your dataset = one vector. A customer with
4 measurements is a point in ℝ⁴. "Similar customers" becomes "nearby points", which is
a thing you can actually compute. That's the whole idea behind the movie-recommendation
hook in the lecture.

**Vocabulary you're expected to use:**
- *feature* — one measured quantity (e.g. "age")
- *feature vector* — one sample's features stacked into a list
- *weight vector* — a model's learned parameters, as a list
- *embedding* — a vector a model produces to represent something (a word, a user)

## 2. Operations

- **Addition**: add componentwise. `(3,1) + (1,2) = (4,3)`. Picture: tip-to-tail.
- **Scalar multiplication**: `2u` stretches it to double length, `−u` flips it around.
- **Linear combination**: `c₁u + c₂w` — a weighted sum of vectors.

**Linear combination is the single most important idea in this entire module.** Matrices,
rank, span, basis, projection, SVD, PCA — every one of them is just a question about
linear combinations. If you understand nothing else today, understand this phrase.

## 3. Basis and dimension

- `e₁ = (1,0)`, `e₂ = (0,1)` are the standard **basis** for ℝ².
- Any vector is built from them: `(5,2) = 5e₁ + 2e₂`.

**Basis** = a minimal set of vectors that can build *every* vector in the space via
linear combinations. Minimal matters: no spare, redundant vectors allowed.

**Dimension** = how many vectors that minimal set needs.

Park this. Session 2's "rank" is literally this definition applied to a matrix's columns.

## 4. Norms — measuring size

A **norm** `‖v‖` measures the size of **one** vector.

**Distance is not a separate idea.** Distance between two vectors is the norm of their
difference:

> **dist(u, w) = ‖u − w‖**

Why: `u − w` is the arrow that connects the two points. Its length is the gap between them.
Nothing new is being defined.

### What qualifies as a norm

Any formula satisfying all three of these earns the name:

1. **Non-negativity** — `‖v‖ ≥ 0`, and `= 0` only for the zero vector.
2. **Homogeneity** — `‖cv‖ = |c|·‖v‖`. Double the vector, double the size.
3. **Triangle inequality** — `‖u + w‖ ≤ ‖u‖ + ‖w‖`. A detour is never shorter than
   going direct.

This is *why* three completely different formulas can all legitimately be called norms.
That's the point of the definition — it isn't trivia.

### The three norms you must know

For `v = (v₁, …, vₙ)`:

| Norm | Formula | Name | Intuition |
|------|---------|------|-----------|
| `‖v‖₁` | `Σ\|vᵢ\|` | L1 / Manhattan / taxicab | walk along a city grid |
| `‖v‖₂` | `√(Σ vᵢ²)` | L2 / Euclidean | straight-line, ruler distance |
| `‖v‖∞` | `max\|vᵢ\|` | L∞ / max / Chebyshev | the single worst component |

### Worked example

`diff = (4, −3)`

- L1 = |4| + |−3| = **7**
- L2 = √(4² + 3²) = √25 = **5**
- L∞ = max(4, 3) = **4**

One vector, three honest answers. L1 ≥ L2 ≥ L∞ always — a useful sanity check.

## 5. Why L1 and L2 disagree about "nearest"

This is the most conceptual thing in Topic 1 and the lecture spent two slides on it,
which means he cares.

**The mechanism:** L2 squares the differences. Squaring punishes one big error far more
than several small errors that add to the same total.

Concrete: both `(1, 0)` and `(0.5, 0.5)` cost exactly **1** under L1.
- `‖(1,0)‖₂ = 1`
- `‖(0.5,0.5)‖₂ = √0.5 ≈ 0.71`

Same L1 budget, different L2 length. **Concentrating in one coordinate costs more under
L2; spreading evenly costs less.**

So if point P differs from you by (3, 0) and point R differs by (2, 2):
- L1 says P is nearer (3 < 4)
- L2 says R is nearer (3 > 2.83)

They genuinely disagree, and neither is wrong. **"Nearest" is not a property of the data —
it's a property of the data *plus* the ruler you chose.** That sentence is the answer to
the "explain in one or two sentences" part of the exam question.

## 6. Where this shows up in ML (a likely one-liner on the exam)

Add a penalty on the size of a model's weight vector `w` to the loss, to stop weights
growing wild:

- **Ridge regression** adds `‖w‖₂²` → shrinks all weights smoothly toward zero, never
  exactly to zero.
- **Lasso** adds `‖w‖₁` → drives some weights **exactly** to zero, so it also selects features.

**Why Lasso hits exact zero and Ridge can't** — the geometric argument from the slides:
draw the set of vectors with norm 1 (the "unit ball").
- L1's unit ball is a **diamond**, with sharp **corners sitting exactly on the axes**.
  A corner on an axis means some coordinate is exactly 0.
- L2's unit ball is a **circle** — perfectly smooth, no corners anywhere.

The fitted solution is where the loss contours first touch that ball as the penalty grows.
Corners are disproportionately likely first-contact points (many different contour
orientations all hit the same corner first). The circle has no corners to land on, so
Ridge just slides smoothly and never lands exactly on an axis.

## 6b. WORKED EXAMPLES — study these before the drills

Each example below is the **same type** as a drill, with different numbers. Read the
example, then close the file and redo it from scratch. Only then attempt the drill.

---

### Worked Example 1 — computing all three norms

> **Problem:** Let `v = (2, −6, 3)`. Compute `‖v‖₁`, `‖v‖₂` and `‖v‖∞`.

**Step 1 — write down what each norm asks for.**

| Norm | What to do |
|---|---|
| L1 | add the absolute values |
| L2 | square each, add, take the square root |
| L∞ | take the largest absolute value |

**Step 2 — take absolute values first.** Do this once and reuse it; it prevents sign errors.

```
|2| = 2      |−6| = 6      |3| = 3
```

> ⚠ The minus sign **disappears**. `|−6| = 6`, not `−6`.

**Step 3 — L1: add them.**

```
‖v‖₁ = 2 + 6 + 3 = 11
```

**Step 4 — L2: square, add, square-root.** Square the *original* entries (squaring kills
the sign anyway, so `(−6)² = 36`, not `−36`).

```
2² = 4
(−6)² = 36
3² = 9
          ────────
sum     =   49

‖v‖₂ = √49 = 7
```

**Step 5 — L∞: the largest absolute value.**

```
‖v‖∞ = max(2, 6, 3) = 6
```

**Step 6 — sanity check.** It must always be true that `L1 ≥ L2 ≥ L∞`:

```
11 ≥ 7 ≥ 6   ✓
```

> **Answer:** `‖v‖₁ = 11`, `‖v‖₂ = 7`, `‖v‖∞ = 6`.

**If the check fails, you made an arithmetic error — go back.** This costs five seconds and
catches most mistakes.

---

### Worked Example 2 — which candidate is nearest (L1 vs L2)

> **Problem:** Relative to the query point `q = (0, 0)`, which of `p = (4, 0)` and
> `r = (3, 3)` is closer **to q** under L1? Under L2?

**Step 1 — know what "distance" means here.** Distance is always the **norm of a
difference**:

```
dist(p, q) = ‖p − q‖
```

**Step 2 — compute the difference vectors.**

```
p − q = (4, 0) − (0, 0) = (4, 0)
r − q = (3, 3) − (0, 0) = (3, 3)
```

> Because `q` is the origin, subtracting it changes nothing — the difference *is* the point.
> **This shortcut only works when the query sits at (0,0).** If `q` were `(1,1)` you would
> have to subtract properly.

**Step 3 — L1 distance for each candidate.**

```
dist(p,q) = |4| + |0| = 4
dist(r,q) = |3| + |3| = 6
```

`4 < 6` → **under L1, p is closer to q.**

**Step 4 — L2 distance for each candidate.**

```
dist(p,q) = √(4² + 0²) = √16     = 4
dist(r,q) = √(3² + 3²) = √18 ≈ 4.243
```

`4 < 4.243` → **under L2, p is also closer to q.**

**Step 5 — interpret.** Here the two norms **agree**. That is normal — they don't *always*
disagree. Disagreement needs the concentrated candidate to have a *larger* L1 total.

**Step 6 — see a case where they DO disagree.** Change `p` to `(5, 0)`:

```
L1:  dist(p,q) = 5      dist(r,q) = 6      → p closer  (5 < 6)
L2:  dist(p,q) = 5      dist(r,q) ≈ 4.243  → r closer  (4.243 < 5)
```

**Now they disagree.** `p` dumped all 5 units of difference into one coordinate; `r` spread
6 units across two. L2 squares, so it punishes `p`'s concentration; L1 just totals and
prefers `p`'s smaller sum.

> **The sentence to write:** L2 squares each coordinate difference, so one large deviation
> costs more than several small ones adding to the same total — which means "nearest"
> depends on the norm you chose, not on the data alone.

---

### Worked Example 3 — distance between two arbitrary vectors

> **Problem:** `u = (7, 1)`, `w = (2, 4)`. Find the L1 and L2 distance between them.

**Step 1 — subtract to get the difference vector.** This is the step people skip, and
skipping it is the single most common error in this topic.

```
u − w = (7 − 2, 1 − 4) = (5, −3)
```

**Step 2 — now take norms of that difference.** Do **not** take norms of `u` and `w`
separately and subtract those — that is a different (and wrong) quantity.

```
L1:  ‖(5, −3)‖₁ = |5| + |−3| = 5 + 3 = 8
L2:  ‖(5, −3)‖₂ = √(5² + (−3)²) = √(25 + 9) = √34 ≈ 5.83
```

**Step 3 — check.** `8 ≥ 5.83` ✓ (and L∞ would be 5, so `8 ≥ 5.83 ≥ 5` ✓)

> **Answer:** L1 distance `= 8`, L2 distance `= √34 ≈ 5.83`.

**Step 4 — note the direction doesn't matter.** `w − u = (−5, 3)`, and both norms would give
the same answers, because norms discard sign. Distance from `u` to `w` equals distance from
`w` to `u`, as it must.

---

### What to notice across all three

Every problem in this topic is the same two moves:

1. **If two things are involved, subtract first** to get one vector.
2. **Apply the norm formula** to that single vector.

Now do the drills.


## 7. Drills — do these on paper

**D1.** `x = (3, −4, 1)`. Compute ‖x‖₁, ‖x‖₂, ‖x‖∞.

**D2.** Relative to the **query point** `q = (0,0)`, compare the candidates `p = (3,0)`
and `r = (2,2)`. Which candidate is closer **to q** under L1 distance? Which is closer
**to q** under L2? Explain the disagreement in one sentence.

**D3.** `u = (1, 2)`, `w = (4, 6)`. Compute the L1 and L2 distance between them.

**D4.** State the three properties a norm must satisfy, from memory.

**D5.** In one sentence each: why does Lasso zero out coefficients, and why doesn't Ridge?

---

### Answers

**D1.** L1 = 3+4+1 = **8**.  L2 = √(9+16+1) = √26 ≈ **5.10**.  L∞ = **4**.
(Sanity check: 8 ≥ 5.10 ≥ 4 ✓)

**D2.** Distance is the norm of a difference: `dist(p,q) = ‖p − q‖`. Here `q = (0,0)`, so
`p − q = p` and each distance collapses to the norm of the candidate itself — a shortcut
that works **only because the query sits at the origin**.
L1: `dist(p,q) = 3`, `dist(r,q) = 4` → **p is closer to q**.
L2: `dist(p,q) = 3`, `dist(r,q) = √8 ≈ 2.83` → **r is closer to q**.
*They disagree because L2 squares each difference, so p's single large deviation of 3
is penalised more heavily than r's two moderate deviations of 2, even though r's
deviations sum to more.*

**D3.** `u − w = (−3, −4)`. L1 = 3 + 4 = **7**. L2 = √(9+16) = **5**.

**D4.** Non-negativity (zero only for the zero vector); homogeneity `‖cv‖ = |c|‖v‖`;
triangle inequality `‖u+w‖ ≤ ‖u‖+‖w‖`.

**D5.** Lasso's L1 ball is a diamond whose corners lie on the coordinate axes, and the
loss contour is disproportionately likely to first touch a corner — a corner has a
coordinate exactly zero. Ridge's L2 ball is a smooth circle with no corners, so contact
happens at a generic point and every coefficient stays non-zero (just smaller).
