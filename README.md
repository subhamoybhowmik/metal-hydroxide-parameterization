# Thermodynamics of Metal–Hydroxide Interactions

This repository contains selected molecular dynamics (MD) simulation inputs, numerical potential-of-mean-force (PMF) profiles, thermodynamic integration (TI) setup files, and quantum-chemical input structures associated with the manuscript **“Thermodynamics of Metal-Hydroxide Interactions”** by Subhamoy Bhowmik, Zhen Li, and Kenneth M. Merz, Jr. (*Journal of Chemical Theory and Computation*; publication details to be added).

The study develops modified 12-6-4 Lennard-Jones (m-12-6-4 LJ) parameters for the interactions of hydroxide with **Mg(II), Ca(II), Mn(II), Fe(II), Co(II), Ni(II), Cu(II), Zn(II), and Cd(II)** in **OPC** and **TIP3P** water. Parameterization uses hydroxide hydration free energies and metal–hydroxide PMFs; metal-ion transformation TI calculations and quantum-chemical metal–oxygen distances provide additional tests.

## Repository organization

```text
pmf/
├── free_energy/
│   ├── txt_files/                  Final PMF profiles (OPC and TIP3P)
│   ├── metafile_1                  Representative WHAM metadata
│   └── 300_wham_1.sh              Representative WHAM command
├── input_files/
│   ├── opc/                        System files and setup examples for OPC
│   ├── tip3p/                      System files and setup examples for TIP3P
│   └── pmf_mdin_files_representative/
│                                   Example steered-MD and umbrella-sampling inputs
└── restraint_files/               Representative steered-MD and umbrella restraints

ti/
├── input_files/
│   ├── hfe/                        Hydroxide hydration free-energy TI setup
│   ├── opc/                        Metal-transformation systems and parameters (OPC)
│   ├── tip3p/                      Metal-transformation systems and parameters (TIP3P)
│   └── ti_mdin_files_representative/
│                                   Representative TI inputs, including lambda windows
└── restraint_files/               TI restraint example

quantum_opt/
├── inputs/                         ORCA calculation inputs
└── opt_xyz/                        Optimized structures in XYZ format
```

## Potential-of-mean-force calculations (`pmf/`)

The `pmf/free_energy/txt_files/` directory contains PMF profiles for the nine metal ions in both water models. Filenames identify the metal and water model, for example `co_opc.txt` or `zn_tip3p.txt`. Three independently labeled Ni(II)–OPC profiles are stored as `ni_opc_1.txt`, `ni_opc_2.txt`, and `ni_opc_3.txt`.

The text files have five columns:

| Column | Description |
| --- | --- |
| `Coor` | Metal–hydroxide separation (Å) |
| `Free` | Relative free energy (kcal/mol) |
| `+/-` | Reported PMF uncertainty (kcal/mol) |
| `Prob` | Probability density reported by WHAM |
| `+/-` | Reported uncertainty in the probability density |

The `input_files/` folders contain AMBER topology (`.prmtop`) and coordinate (`.inpcrd`) files, molecular descriptions, and representative LEaP/ParmEd inputs. Example control files for steered MD, equilibration, and umbrella sampling are under `pmf_mdin_files_representative/`; example restraint definitions are under `restraint_files/`.

The WHAM shell command and metadata illustrate the analysis setup. Their file paths and window references are representative and must be adapted when processing the corresponding raw window time series; those raw per-window time series are not included in this deposit.

## Thermodynamic integration calculations (`ti/`)

The `ti/input_files/opc/` and `ti/input_files/tip3p/` directories provide representative metal-ion transformation setups and associated topology/coordinate files. The directory `ti_mdin_files_representative/` contains example AMBER TI control files, including the lambda-window inputs. Files in `ti/input_files/hfe/` document the separate TI setup used for hydroxide hydration free-energy calculations.


## Quantum-chemical calculations (`quantum_opt/`)

The `quantum_opt/inputs/` folder contains ORCA input files for metal–hydroxide model systems. Corresponding optimized geometries are provided in `quantum_opt/opt_xyz/` as XYZ files. These files support the comparison of metal–oxygen coordination distances obtained from MD and density functional theory.

## Modified 12-6-4 Lennard-Jones setup

The m-12-6-4 LJ model requires appropriately constructed AMBER topology files, including the intended pair-specific `C4` terms. Representative LEaP and ParmEd inputs are included in the relevant `input_files/` directories. The `c4.txt` files and polarizability/parameter files used by those inputs should be kept together with the corresponding setup files in the final repository.

**Detailed step-by-step tutorial:**

[AMBER tutorial — Implementing modified 12-6-4 LJ potential using ParmEd](https://ambermd.org/tutorials/advanced/tutorial20/m1264.php)

## Software and methods

- **AMBER 24 / AmberTools:** system preparation, ParmEd parameter modification, and classical MD/TI calculations
- **LEaP, ParmEd, and PMEMD.cuda:** system assembly, nonbonded-parameter setup, and dynamics
- **OPC and TIP3P:** explicit water models
- **WHAM:** reconstruction of the PMF profiles from umbrella-sampling data
- **ORCA:** quantum-chemical geometry optimizations

The manuscript and its Supporting Information describe the methodology, simulation protocols, and interpretation of the results in greater detail.


## Citation

If you use the parameterization or data provided here, please cite:

> Bhowmik, S.; Li, Z.; Merz, K. M., Jr. **Thermodynamics of Metal-Hydroxide Interactions.** *Journal of Chemical Theory and Computation*. [Publication details and DOI to be added.]

Please also cite the relevant AMBER and methodological references given in the paper when reusing the computational workflow.
