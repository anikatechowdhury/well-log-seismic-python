# well-log-seismic-python

![Python](https://img.shields.io/badge/Python-3.8+-blue)
![License](https://img.shields.io/badge/License-MIT-green)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

A Python toolkit for subsurface geophysical analysis integrating well log petrophysics and synthetic seismogram generation. The workflow follows a standard exploration geophysics pipeline — from raw log curves through petrophysical evaluation to seismic forward modelling.

---

## Overview

This project implements two interconnected geophysical workflows in Python:

**Module 1 — Well Log Petrophysical Analysis**
Loading and QC of LAS-format well logs, lithology identification, porosity and water saturation computation, and multi-track log visualisation.

**Module 2 — Synthetic Seismogram Generation**
Derivation of acoustic impedance and reflection coefficients from well log data, Ricker wavelet generation, and convolution to produce a synthetic seismic trace — directly linking subsurface rock properties to seismic response.

---

## Workflow

```
LAS File (GR, SP, ILD, RHOB, NPHI, DT, CALI)
        │
        ▼
  Module 1: Petrophysical Analysis
  ├── Lithology flag (GR cutoff)
  ├── Vshale (GR linear method)
  ├── Density Porosity (PHID)
  ├── Average Porosity (PHIA)
  └── Water Saturation (Archie's Equation)
        │
        ▼
  Module 2: Synthetic Seismogram
  ├── Velocity from DT (µs/ft → m/s)
  ├── Acoustic Impedance (AI = Vp × ρ)
  ├── Reflection Coefficients (RC)
  ├── Ricker Wavelet — bruges (40 Hz)
  └── Convolution → Synthetic Trace
```

---

## Petrophysical Equations

**Density Porosity:**
$$\phi_D = \frac{\rho_{ma} - \rho_b}{\rho_{ma} - \rho_{fl}}$$

**Vshale (Linear GR):**
$$V_{sh} = \frac{GR - GR_{clean}}{GR_{shale} - GR_{clean}}$$

**Water Saturation (Archie):**
$$S_w = \left(\frac{a \cdot R_w}{\phi^m \cdot R_t}\right)^{1/n}$$

**Acoustic Impedance:**
$$AI = V_p \times \rho_b$$

**Reflection Coefficient:**
$$RC = \frac{AI_2 - AI_1}{AI_2 + AI_1}$$

---

## Dataset

Synthetic LAS file — `sample.las` — simulating a 600 m sedimentary section (1500–2100 m depth) with alternating shale, sand, and carbonate intervals:

| Lithology | GR (API) | RHOB (g/cc) | DT (µs/ft) |
|---|---|---|---|
| Shale | 80–120 | 2.40–2.65 | 90–120 |
| Sand | 15–45 | 2.10–2.45 | 55–90 |
| Carbonate | 10–30 | 2.50–2.71 | 45–65 |

---

## Project Structure

```
well-log-seismic-python/
│
├── generate_las.py          # Synthetic LAS file generator
├── sample.las               # Synthetic well log (LAS format)
├── well_log_seismic.py      # Main analysis script (both modules)
└── README.md
```

---

## Dependencies

| Library | Purpose |
|---|---|
| `lasio` | Read and write LAS well log files |
| `numpy` | Array operations, convolution, FFT |
| `pandas` | Log data handling and computation |
| `matplotlib` | Multi-track log plots, seismic display |
| `bruges` | Ricker wavelet generation (geophysics utilities) |
| `scipy` | Signal processing support |

```bash
pip install lasio numpy pandas matplotlib bruges scipy
```

---

## Usage

```bash
# Step 1: Generate synthetic LAS file
python generate_las.py

# Step 2: Run full analysis
python well_log_seismic.py
```

---

## Outputs

| File | Description |
|---|---|
| `well_log_tracks.png` | 8-track well log display with lithology, GR, SP, ILD, RHOB, NPHI, DT, PHIA |
| `neutron_density_crossplot.png` | NPHI vs RHOB crossplot for lithology identification |
| `petrophysical_distributions.png` | Histograms of PHIA, Sw, Vsh |
| `synthetic_seismogram.png` | Side-by-side display: GR, AI, RC, synthetic trace, wavelet |
| `wavelet_spectrum.png` | Ricker wavelet in time and frequency domain |

---

## Author

**Anikate Chowdhury**
M.Sc. Applied Geology (2nd Year), Presidency University, Kolkata
GitHub: [github.com/anikatechowdhury](https://github.com/anikatechowdhury)

---

## License

MIT License
