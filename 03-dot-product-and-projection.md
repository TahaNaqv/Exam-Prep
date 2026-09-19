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

Law of cosines:      `‖u−w‖² = ‖u‖² + ‖w‖² − 2‖u‖‖w‖cos θ`
Algebraic expansion: `‖u−w‖² = (u−w)·(u−w) = ‖u‖² − 2(u·w) + ‖w‖²`

Set them equal, cancel `‖u‖² + ‖w‖²`, divide by `−2`:  **`u·w = ‖u‖‖w‖cos θ`** ∎

Four lines. Worth memorising — it's cheap insurance for a bonus mark.

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
