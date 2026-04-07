# sha-poc — Time-modulated array (TMA) simulation

MATLAB scripts that simulate a **time-modulated phased array**: several RF paths are switched with **non-overlapping square-wave LOs** at a modulation rate \(f_\mathrm{TM}\), with a **progressive time delay** \(\tau = T_\mathrm{TM}/N\) per channel. The model illustrates **harmonic content** in the combined output and the **spatial-to-spectral** idea that different harmonics can correspond to different beam directions.

This is a compact **proof-of-concept** for exploring parameters and generating figures (the code comments reference “Figure 5(a)/(b)” style plots).

## Requirements

- **MATLAB** (R2019b or newer is typical for the APIs used; adjust if your install is older.)
- **Signal Processing Toolbox** — the scripts use `square` for the periodic LO waveforms.

No additional MATLAB path setup is required if you run scripts from this folder.

## Quick start

1. Add the project folder to the MATLAB path or `cd` into it.
2. Run either script from the Command Window:

   ```matlab
   sha-pocm
   ```

   or

   ```matlab
   harmonic-loss-comparison
   ```

3. Several figure windows will open (LO waveforms, output spectrum, beam patterns vs angle, frequency–angle mapping, and a sweep over \(\tau\)). Console output summarizes \(N\), \(f_\mathrm{TM}\), and \(\tau\).

## What the simulation does

| Piece | Role |
|--------|------|
| **\(N\) channels** | Each channel has a delayed square wave; duty cycle \(1/N\) so slots do not overlap in the nominal design. |
| **\(f_\mathrm{RF}\), \(\lambda\), spacing \(d\)** | Carrier frequency and half-wavelength spacing for spatial phase. |
| **Harmonics \(m\)** | Fourier coefficients \(\beta_m\) of the periodic switching; spectrum at \(f_\mathrm{RF} + m f_\mathrm{TM}\). |
| **Beam patterns** | For each \(m\), array factor vs scan angle \(\theta\), normalized and plotted in dB. |
| **Peak-angle curve** | Relates harmonic index / frequency offset to a nominal beam direction; includes a comparison for multiple \(\tau\) values. |

Default numerical example in code: **\(N = 5\)**, **\(f_\mathrm{RF} = 28\) GHz**, **\(f_\mathrm{TM} = 1\) GHz**, example AoA **30°** for the spectrum stem plot.

## Repository files

| File | Description |
|------|-------------|
| [`sha-pocm.m`](sha-pocm.m) | Main TMA script. Uses a **high** sampling rate `fs = 2000*f_TM` for the time-domain LO subplot so the square edges are well resolved. |
| [`harmonic-loss-comparison.m`](harmonic-loss-comparison.m) | Same structure and figures as `sha-pocm.m`, but `fs = 20*f_TM` — coarser sampling of the LO waveforms (useful to see **staircase / harmonic loss** effects in the **time-domain LO plot**; the closed-form harmonic / beam formulas are unchanged). |

Autosave and build artifacts are ignored via [`.gitignore`](.gitignore) (e.g. `*.asv`, MEX, Simulink caches).

## Tweaking parameters

Edit the **Parameters** section at the top of either script:

- **`N`** — number of channels / time slots per period.
- **`f_RF`**, **`f_TM`** — RF and modulation frequencies (Hz).
- **`theta_aoa`** — angle of arrival (degrees) for the spectrum example.
- **`num_harmonics`** — how many \(m\) values to include on each side of DC.
- **`tau_values`** (in the last figure block) — scaling of progressive delay relative to \(T_\mathrm{TM}/N\).

## License

Not specified in this repository. Add a `LICENSE` file if you distribute the code.
