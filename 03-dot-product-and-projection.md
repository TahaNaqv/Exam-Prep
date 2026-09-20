# Topic 3 — Dot Product, Orthogonality & Projection

The instructor said this is the one session he refused to compress, and orthogonality was
the **weakest topic on the whole diagnostic (45%)**. Expect it to be heavily examined.

The whole session is **one question asked at three scales**:
a number (dot product) → a line (project onto one direction) → a subspace (project onto many).

## 1. The dot product, two ways

**Algebraic:**  `u · w = Σ uᵢwᵢ`

`u = (3,1)`, `w = (1,2)` → `u·w = 3(1) + 1(2) = 5`

**Geometric:**  `u · w = ‖u‖ ‖w‖ cos θ`

These are the *same number*. The algebraic form is how you compute it; the geometric form
is what it means.

### Why this matters: orthogonality falls straight out

At θ = 90°, `cos θ = 0`, so the whole product is 0. Therefore:

> **u · w = 0  ⟺  u and w are orthogonal (perpendicular).**

This is the *definition* of orthogonality in this course — algebraic, no angle needed.

**What orthogonal means in words:** two directions that carry **zero overlapping
information**. It's the same word you already use in software ("orthogonal concerns",
"decoupled modules") and it's not a coincidence — it's the same idea, made precise.

### (Optional) Proof the two formulas agree — flagged as a possible bonus question

**What is being proved.** The two formulas look unrelated — one multiplies coordinates, the
other uses lengths and an angle. The proof shows they always produce the same number. This
is what licenses `u·w = 0 ⟺ perpendicular`; without it, that's an unjustified leap.

**The strategy:** pick one quantity, compute it **two different ways**, and set the answers
equal. The quantity chosen is `‖u − w‖²`, because `u`, `w` and `u − w` form a **triangle**
(which lets geometry in), and `‖·‖²` is easy to expand algebraically.

```
        w
        ●
       /│
      / │  ← u − w   (the side joining the two tips)
     /  │
    /θ  │
   ●────●
 origin  u
```

**Facts you need first:**
- `‖v‖² = v·v` — because `v·v = v₁² + v₂² + ⋯` and `‖v‖ = √(v₁² + v₂² + ⋯)`; squaring the
  norm removes the root.
- The dot product **distributes** over addition, just like ordinary multiplication.
- The dot product **commutes**: `u·w = w·u` (since `u₁w₁ + u₂w₂` reads the same either way).

---

**Line 1 — the geometric side: quote the law of cosines.**

For any triangle with sides `a`, `b` and included angle `θ`, the third side satisfies
`c² = a² + b² − 2ab·cos θ`. *(This is just Pythagoras plus a correction: at `θ = 90°`,
`cos θ = 0` and it collapses to `c² = a² + b²`.)*

With `a = ‖u‖`, `b = ‖w‖`, `c = ‖u−w‖`:

```
‖u − w‖² = ‖u‖² + ‖w‖² − 2‖u‖‖w‖cos θ                    ...(1)
```

**Line 2 — the algebraic side: expand the bracket.**

```
‖u − w‖² = (u − w)·(u − w)
         = u·u − u·w − w·u + w·w        [distribute, like FOIL]
         = ‖u‖² − 2(u·w) + ‖w‖²          [u·u = ‖u‖², and u·w = w·u]     ...(2)
```

**Line 3 — set (1) and (2) equal.** Both compute the same number, so:

```
‖u‖² + ‖w‖² − 2‖u‖‖w‖cos θ  =  ‖u‖² − 2(u·w) + ‖w‖²
```

**Line 4 — cancel `‖u‖²` and `‖w‖²` (they appear on both sides), then divide by −2.**

```
−2‖u‖‖w‖cos θ = −2(u·w)

         u·w = ‖u‖‖w‖cos θ          ∎
```

---

**Check it with numbers** (`u = (3,1)`, `w = (1,2)`, so `u − w = (2,−1)` and `‖u−w‖² = 5`):

