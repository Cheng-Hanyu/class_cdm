# CLASS with a Late-Onset CDM Equation of State

This modified version of [CLASS](https://github.com/lesgourg/class_public/tree/v3.2.3)
(v3.2.3) introduces a CDM equation of state that is zero before a transition
scale factor and varies linearly afterwards. With the present scale factor
normalized to $a=1$, the equation of state is as follows.

Before the transition, $a\lt a_{\mathrm{nz}}$:

```math
w_{\mathrm{cdm}}(a)=0.
```

At and after the transition, $a\geq a_{\mathrm{nz}}$:

```math
w_{\mathrm{cdm}}(a)=w_{\mathrm{dm},0}\dfrac{a-a_{\mathrm{nz}}}{1-a_{\mathrm{nz}}}.
```

The background density implemented in the code is as follows.

Before the transition, $a\lt a_{\mathrm{nz}}$:

```math
\frac{\rho_{\mathrm{cdm}}(a)}{\rho_{\mathrm{cdm},0}}=
a^{-3}\exp\!\left[\dfrac{3w_{\mathrm{dm},0}}{1-a_{\mathrm{nz}}}
\left(1-a_{\mathrm{nz}}+a_{\mathrm{nz}}\ln a_{\mathrm{nz}}\right)\right].
```

At and after the transition, $a\geq a_{\mathrm{nz}}$:

```math
\frac{\rho_{\mathrm{cdm}}(a)}{\rho_{\mathrm{cdm},0}}=
a^{-3}\exp\!\left[\dfrac{3w_{\mathrm{dm},0}}{1-a_{\mathrm{nz}}}
\left(1-a+a_{\mathrm{nz}}\ln a\right)\right].
```

The density is normalized at $a=1$ and continuous at the transition.
Use $a\gt0$ and $0\leq a_{\mathrm{nz}}\lt1$; $a_{\mathrm{nz}}=1$ is singular.
When $a_{\mathrm{nz}}=0$, only the second branch is used for $a\gt0$.
These are the mathematical domain restrictions; the input parser does not
enforce them. Setting $w_{\mathrm{dm},0}=0$ recovers pressureless CDM.

This is distinct from the [CPL CDM fork](https://github.com/Cheng-Hanyu/class_ddm)
and its [quadratic extension](https://github.com/Cheng-Hanyu/class_ddm_extension).

## Key Modifications

Relative to official CLASS v3.2.3, the custom C-source and header changes are
confined to the following files:

- [`source/background.c`](source/background.c) — CDM density, pressure, equation of state and its derivative; `background_w_cdm()`; the `w_cdm` and `(.)p_cdm` background output columns.
- [`source/input.c`](source/input.c) — input parsing and default values for the model parameters listed below.
- [`source/perturbations.c`](source/perturbations.c) — CDM density and velocity perturbation equations in Newtonian and synchronous gauges, including the time derivative of the equation of state. The implemented CDM rest-frame sound speed is fixed to zero.
- [`include/background.h`](include/background.h) — model parameters, background indices and the `background_w_cdm()` declaration.

These are modifications of the CDM sector. CLASS's separate, built-in decaying
dark matter sector is not a new feature of this fork. No custom changes to
`source/thermodynamics.c`, `source/output.c`, `source/fourier.c` or
`source/harmonic.c` are needed for the modifications listed above.

## New Input Parameters

| Input name | Symbol | Meaning | Default |
| --- | --- | --- | --- |
| `a_nz` | $a_{\mathrm{nz}}$ | Scale factor at which the CDM equation of state starts departing from zero | `0.0` |
| `w_dm_0` | $w_{\mathrm{dm},0}$ | Present-day CDM equation of state | `0.0` |

`w0_cdm` and `wa_cdm` belong to the separate CPL fork; they are not the
custom parameters read by this code.

## Compilation

The standalone code requires a C compiler, a C++11 compiler and `make`.
The legacy Python wrapper also requires NumPy, Cython and setuptools in the
active Python environment. Use a separate environment to avoid replacing a
different CLASS installation.

```bash
make -j class                 # standalone executable
make -j all PYTHON=python     # executable, library and Python wrapper
```

The second command installs the wrapper into the active Python environment.
For an in-place wrapper build without installation:

```bash
make -j libclass.a
cd python
python setup.py build_ext --inplace
```

Compiler and OpenMP settings are controlled by the supplied `Makefile`.
See the [upstream installation documentation](https://github.com/lesgourg/class_public/wiki/Installation)
for platform-specific compiler configuration.

## Citation

If you use this code, please cite the original CLASS paper:

- D. Blas, J. Lesgourgues and T. Tram, *The Cosmic Linear Anisotropy Solving System (CLASS). II. Approximation schemes*, JCAP 07 (2011) 034, [arXiv:1104.2933](https://arxiv.org/abs/1104.2933).

Please also identify this repository and the commit used when describing the
modified model. Upstream author acknowledgements, citation requirements and
third-party notices continue to apply.
