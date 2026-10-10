Computational library for the SU(3)_dark self-interacting dark matter framework
with an inverted dark hypercharge hierarchy.

## Physics

The dark sector organizes into the baryon-like **8** of SU(3)_dark, decomposing as

    8 = 2_{+1} ⊕ 3_0 ⊕ 1_0 ⊕ 2_{-1}

giving dark-nucleon doublets N_dark (Y=+1), a dark-Lambda singlet and dark-Sigma triplet
(Y=0), and dark-cascade doublets Xi_dark (Y=-1).

The Gell-Mann–Okubo mass formula with β < 0, γ < 0 (both negative, consistent with the
QCD sign convention) places N_dark^0 at the bottom of the spectrum:

    M(I, Y, I₃) = M₀ + β Y + γ[Y²/4 − I(I+1)] + δ_iso · I₃

with M₀ = 177.52 GeV, β = −15 GeV, γ = −8 GeV, δ_iso = +1.70 GeV.

The Gell-Mann–Okubo sum rule

    2(M̄_N + M̄_Ξ) = 3M_Λ + M̄_Σ

holds as an exact algebraic identity and constitutes a parameter-free, falsifiable
prediction for FCC-ee spectroscopy.

Self-interactions are mediated by the surviving light pseudo-Goldstone φ₁ (mass 70 MeV,
coupling g_φ = 0.130), giving σ/m ≈ 0.47 cm²/g at dwarf scales and ≈ 0.0044 cm²/g at
cluster scales. Dark matter annihilation via N N → Z′Z′ with g_dark = 0.130,
m_Z′ = 10 GeV gives Ωh² ≈ 0.120. The dark sector contribution ΔN_eff ≈ 0.086 is within
Planck 2σ bounds.

## Installation

```bash
git clone https://github.com/ahmed19999520-alt/dark_sector_octet
cd dark_sector_octet
pip install -e ".[mcmc,cluster]"
```

## Docker

```bash
docker compose -f docker/docker-compose.yml up notebook
# then open http://localhost:8888
```

## Quick start

```python
from dark_sector_octet import (
    build_octet, gmo_sum_rule, mass_ratios, print_spectrum,
    sigma_over_m, delta_neff, dao_scale,
    stability_analysis, pi2_coset,
)

states = build_octet()
print_spectrum(states)

lhs, rhs, diff = gmo_sum_rule(states)
print(f"GMO sum rule residual: {diff:.2e} GeV")

print(f"sigma/m(10 km/s)   = {sigma_over_m(10.0):.4f} cm2/g")
print(f"sigma/m(1000 km/s) = {sigma_over_m(1000.0):.5f} cm2/g")
print(f"Delta_Neff = {delta_neff():.4f}")
print(f"r_DAO = {dao_scale():.3f} Mpc")

sa = stability_analysis()
for m in sa["stability_mechanisms"]:
    print(" -", m)
```

## Repository layout

```
dark_sector_octet/
├── dark_sector_octet/
│   ├── constants.py         physical constants and benchmark parameters
│   ├── spectrum.py          GMO mass formula, octet states, sum rules
│   ├── sidm.py              self-interaction cross-sections
│   ├── sommerfeld.py        Sommerfeld enhancement (Coulomb, Hulthen, numerical)
│   ├── boltzmann.py         coupled Boltzmann equations, freeze-out
│   ├── homotopy.py          homotopy groups, topological stability (Appendix A)
│   ├── neff.py              ΔN_eff calculation
│   ├── dao.py               dark acoustic oscillation scale
│   ├── higgs_triplet.py     dark Higgs triplet sector (Appendix C)
│   ├── stueckelberg.py      Higgs-Stückelberg mechanism (Appendix C)
│   ├── decay.py             two-body decay widths and lifetimes
│   ├── torch_scan.py        PyTorch gradient-based parameter scan
│   └── parameter_space.py   differential evolution scan, profile likelihood
├── notebooks/               Jupyter notebooks (one per topic)
├── mathematica/             Wolfram Language scripts
├── tests/                   pytest test suite
├── docker/                  Dockerfile and docker-compose.yml
└── .github/workflows/ci.yml GitHub Actions CI
```

## Tests

```bash
pytest tests/ -v
```

## Key numerical outputs

| Observable            | Value         | Source                    |
|-----------------------|---------------|---------------------------|
| M(N_dark^0)           | 165.67 GeV    | GMO formula               |
| M(N_dark^+)           | 167.37 GeV    | GMO + δ_iso               |
| M(Λ_dark)             | 177.52 GeV    | GMO formula               |
| M(Σ_dark^0)           | 193.52 GeV    | GMO formula               |
| M(Ξ_dark^0)           | 197.37 GeV    | GMO formula               |
| GMO sum rule diff     | 0.00 GeV      | exact algebraic identity  |
| r31 = M(Λ)/M(N0)      | 1.0716        | GMO formula               |
| double ratio          | 2.35          | SU(3) group theory        |
| σ/m at 10 km/s        | 0.47 cm²/g    | Born + Yukawa saturation  |
| σ/m at 1000 km/s      | 0.0044 cm²/g  | Born + Coulomb limit      |
| ΔN_eff                | 0.086         | T_dec ≈ 500 MeV           |
| r_DAO                 | 2.3 Mpc       | T_kd ≈ 10 MeV, R=1.5      |
| π₂(G/H)               | Z_2           | homotopy exact sequence   |

## References

Ali, A., "Dark Sector Flavor Symmetry: Gell-Mann–Okubo Mass Splittings for
SU(3)_dark Self-Interacting DM,"in preparation (2026).
