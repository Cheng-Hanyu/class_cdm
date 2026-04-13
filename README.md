# CLASS with Variable CDM Equation of State

This is a modified version of the [**CLASS**](https://github.com/lesgourg/class_public) code (v3.x). It introduces a variable equation of state for cold dark matter (CDM), parameterized as:

$$w_\mathrm{cdm}(a) = w_0 + w_a (1 - a)$$

where `w0_cdm = 0`, `wa_cdm = 0` recovers standard pressureless CDM. This allows cosmological constraints on departures from the standard CDM pressure assumption.

## Key Modifications

The main changes from the original CLASS code are marked with `/* Hanyu */` in the source. Modified files:

- `source/background.c` — CDM pressure and EoS computation; new `background_w_cdm()` function
- `source/input.c` — reads `w0_cdm`, `wa_cdm` parameters
- `source/perturbations.c` — CDM perturbation equations updated for variable EoS
- `source/fourier.c`, `source/harmonic.c` — power spectrum and harmonic space corrections
- `source/output.c` — outputs `w_cdm`, `(.)p_cdm` background quantities
- `include/background.h`, `include/perturbations.h` — new parameter and index definitions

## New Input Parameters

Add to your `.ini` file:

```ini
w0_cdm = 0.0   # CDM EoS w0 (0 → standard pressureless CDM)
wa_cdm = 0.0   # CDM EoS wa (0 → standard)
```

## Compilation

```bash
make clean
make -j class       # compile binary
make -j             # compile binary + Python wrapper
```

## Citation

If you use this code, please cite:
- **Cheng et al. (2025)** (in preparation)
- The original CLASS paper: [Blas et al. (2011)](https://arxiv.org/abs/1104.2933)
