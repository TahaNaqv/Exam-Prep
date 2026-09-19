# Topic 7 — Truncated SVD, Energy & Eckart–Young

## 1. The idea

Singular values are **sorted largest to smallest** and are **always ≥ 0**. In real data,
most of them turn out to be tiny.

So: **what if we just drop the small ones?**

> **Aₖ = Uₖ Σₖ Vₖᵀ**
>
> Keep only the first **k** columns of U, the top **k** singular values, and the first
> **k** rows of Vᵀ. Throw the rest away.

For `A` of size m × n: `(m×k)(k×k)(k×n) → m×n`. **Same shape as A** — still a full-size
matrix, just built from less information. Always do this shape check.

Equivalently, as a sum of rank-1 pieces:

    A = σ₁u₁v₁ᵀ + σ₂u₂v₂ᵀ + ⋯      and      Aₖ = the first k terms

Each term is a rank-1 layer, and they're ordered by importance. Truncation = keep the
first k layers.

## 2. Eckart–Young — the theorem that makes this respectable

Obvious question: among **all** rank-k matrices — not just truncated SVDs — which one gets
closest to A?

> **Eckart–Young theorem:** the truncated SVD `Aₖ` is the **best possible rank-k
> approximation of A**, in both the **Frobenius norm** and the **spectral norm**, among
> **all** rank-k matrices. The leftover error is determined exactly by the singular values
> you discarded.

That last clause matters: it isn't merely "good", it's **provably optimal**, and the error
is known in advance. When the exam says "state precisely what Eckart–Young guarantees",
give all three parts: *(i)* best among **all** rank-k matrices, not just SVD-derived ones,
*(ii)* in both Frobenius and spectral norm, *(iii)* with error exactly given by the dropped
singular values.

(The proof is short and elegant but the slides say it's **never required** — possible
optional bonus only.)

## 3. Energy — the two formulas you must not mix up

> **Total energy = Σᵢ σᵢ²** — the sum of the **SQUARED** singular values.

"Squared" is item #6 on the instructor's self-check list. Do not forget the square.

> **Energy retained by a rank-k approximation:**
>
>     (σ₁² + ⋯ + σₖ²) / (σ₁² + ⋯ + σᵣ²)
>
> **Frobenius reconstruction error:**
>
>     ‖A − Aₖ‖_F = √(σ²ₖ₊₁ + σ²ₖ₊₂ + ⋯ + σ²ᵣ)   ← the DROPPED ones, and take the SQUARE ROOT

Two different things, easy to confuse:
- **Energy retained** is a **ratio**, no square root, uses the **kept** values.
- **Frobenius error** is a **length**, **has a square root**, uses the **dropped** values.

### Worked example

Singular values `σ₁ = 4, σ₂ = 3, σ₃ = 1`.

**Total energy:** `4² + 3² + 1² = 16 + 9 + 1 = **26**`

**Rank-1:** retained `= 16/26 = **0.6154 → 61.54%**`
Frobenius error `= √(3² + 1²) = √10 ≈ **3.162**`

**Rank-2:** retained `= (16+9)/26 = 25/26 = **0.9615 → 96.15%**`
Frobenius error `= √(1²) = **1**`

**"Retain at least 95% of the energy — which k?"**
k=1 gives 61.54% ✗. k=2 gives 96.15% ✓. → **k = 2**.

Note how fast it saturates: two of three components already carry 96% of the signal. That
is the whole phenomenon this module is about.

*(Sanity check: retained-energy fraction + (error² / total energy) = 1. Here
25/26 + 1/26 = 1 ✓.)*

## 4. The payoff: image compression

The Day 1 raccoon photo, 768 × 1024, is just a matrix of pixel values. Truncate its SVD:

- **k = 5** already captures **93.8%** of the energy
- **k = 50** captures **~98.4%** and looks essentially like the original

**Why it works so well:** real photographs are **very low rank**. Neighbouring pixels are
strongly correlated, so most of the 768 available components carry almost nothing. The raw
size wildly overstates the true complexity. That's not a trick specific to images — it's a
general fact about real data, and it's the thesis of the whole module.

And Eckart–Young guarantees each of those reconstructions is **the best possible** at that
rank.

**Counting the storage:**
- Original: `m × n` values
- Rank-k: `k(m + n + 1)` values — `mk` for Uₖ, `k` for Σₖ, `kn` for Vₖᵀ

For 768×1024 at k=50: `786,432` → `50 × (768 + 1024 + 1) = 89,650`. About **8.8× fewer
values**.

⚠ The slides are careful here: that's a comparison **in number of values, not bytes**. If
the original is `uint8` and the SVD factors are `float32`, the real byte-level saving is
smaller. Mention this if asked — it's the kind of honesty being marked.

## 5. How to actually choose k

Four legitimate criteria — know all four, they're a likely short question:

1. **Cumulative energy** — pick the smallest k where cumulative energy crosses a target,
   typically **90% or 95%**.
2. **Elbow method** — plot singular values and find where the curve visibly flattens;
   beyond the elbow, extra components buy almost nothing.
3. **Domain constraints** — a storage budget, latency limit, or required compression ratio
   can simply dictate k. ("I need to plot this, so k = 2, full stop.")
4. **Error vs compression tradeoff** — smaller k = more compression *and* more
   reconstruction error. **There is no free lunch**; pick the tradeoff that fits the task.

## 6. Same math, different data

SVD does not know or care whether the columns are pixels or word counts.

Apply it to a **document-term matrix** and the left singular vectors group **documents** by
topic while the right singular vectors group **words** by topic — with **no topic labels
ever supplied**. (Details in Topic 6 §5.)

