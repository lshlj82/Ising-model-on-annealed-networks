# Ising model on annealed scale-free networks: an interactive demo

This is an in-browser companion to

> S. H. Lee, M. Ha, H. Jeong, J. D. Noh, and H. Park, "Critical behavior of the Ising model in annealed scale-free networks," *Phys. Rev. E* **80**, 051127 (2009). [doi:10.1103/PhysRevE.80.051127](https://doi.org/10.1103/PhysRevE.80.051127)

*Created by Claude Opus 5.5 (Anthropic).*

## The model

Here an **annealed network** is a *globally coupled* network. Every pair of nodes interacts, with a weight proportional to the product of their nominal degrees:

```
H = −K̃ Σ_{i<j} (k_i k_j / N z₁) s_i s_j  =  −(K̃ / 2N z₁) (Σ_i k_i s_i)² + const      (Eqs. 8–9)
```

The k_i are **weights** drawn from a scale-free distribution P(k) ∝ k^−λ with k₀ ≤ k ≤ k_c = int(d N^(1/ω)). They are not counts of realized links, and no graph is ever drawn. The energy depends only on M̃ = Σ k_i s_i, so the finite-size problem reduces to a one-dimensional saddle-point integral over the order parameter m̃ = M̃ / N z₁.

The paper motivates this Hamiltonian as the average of the Ising model over random graphs with link probability p_ij = k_i k_j / N z₁. That derivation needs p_ij < 1, which restricts it to λ > 3 (its note 14). The weighted, globally coupled model has no such restriction, so this demo also covers 2 < λ ≤ 3.

## What it reproduces and adds

| Regime | Behaviour | Source |
|--------|-----------|--------|
| λ > 5 | Standard mean-field FSS, β = 1/2 and ν̄ = 2, plus a critical-point shift K̃c(N) − K̃c^∞ ∝ N^−ζ with ζ = (λ − 3)/ω | Paper |
| 3 < λ < 5, ω > λ − 1 | Two-parameter scaling with εN^(1/ν̄c) and εN^(1/ν̄*), and four regimes | Paper |
| 3 < λ < 5, ω = λ − 1 | The two scaling variables merge into one, with ν̄ = (λ − 1)/(λ − 3) | Paper |
| **2 < λ ≤ 3** | **No transition as N → ∞.** K̃c(N) = z₁/z₂ ∝ N^−(3−λ)/ω → 0, and the system is ordered at every K̃ > 0 | Derived here |

For 2 < λ < 3, the demo uses these results, obtained from the same saddle point:
- The bulk order parameter is ⟨m̃⟩∞ ∝ K̃^((λ−2)/(3−λ)).
- The critical window shrinks as ε ∼ N^−[1 − (λ−1)/ω]/2.
- At ε = 0, ⟨m̃⟩ ∝ N^−[1 + (3λ−7)/ω]/4. At the natural cutoff this becomes N^−(λ−2)/(λ−1).
- In f(m̃), the part beyond the quadratic term grows as m̃^((λ−1)/(λ−2)), a power above 2. Once k_c → ∞, the −z₁K̃m̃²/2 term therefore always wins at small m̃.
- At the natural cutoff even z₂ is not self-averaging, so the pseudo-critical point itself fluctuates from sample to sample.

These results come from inserting the finite-N moments z₂ ∼ k_c^(3−λ) and z₄ ∼ k_c^(5−λ) into the Landau coefficients of Eq. (20). I checked them against the numerical saddle-point integral up to N = 10⁸:
- At λ = 2.5 the slopes agree to within about 0.01, for both ω = 3 and the natural cutoff.
- At λ = 2.7, ω = 2 the slopes are still converging at N = 10⁸.
- Convergence is slower for forced cutoffs, because corrections from the low-degree nodes decay only as k_c^(λ−3).

At λ = 3 the corrections are logarithmic.

## Running it

The demo is a single self-contained file, `index.html`, with no build step and no dependencies. You can run it in two ways:

- **Locally:** open `index.html` in any modern browser.
- **GitHub Pages:** push this folder to a repository and enable Pages from the repository settings.

All computation runs in the browser. The only external request is to Google Fonts, and the page falls back to system fonts if that request fails.

## Parameters

Four parameters are shared by every panel:

| Parameter | Meaning |
|-----------|---------|
| λ | Degree exponent, from 2.2 to 7 |
| ω | Cutoff exponent, from ω_nat = λ − 1 upward |
| d | Cutoff prefactor |
| k₀ | Lower degree cutoff, fixed at 3 as in the paper |

The presets load the paper's Figs. 4, 5, 5(b) and 6, plus two λ = 2.5 cases: the natural cutoff and ω = 3.

## What the page shows

### 1. Live globally coupled network

Metropolis Monte Carlo runs on the Hamiltonian above, drawn as a network diagram:
- **Layout.** Nodes sit on a sunflower spiral with the hubs at the centre. Node area is proportional to the nominal degree k_i, and node colour shows the spin.
- **Every pair is drawn.** Each of the N(N−1)/2 pairs is a line whose opacity is proportional to its weight k_i k_j, so the shading *is* the coupling K̃ k_i k_j / N z₁ up to a constant.
- **Normalization.** The peak opacity is inversely proportional to the effective number of lines, N_eff = Σ_{i<j} k_i k_j / k_max². This keeps the total shading comparable across λ and N. Lines too faint to register on screen are skipped.
- **Square-root scale.** An optional √(k_i k_j) scale shows the weaker hub–leaf couplings, which for λ < 3 are otherwise invisible next to the hub–hub ones.
- **Frustration tint.** Among the 496 pairs of the 32 largest-weight nodes, those whose spins disagree are tinted red.

The weighted lines only change when the network changes, so they are rendered once into a cached layer. Each frame then redraws only the spins and the tint, in under 1 ms at N = 1,200.

A histogram of m̃ from the run is plotted against the saddle-point weight e^(−N f(m̃)), next to a time trace.

### 2. Theory

- **Degree sequence:** the deterministic sequence of Eq. (54), with k_c, the moments, K̃c(N) and either K̃c^∞ or, for λ ≤ 3, the scaling of K̃c(N).
- **Free energy:** f(m̃) of Eq. (15) against its quartic Landau approximation, with the crossover m̃× = z₂/(z₁k_c) marked.
- **Order parameter:** ⟨m̃⟩ for N = 10³ to 10⁷, plotted against ε or against the bulk variable (ε_b for λ > 3, the absolute coupling K̃ for λ ≤ 3). The N = ∞ bulk curve is overlaid. For λ ≤ 3 the finite-N curves climb toward a bulk curve that is nonzero at every K̃ > 0.
- **Critical FSS:** ⟨m̃⟩ at ε = 0 against N, with the predicted slope. It is compared with ε_b = 0 for λ > 3, or with a fixed K̃ for λ ≤ 3. In the λ ≤ 3 case the magnetization is zero at small N and then saturates at the bulk value.

### 3. Monte Carlo test (paper Sec. IV)

- ⟨m̃⟩ against ε, with theory lines for each size.
- ⟨m̃⟩ at ε = 0 and at ε_b = 0 or fixed K̃, against N.
- **Scaling collapses:**
  - ψ(x) of Eq. (A3) for λ > 5.
  - Eq. (51), Eq. (52) or the natural-cutoff form for 3 < λ < 5.
  - For λ ≤ 3, the critical-window form or ⟨m̃⟩ against K̃ with the bulk envelope.

### 4. Sample-to-sample fluctuations (paper Sec. V)

Degree sequences are drawn at random, and the relative spreads of ⟨m̃⟩c, z₄ and K̃c = z₁/z₂ are compared with Eq. (58).

## Methods

**Degree sequences.**
- The deterministic sequence follows Eq. (54).
- Random sequences are multinomial draws, made with sequential binomials in O(k_c) time.
- Above k = 300, the integrals merge degrees into bins of 0.4% relative width, preserving each bin's weight and Σ n k². Checked against the uncompressed sum, this changes ⟨m̃⟩ by less than 10⁻⁷.

**Saddle point.** f(m̃) is evaluated parametrically in u₀, and ⟨|m̃|⟩ is computed from Eq. (16) with Simpson's rule on a window around the peak.

**Bulk (N = ∞).** The bulk curve is the largest root of m̃ = ⟨k tanh(K̃m̃k)⟩/z₁. The sum over degrees is exact up to k = 2000, and the tail is an integral. The bulk critical coupling K̃c^∞ = ζ(λ − 1, k₀)/ζ(λ − 2, k₀) uses the Hurwitz zeta function for λ > 3, and K̃c^∞ = 0 for λ ≤ 3.

**Monte Carlo.** Flipping spin i changes the energy by ΔH = (2K̃k_i/(N z₁))(s_i M̃ − k_i), so each update costs O(1).

**Reproducibility.** All random numbers come from a seeded PRNG (mulberry32), so every run gives the same results.

## Caveats

- **The λ ≤ 3 results are not from the paper.** They are derived here and checked numerically.
- **System size.** The Monte Carlo sizes go up to 32,768, compared with the paper's 32,768 × 10³. For λ ≤ 3 with a forced cutoff, the critical-window collapse drifts visibly at these sizes.
- **Large cutoffs.** Theory curves skip sizes with k_c > 3 × 10⁶, and random sampling skips sizes with k_c > 3 × 10⁵. This matters near λ = 2 with the natural cutoff.
- **What the fluctuation panel measures.** The spreads are purely from the sampled degrees, using saddle-point thermal averages. The width of the paper's Fig. 6 histogram also comes from Monte Carlo measurement.
- **Scope of the theory.** Logarithmic corrections, for example at λ = 3, λ = 5 or ω = ω_nat, are not included.

## References

1. S. H. Lee, M. Ha, H. Jeong, J. D. Noh, and H. Park, "Critical behavior of the Ising model in annealed scale-free networks," *Phys. Rev. E* **80**, 051127 (2009). [doi:10.1103/PhysRevE.80.051127](https://doi.org/10.1103/PhysRevE.80.051127)
