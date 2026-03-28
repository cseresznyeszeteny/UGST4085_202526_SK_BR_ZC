# Finite Difference Schemes for Two Variable Functions: Rumor Spreading Simulation

**Course:** UGST4085 — Numerical Methods in Calculus and Algebra 2025/26 Fall
**Institution:** Central European University
**Authors:** Zeteny Cseresznyes, Balazs Remenyi, Shiwam KC

## Overview

This project simulates rumor propagation on a 2D spatial grid using finite difference schemes. Rumor intensity is modeled as a scalar field evolving under a reaction-diffusion equation, combining spatial diffusion via the discrete Laplacian and a nonlinear local growth/decay function.

## Contents

- `FDS_model_UGST4085_SK_BR_ZC.ipynb` — Main Jupyter notebook with simulation and visualizations
- `FDS_report_UST4085_202526_SK_BR_ZC.pdf` — Written report

## Method

The governing equation is:

```
∂φ/∂t = D·∇²φ + G(φ, N[φ])
```

- **∇²φ** — 2D Laplacian approximated with a 5-point finite difference stencil
- **D = 0.4** — diffusion coefficient
- **G(φ, N[φ])** — nonlinear growth/decay function based on neighbor influence
- **Periodic boundary conditions** via `np.roll()`
- **Grid:** 100×100, **Time step:** dt = 0.75, **Frames:** 400

## Key Parameters

| Parameter | Value | Description |
|-----------|-------|-------------|
| Grid size | 100×100 | Spatial domain |
| D | 0.4 | Diffusion coefficient |
| dt | 0.75 | Time step |
| φ_m | 1.0 | Max rumor intensity |
| α, β | 6, 3 | Growth nonlinearity exponents |
| Seed | 42 | For reproducibility |

## Output

- Animated GIF (`rumor_animation_YYYYMMDDHHMM.gif`) of the full simulation
- Snapshot frames at selected timesteps (inferno colormap)

## Requirements

```bash
pip install numpy matplotlib pillow
```