```
Way 1:  ‖u‖² + ‖w‖² − 2‖u‖‖w‖cos θ  =  10 + 5 − 2(5)  =  5   ✓
Way 2:  ‖u‖² − 2(u·w) + ‖w‖²        =  10 − 10 + 5    =  5   ✓
```

Both routes give 5 — the proof, in numbers.

**Why memorise it:** you invent nothing. Quote the law of cosines, expand a bracket, cancel,
divide. Four lines for a possible bonus mark. And the payoff is the fact you use constantly:
`θ = 90° → cos θ = 0 → u·w = 0`, turning perpendicularity into pure arithmetic with no angle
ever computed.

## 2. Cosine similarity

Rearrange the geometric form:

> **cos θ = (u · w) / (‖u‖ ‖w‖)**

The name isn't decorative — this literally *is* the cosine of the angle. Range:

    −1  (opposite)  ←→  0  (orthogonal)  ←→  +1  (same direction)

### Worked example (the lecture's own data)

`A = (4,4,1,2)`, `D = (5,4,3,2)`

- `A·D = 20 + 16 + 3 + 4 = 43`
- `‖A‖ = √(16+16+1+4) = √37 ≈ 6.08`
- `‖D‖ = √(25+16+9+4) = √54 ≈ 7.35`
- `cos θ = 43 / (6.08 × 7.35) = 43 / 44.7 ≈ **0.96**`

Close to 1 → nearly the same direction → very similar taste profile.

### Why cosine and not the raw dot product

The raw dot product **still depends on magnitude**. A user who rates everything 5/5 gets a
big dot product with almost anybody, regardless of whether their taste actually matches.

Cosine similarity divides magnitude out deliberately, comparing **direction only**. That
is why production recommender systems and embedding search engines overwhelmingly use
cosine similarity specifically. Good exam sentence.

## 3. ⚠ THE TRAP: negative does not mean opposite

`u = (1,3)`, `w = (2,−1)` → `u·w = 1(2) + 3(−1) = **−1**`

A quarter of wrong diagnostic answers said this means the vectors are exactly opposite.
**Wrong.**

> `cos θ < 0` for the **entire range 90° < θ ≤ 180°.**

"Negative" narrows it to **"more than 90°, somewhere"** — nothing more. Exactly opposite
(180°) is one specific extreme case *inside* that range.

Here, `cos θ = −1/√50 ≈ −0.141` → `θ ≈ 98.1°`. Barely past perpendicular. Nowhere near
opposite. If it were exactly opposite, cos θ would be exactly −1.

**90° is a boundary, not a landmark to compare against.**

Also useful: **scaling doesn't change the angle.** Replace `u` by `10u` and the raw dot
product scales to `−10`, but cosine similarity is **unchanged** at `−0.141` — because the
extra factor of 10 appears in the numerator and in `‖10u‖ = 10‖u‖` and cancels.

## 4. Projection onto a line — derived, not memorised

### First: what are these objects?

**"The line through the origin in direction `a`" means every multiple of `a`.** With
`a = (3,1)`:

```
c = 0  → (0,0)      c = 1 → (3,1)      c = 2 → (6,2)      c = −1 → (−3,−1)
```

All of those sit on one straight line through the origin. So:

> **The line = the set of all points `c·a`, as `c` runs over every number.**

**This is the key reframing:** every point on the line is `c·a` for some `c`, so the
question "which point on the line?" is really **"which number `c`?"** An infinite search
collapses to finding one number.

**And `b` is a point off the line.** (Check for `b = (2,4)`: you'd need `3c = 2` → `c = 2/3`
*and* `1c = 4` → `c = 4`. Contradiction, so `b` is not on the line.)

```
              ● b
             ╱
            ╱
   ────────●────────────────→   the line (all multiples of a)
           0
```

**The residual** is the arrow *from* your chosen point *to* `b`:  `r = b − c·a`. It's what's
left over — the part of `b` the line couldn't account for — and **its length is your
distance to `b`**. So the job is: choose `c` to make `r` as short as possible.