## 7. Where it shows up

- **Denoising** — keep high-singular-value structure, drop the rest. Low-rank structure is
  signal; the discarded components are noise or redundancy.
- **Medical imaging** — MRI/CT reconstruction and compression via low-rank structure.
- **Genomics** — gene-expression matrices (genes × samples) summarised by dominant components.
- **Recommenders / LSA / LoRA** — as in Topic 6.

## 8. Honest limitations (the lecture gave a whole slide — likely a short question)

1. **Assumes linear structure.** SVD only finds linear combinations. Genuinely nonlinear
   structure (e.g. data on a spiral) won't compress well for any k.
2. **Cost at scale.** A full SVD is expensive for very large matrices; real systems use
   randomized or truncated algorithms, not the textbook computation.
3. **Components can be hard to interpret.** The toy topic demo was readable *by design*.
   Real singular vectors are often dense, signed combinations with no clean one-line story.
4. **Missing data needs care.** Ordinary SVD assumes a complete matrix. Missing entries —
   very common in real recommender data — need specialised methods, not naive SVD.

## 9. Drills

**D1.** `σ₁ = 4, σ₂ = 3, σ₃ = 1`.
(a) Total energy. (b) Energy retained at rank-1 and rank-2. (c) Frobenius errors for both.
(d) For ≥95% energy, which k? (e) State precisely what Eckart–Young guarantees.

**D2.** `σ = (10, 6, 3, 1)`. Total energy, energy retained at k=2, Frobenius error at k=2.
Smallest k for ≥95%?

**D3.** `A` is 100 × 200, truncated at k = 10. Give the dimensions of `U₁₀`, `Σ₁₀`,
`V₁₀ᵀ`, and the number of stored values vs the original.

**D4.** List the four ways to choose k.

**D5.** Why do real photographs compress so well under truncated SVD?

---

### Answers

**D1.**
(a) `16 + 9 + 1 = **26**`
(b) rank-1: `16/26 ≈ **61.54%**`; rank-2: `25/26 ≈ **96.15%**`
(c) rank-1: `√(9+1) = √10 ≈ **3.162**`; rank-2: `√1 = **1**`
(d) k=1 → 61.54% (fails), k=2 → 96.15% (passes). **k = 2.**
(e) Among **all** matrices of rank k — not only truncated SVDs — the truncated SVD `Aₖ`
minimises the approximation error to A, in **both** the Frobenius and spectral norms, and
the remaining error is determined exactly by the singular values that were discarded.

**D2.** Energy `= 100 + 36 + 9 + 1 = **146**`.
k=2 retained `= 136/146 ≈ **93.15%**`. Frobenius error at k=2 `= √(9+1) = √10 ≈ **3.162**`.
For ≥95%: k=3 gives `145/146 ≈ 99.3%` ✓, and k=2 fails at 93.15%. → **k = 3.**

**D3.** `U₁₀`: **100 × 10**. `Σ₁₀`: **10 × 10**. `V₁₀ᵀ`: **10 × 200**.
Check: `(100×10)(10×10)(10×200) → 100×200` ✓.
Stored: `10 × (100 + 200 + 1) = **3,010**` values vs `100 × 200 = **20,000**` originally —
roughly a **6.6×** reduction in number of values.

**D4.** Cumulative energy crossing a target (90%/95%); the elbow of the singular-value
plot; domain constraints (storage, latency, "I need 2D to plot"); and the explicit
error-vs-compression tradeoff.

**D5.** Because real photographs are effectively **very low rank** — neighbouring pixels
are highly correlated, so the image matrix's columns are close to linear combinations of a
small number of underlying patterns. Almost all the energy concentrates in the first few
singular values (k=5 already gives ~94%), so the vast majority of components can be dropped
with barely any visible loss.

---

## 10. Truncation made concrete (the lecture's backup worked example)

The clearest possible illustration, because for a **diagonal** matrix the singular values
*are* the diagonal entries, so truncation is literally "delete the smaller entries".

    A = [ 4  0  0 ]        σ = (4, 3, 1)
        [ 0  3  0 ]
        [ 0  0  1 ]

**Rank-1 truncation** — keep only σ₁:

    A₁ = [ 4  0  0 ]        ‖A − A₁‖_F = √(3² + 1²) = √10 ≈ 3.16
         [ 0  0  0 ]
         [ 0  0  0 ]

**Rank-2 truncation** — keep σ₁ and σ₂:

    A₂ = [ 4  0  0 ]        ‖A − A₂‖_F = √(1²) = 1.00
         [ 0  3  0 ]
         [ 0  0  0 ]

Notice the error is built **only from the singular values you threw away** — nothing else
enters the formula. That's the content of the Eckart–Young error clause, visible directly.

*(These are the same σ = (4,3,1) as Assignment Q8 and as drill D1 above.)*

## 11. The compression arithmetic, exactly as the lecture states it

    Full image:    768 × 1024                = 786,432 values
    Rank-50 SVD:   50 × (768 + 1024 + 1)     =  89,650 values
    Ratio:         786,432 / 89,650          ≈ 8.77×
    Storage saved: ≈ 88.6%   (you store ~11.4% of the original)

The `+1` per component is the singular value itself; the `768` and `1024` are that
component's column of U and row of Vᵀ.

⚠ As the slide says: this is a comparison **in number of values, not bytes**. If the
original is stored as `uint8` (1 byte each) and the SVD factors as `float32` (4 bytes each),
the real byte-level saving is smaller. Say so if you quote the figure — that caveat is
exactly the kind of interpretive care being marked.
