# Finite Difference Schemes for Two-Variable Functions: Rumor Spreading Simulation

**Course:** UGST4085 — Numerical Methods in Calculus and Algebra 2025/26 Fall
**Institution:** Central European University
**Authors:** Balazs Remenyi, Shiwam KC, Zeteny Cseresznyes
**Date:** March 28, 2026

## Overview

This project demonstrates finite difference schemes for numerically solving 2D PDEs, with a sociological application: modeling rumor spreading as a reaction-diffusion process. The report reviews the classical 5-point Laplacian stencil, classifies second-order PDEs, and applies the framework to simulate how rumors propagate and saturate across a spatial grid.

## Contents

- `FDS_model_UGST4085_SK_BR_ZC.ipynb` — Simulation notebook (implementation and animation)
- `FDS_report_UST4085_202526_SK_BR_ZC.pdf` — Written report

## Background

Finite difference methods convert continuous PDEs on a discrete grid into sparse linear systems Au = b. The **5-point stencil** approximates the 2D Laplacian at each interior point using its four cardinal neighbors:

```
Δu(xᵢ,yⱼ) ≈ (uᵢ₊₁,ⱼ + uᵢ₋₁,ⱼ + uᵢ,ⱼ₊₁ + uᵢ,ⱼ₋₁ − 4uᵢ,ⱼ) / h²
```

This is second-order accurate (O(h²)) and yields a symmetric positive definite (SPD) matrix, guaranteeing stability and convergence (Lax Equivalence Theorem).

**PDE classification by discriminant Δ = B² − AC:**
- Δ < 0 → Elliptic (Laplace/Poisson)
- Δ = 0 → Parabolic (heat equation)
- Δ > 0 → Hyperbolic (wave equation)

## Rumor Spreading Model

The governing reaction-diffusion PDE:

```
∂u/∂t = D·∇²u + G(u, N[u])
```

where `u(i,j)` is rumor awareness at each grid point, `D` is the diffusion coefficient (social connectivity), and `G` is a nonlinear growth/decay function:

```
G = (Nᵢ,ⱼ + ε)^α · max(uₘ − u, 0)^β − u · (1 − Nᵢ,ⱼ / (uₘ + ε))
```

`Nᵢ,ⱼ` is the average awareness of neighboring cells, capturing local social influence.

### Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| Grid size | 100×100 | Spatial domain |
| D | 0.3 | Diffusion coefficient |
| dt | 0.75 | Time step |
| uₘ | 1.0 | Max awareness (saturation) |
| α, β | 6, 3 | Reaction nonlinearity exponents |
| ε | 1e-12 | Numerical stability term |
| Frames | 400 | Simulation duration |
| Seed | 42 | Reproducibility |

Boundary conditions are periodic (wraparound via `np.roll()`).

### Simulation Phases

1. **Initiation** — Rumor starts at a seeded point; diffusion drives a smooth circular expansion
2. **Rapid spread** — Nonlinear reaction term amplifies awareness as wavefronts propagate
3. **Saturation** — Grid approaches `uₘ = 1`; reaction dampens and diffusion smooths residuals

### Sociological Interpretation

- **D** encodes social connectivity — larger D means faster, wider spread
- **α, β** encode sensitivity to neighborhood influence and growth sharpness
- The logistic-like structure (growth → saturation) mirrors real information cascades
- Symmetry in output reflects idealized uniform grid; real social networks would produce irregular patterns

## Limitations

- Assumes a regular 2D grid (4 neighbors per person); real social networks are irregular
- No forgetting, disbelief, or variable individual behavior
- Single rumor source; no competing information modeled
- Stochastic effects not included

## Output

- Animated GIF (`rumor_animation_YYYYMMDDHHMM.gif`) of the full simulation
- Snapshot frames at selected timesteps (inferno colormap: black → yellow = low → high awareness)

## Requirements

```bash
pip install numpy matplotlib pillow
```

## References

1. LeVeque, R. J. *Finite Difference Methods for Ordinary and Partial Differential Equations.* SIAM, 2007.
2. Hoffman, J. D. & Frankel, S. *Numerical Methods for Engineers and Scientists.* CRC Press, 2018.
3. Chapra, S. C. & Canale, R. P. *Numerical Methods for Engineers.* McGraw-Hill, 2020.