### The real-world version

You're standing in a field beside a straight road. **Where do you walk to reach the road
fastest?** Straight at it, at a right angle — never diagonally. That instinct *is* this
theorem.

### Why perpendicular is the stopping condition

If `r` leans instead of standing at a right angle, then part of it points **along the
line** — and "along the line" is a direction you can actually move in. So slide that way
and you get closer: you weren't at the closest point.

The only time you **can't** improve is when `r` has no component along the line at all.

> **Perpendicular isn't a formula to memorise — it's the signal that no improvement is left.**

---

**The question:** given a line through the origin in direction `a`, and a point `b` off
the line, what's the closest point on the line to `b`?

The answer must be `c·a` for some scalar `c` (it's on the line). Which `c`?

**The key insight — the stopping condition is perpendicularity.** The leftover piece
(the *residual*) must be perpendicular to `a`, otherwise you could slide along the line
and get closer.

    (b − c·a) · a = 0
     b·a − c(a·a) = 0
                c = (a·b)/(a·a)

> **projₐ(b) = [ (a·b) / (a·a) ] · a**
>
> **residual r = b − projₐ(b)**,  and always  **r · a = 0**

Note `a·a = ‖a‖²`, so you'll also see it written `proj = ((a·b)/‖a‖²)·a`. Same thing.

### Worked example

`a = (3,1)`, `b = (2,4)`

1. `a·b = 3(2) + 1(4) = 10`
2. `a·a = 9 + 1 = 10`
3. `c = 10/10 = **1**`
4. `projₐ(b) = 1·(3,1) = **(3,1)**`
   > ⚠ Here `c = 1`, so the projection happens to equal `a` itself. **That is a coincidence
   > of these numbers, not a rule** — with `a = (2,1)`, `b = (3,4)` you get `c = 2` and a
   > projection of `(4,2)`, nowhere near `a`.
5. `r = b − proj = (2,4) − (3,1) = **(−1,3)**`
6. **Check:** `r·a = (−1)(3) + (3)(1) = −3 + 3 = **0** ✓`

> **Always do step 6.** The instructor called it "the habit that catches almost every
> mistake", and it's on his self-check list. It costs you five seconds and it verifies
> the entire calculation.

**Why the perpendicular is the closest point (the geometric explanation they'll ask for):**
`b`, its projection, and any other point on the line form a right triangle, with the
segment from `b` to any other point as the hypotenuse. The hypotenuse is always the
longest side, so every other point on the line is strictly farther from `b` than the
foot of the perpendicular. Equivalently: from any non-perpendicular point you could
slide toward the foot and shorten the distance, so it wasn't the closest.

## 5. Projection onto a subspace

Now project onto a whole plane instead of one line.

### The easy case, by inspection

`a₁ = (1,0,0)`, `a₂ = (0,1,0)` span the xy-plane. `b = (2,3,5)`.

Closest point is obvious: **(2,3,0)** — just drop the z-coordinate.
Residual = **(0,0,5)**.
Check: `r·a₁ = 0` ✓ and `r·a₂ = 0` ✓ — orthogonal to *both*.

### The non-obvious bit

The plane also contains infinitely many other directions (`a₁+a₂`, `3a₁−a₂`, …). Do we
have to check them all?

**No** — and here's the argument, which is the heart of the session:

Any direction in the plane is some `c₁a₁ + c₂a₂`. Then

    r · (c₁a₁ + c₂a₂) = c₁(r·a₁) + c₂(r·a₂) = c₁(0) + c₂(0) = 0

True **no matter what c₁, c₂ are.** So checking a small **spanning set** automatically
guarantees orthogonality to the *entire* subspace. That's why the formula is finite.

### The normal equations

Stack the directions `a₁, …, a_p` as **columns** of a matrix `A`. Saying
"`r·aⱼ = 0` for every column" is exactly `Aᵀr = 0`. With `r = b − Ax̂`:

    Aᵀ(b − Ax̂) = 0
         AᵀAx̂ = Aᵀb          ← the normal equations
            x̂ = (AᵀA)⁻¹Aᵀb

**Nothing new was proved.** It's the same one-line derivation, written for p directions
instead of one.

> **Universal principle:** the shortest distance from a point to a subspace is always
> along the perpendicular. Any other path could be shortened by sliding toward it.

## 6. Closing the loop with rank

`(AᵀA)⁻¹` only exists if `AᵀA` is invertible. When does a matrix fail to be invertible?
When it isn't full rank — Topic 2.

> **`AᵀA` is invertible ⟺ `A` has full column rank.**

So if `rank(A) < number of columns` (i.e. your features are collinear), `AᵀA` is singular,
and **there is no unique `x̂`**. The Topic 2 question and the Topic 3 formula are the same
question.

**What people do about it** (named on the slides, not derived — know the names):
- **PCA / SVD** — find the best-fitting subspace rather than projecting onto a pre-chosen one.
- **Gram–Schmidt / QR** — build a convenient *orthonormal* basis for a subspace.
- **Regularised regression** (Ridge) — modifies this exact formula so it works even when
  `AᵀA` is singular or near-singular, instead of demanding full rank.

## 7. Vocabulary

| Term | Meaning |
|------|---------|
| Dot product | `Σuᵢwᵢ` algebraically; `‖u‖‖w‖cos θ` geometrically |
| Orthogonal | dot product = 0 — perpendicular, no overlapping information |
| Span | the set of **all** linear combinations of a set of vectors |
| Subspace | a span, named as a place — everything those directions can reach |
| Projection | the point in a subspace **closest** to a given vector |
| Residual | `vector − projection` — always orthogonal to the subspace |
| Normal equations | `AᵀAx̂ = Aᵀb` |
| Orthonormal basis | a basis that is orthogonal **and** every vector has length 1 |
| Gram–Schmidt | builds an orthonormal basis from any set of vectors (named, not derived) |

## 7b. WORKED EXAMPLES — study these before the drills

---

### Worked Example 1 — dot product, cosine similarity, and reading the sign

> **Problem:** `u = (2, 4)`, `w = (3, −1)`. Compute `u·w`, the cosine similarity and the
> angle. What does the sign say? Are they exactly opposite? What happens if `u` becomes `5u`?

**Step 1 — the dot product: multiply matching entries, then add.**

```
u = (2, 4)
w = (3, −1)

first entries:   2 ×  3  =  6
second entries:  4 × (−1) = −4
                          ──────
u · w =                      2
```

> The result is a **single number**, not a vector. That's the whole point of a dot product.

**Step 2 — compute both lengths.**

```
‖u‖ = √(2² + 4²)     = √(4 + 16) = √20 ≈ 4.472
‖w‖ = √(3² + (−1)²)  = √(9 + 1)  = √10 ≈ 3.162
```

**Step 3 — cosine similarity = dot product ÷ (product of lengths).**

```
cos θ = (u·w) / (‖u‖·‖w‖)
      = 2 / (√20 · √10)
      = 2 / √200
      = 2 / 14.142
      ≈ 0.141
```

> Tip: `√20 · √10 = √200`. Multiplying under one root is easier than multiplying two
> decimals.

**Step 4 — the angle.**

```
θ = arccos(0.141) ≈ 81.9°
```

**Step 5 — sanity-check the sign before trusting the calculator.**

| `cos θ` | angle must be |
|---|---|
| > 0 | between 0° and 90° |
| = 0 | exactly 90° |
| < 0 | between 90° and 180° |

`cos θ ≈ +0.141` is positive → the angle must be **under 90°**. We got 81.9° ✓ Consistent.

**Step 6 — what the sign tells you.**

> The dot product is **positive**, so the angle is **less than 90°** (acute) — the two
> vectors broadly point the same way.

**Step 7 — are they exactly opposite?**

> **No.** Exactly opposite requires `cos θ = −1` (θ = 180°). Here `cos θ ≈ +0.141`, so they
> aren't even obtuse, let alone opposite.

**Step 8 — replace `u` with `5u = (10, 20)`.**

```
raw dot product:  (5u)·w = 5(u·w) = 5 × 2 = 10     ← scaled by 5

cosine:  ‖5u‖ = 5‖u‖, so
         cos θ = 5(u·w) / (5‖u‖·‖w‖)
                 the 5s cancel
               ≈ 0.141                              ← UNCHANGED
```

> **Answer:** raw dot product scales by 5 → 10; cosine similarity is unchanged at ≈0.141,
> because the factor appears in the numerator and in `‖5u‖ = 5‖u‖` and cancels. This is
> exactly why recommender systems use cosine rather than the raw dot product — otherwise a
> user who rates everything 5/5 would score highly against everyone.

---

### Worked Example 2 — the negative-dot-product trap

> **Problem:** `u = (1, 2)`, `w = (−4, 1)`. Compute `u·w`. Does this mean they point in
> exactly opposite directions?

**Step 1 — dot product.**

```
1 × (−4) = −4
2 ×   1  =  2
         ──────
u · w   =  −2
```

**Step 2 — cosine.**

```
‖u‖ = √(1 + 4)  = √5  ≈ 2.236
‖w‖ = √(16 + 1) = √17 ≈ 4.123

cos θ = −2 / (√5 · √17) = −2/√85 = −2/9.220 ≈ −0.217
```

**Step 3 — the angle.**

```
θ = arccos(−0.217) ≈ 102.5°
```

**Step 4 — answer the question carefully.**

> **No, they are not exactly opposite.** A negative dot product only tells you the angle is
> **greater than 90°** — an entire *range* from 90° to 180°. Here θ ≈ 102.5°, barely past
> perpendicular.
>
> "Exactly opposite" means θ = 180°, which requires `cos θ = −1` exactly. We have −0.217.
> **90° is a boundary, not a landmark to compare against.**

> ⚠ A quarter of wrong diagnostic answers picked "exactly opposite" here. This is on the
> instructor's self-check list.

---

### Worked Example 3 — projection onto a line, with the check

> **Problem:** `a = (2, 1)`, `b = (3, 4)`. Find the scalar coefficient, the projection, the
> residual, and verify orthogonality.

**Step 1 — know the goal.** You want the point on the line through `a` that sits closest to
`b`. That point must look like `c·a` for some number `c` — so the job is to find `c`.

**Step 2 — know where the formula comes from.** The residual must be **perpendicular** to
`a`, otherwise you could slide along the line and get closer:

```
(b − c·a) · a = 0
 b·a − c(a·a) = 0
            c = (a·b)/(a·a)
```

**Step 3 — compute the two dot products.**

```
a · b = (2)(3) + (1)(4) = 6 + 4 = 10
a · a = (2)(2) + (1)(1) = 4 + 1 =  5
```

> `a·a` is always the vector dotted with **itself** — never with `b`. Getting these two
> confused is the most common error here.

**Step 4 — the coefficient.**

```
c = 10 / 5 = 2
```

**Step 5 — the projection: multiply `a` by `c`.**

```
projₐ(b) = 2 · (2, 1) = (4, 2)
```

**Step 6 — the residual: subtract the projection from `b`.**

```
r = b − projₐ(b) = (3, 4) − (4, 2) = (−1, 2)
```

> Order matters: it's `b` minus the projection, not the other way round.

**Step 7 — VERIFY. Never skip this.**

```
r · a = (−1)(2) + (2)(1) = −2 + 2 = 0   ✓
```

Zero means the residual really is perpendicular to `a`, which confirms the whole
calculation in five seconds.

> **Answer:** `c = 2`, `projₐ(b) = (4, 2)`, `r = (−1, 2)`, and `r·a = 0` ✓

**Step 8 — the geometric explanation (worth marks).**

> Take any other point `p` on the line. Then `b`, the projection, and `p` form a **right
> triangle**, with the right angle at the projection and the segment from `b` to `p` as the
> **hypotenuse**. The hypotenuse is always the longest side, so `p` is strictly farther from
> `b` than the projection is. Since `p` was arbitrary, the projection is the closest point.

---

### Worked Example 4 — testing orthogonality

> **Problem:** Are `u = (3, 6)` and `w = (4, −2)` orthogonal?

**Step 1 — know the test.** Two vectors are orthogonal exactly when their dot product is
**zero**. No angles, no lengths needed.

**Step 2 — compute.**

```
3 ×   4  =  12
6 × (−2) = −12
          ─────
             0
```

**Step 3 — conclude.**

> `u·w = 0`, so **yes, `u` and `w` are orthogonal** — perpendicular, carrying no overlapping
> information.

*(Counter-check: `(3,6)` and `(4,1)` give `12 + 6 = 18 ≠ 0` → not orthogonal.)*

---

### What to notice across all four

- Dot product → a **number**. It answers "how much do these two agree?"
- **Zero** → perpendicular. **Positive** → under 90°. **Negative** → over 90° (a *range*).
- Cosine divides out length, so it compares **direction only**.
- Projection is three steps — `c`, then `c·a`, then `b −` that — **and always verify `r·a = 0`.**

Now do the drills.


## 8. Drills

**D1.** `u = (1,3)`, `w = (2,−1)`. (a) `u·w`. (b) cosine similarity. (c) What does the
sign say about the angle? (d) Are they exactly opposite? Explain. (e) Replace `u` with
`10u` — what changes in the dot product, what doesn't in cosine similarity?

**D2.** `a = (3,1)`, `b = (2,4)`. Derive `c`, find `projₐ(b)`, find `r`, verify `r·a = 0`,
and explain geometrically why the projection is the closest point.

**D3.** `a = (1,2)`, `b = (4,3)`. Same five steps.

**D4.** Are `(2,−3)` and `(3,2)` orthogonal? Show it.

**D5.** State the single condition that characterises the closest point in a subspace,
and explain why checking a spanning set is enough.

---

### Answers

**D1.**
(a) `1(2) + 3(−1) = **−1**`
(b) `‖u‖=√10`, `‖w‖=√5`, so `cos θ = −1/√50 = −1/(5√2) ≈ **−0.141**`
(c) Negative ⇒ the angle is **greater than 90°** (obtuse). That's all it says.
(d) **No.** Exactly opposite means `cos θ = −1`; here `cos θ ≈ −0.141`, so `θ ≈ 98.1°` —
only just past perpendicular. Negative is a whole *region* (90°–180°), not the single
point 180°.
(e) Raw dot product scales by 10 → **−10**. Cosine similarity is **unchanged** (−0.141),
because the factor 10 appears in the numerator and in `‖10u‖ = 10‖u‖`, and cancels.

**D2.** `a·b = 10`, `a·a = 10`, `c = 1`, `proj = (3,1)`, `r = (−1,3)`, `r·a = −3+3 = 0` ✓.
Geometric reason: `b`, the projection, and any other point on the line form a right
triangle with the segment from `b` to that other point as the hypotenuse; the hypotenuse
is always the longest side, so every other point on the line is strictly farther from `b`.

**D3.** `a·b = 4 + 6 = 10`. `a·a = 1 + 4 = 5`. `c = 10/5 = **2**`.
`proj = 2(1,2) = **(2,4)**`. `r = (4,3) − (2,4) = **(2,−1)**`.
Check: `r·a = 2(1) + (−1)(2) = 0` ✓

**D4.** `(2)(3) + (−3)(2) = 6 − 6 = **0**` → **yes, orthogonal.**

**D5.** The closest point is characterised by exactly one condition: **the residual is
orthogonal to every vector in the subspace.** Checking a spanning set suffices because
any subspace vector is a linear combination `c₁a₁+⋯+c_pa_p`, and
`r·(Σcⱼaⱼ) = Σcⱼ(r·aⱼ) = 0` whenever each `r·aⱼ = 0` — regardless of the `cⱼ`.
