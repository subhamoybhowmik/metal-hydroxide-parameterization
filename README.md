# Thermodynamics of Metal–Hydroxide Interactions

**Supporting simulation inputs and research data for the manuscript**  
*Thermodynamics of Metal-Hydroxide Interactions*  
Subhamoy Bhowmik, Zhen Li, and Kenneth M. Merz, Jr.  
*Journal of Chemical Theory and Computation* — publication details/DOI to be added when available.

## Overview

This repository accompanies a study of aqueous metal–hydroxide interactions modeled using a **modified 12-6-4 Lennard-Jones (m-12-6-4 LJ) potential**. The work develops hydroxide-specific nonbonded parameters for nine divalent metal ions:

**Mg(II), Ca(II), Mn(II), Fe(II), Co(II), Ni(II), Cu(II), Zn(II), and Cd(II)**

Calculations were performed with two explicit water models, **OPC** and **TIP3P**. The hydroxide–water interaction was first adjusted using the hydration free energy of hydroxide. Metal–hydroxide interactions were then refined using potential-of-mean-force (PMF) calculations and experimental formation constants. Thermodynamic integration (TI) and quantum-chemical metal–oxygen distances provided additional validation.

The repository contains **selected inputs, parameter/setup files, analyzed PMF profiles, additional PMF and TI data, and optimized quantum-chemical structures**. It is intended to document the calculations and facilitate reuse of the parameterization.

## Repository structure

```text
metal-hydroxide-parameterization/
├── pmf/
│   ├── free_energy/
│   │   ├── txt_files/                       # Final WHAM-derived PMF profiles
│   │   ├── metafile_1                       # Representative WHAM metadata
│   │   └── 300_wham_1.sh                    # Representative WHAM command
│   ├── input_files/
│   │   ├── opc/                             # OPC system/setup files
│   │   ├── tip3p/                           # TIP3P system/setup files
│   │   └── pmf_mdin_files_representative/   # Example AMBER MD inputs
│   └── restraint_files/                    # Example pulling/umbrella restraints
├── pmf_data/                               # Additional PMF-related data (e.g., saved snapshots)
├── ti/
│   ├── input_files/
│   │   ├── hfe/                             # Hydroxide hydration free-energy TI
│   │   ├── opc/                             # Metal-transformation TI setups, OPC
│   │   ├── tip3p/                           # Metal-transformation TI setups, TIP3P
│   │   └── ti_mdin_files_representative/    # Example TI window inputs
│   └── restraint_files/                    # Representative TI restraint file(s)
├── ti_data/                                # Numerical TI dV/dλ data and related files
├── quantum_opt/
│   ├── inputs/                              # ORCA input files
│   └── opt_xyz/                             # Optimized geometries (.xyz)
└── README.md
```

The tree above lists the principal folders and representative files; it is not an exhaustive inventory of every file within each directory. 

## 1. PMF calculations (`pmf/`)

The [`pmf/free_energy/txt_files/`](pmf/free_energy/txt_files/) directory contains the final numerical PMF profiles for metal–hydroxide separation in OPC and TIP3P water. File names identify the metal and water model, for example:

- `co_opc.txt` — Co(II)–hydroxide PMF in OPC water.
- `zn_tip3p.txt` — Zn(II)–hydroxide PMF in TIP3P water.
- `ni_opc_1.txt`, `ni_opc_2.txt`, `ni_opc_3.txt` — separately labeled Ni(II)–OPC profiles.

The WHAM output uses five numerical columns:

| Column | Meaning |
| --- | --- |
| `Coor` | Metal–hydroxide separation (Å) |
| `Free` | Relative free energy (kcal/mol) |
| First `+/-` | Reported uncertainty in relative free energy (kcal/mol) |
| `Prob` | WHAM probability density |
| Second `+/-` | Reported uncertainty in probability density |


The [`pmf/input_files/`](pmf/input_files/) directories contain example AMBER topology/coordinate files and system-preparation materials for the respective water models. Representative AMBER input files for equilibration, steered MD, and umbrella sampling are under `pmf_mdin_files_representative/`. The [`pmf/restraint_files/`](pmf/restraint_files/) directory provides example restraint definitions.

The WHAM metadata (`metafile_1`) and command (`300_wham_1.sh`) are included to document an example analysis configuration. **Paths and window-specific filenames in these examples must be adjusted** for a new calculation. The analyzed PMFs are supplied directly.

## PMF supporting data (`pmf_data/`)

The [`pmf_data/`](pmf_data/) directory is a separate location for additional PMF-related data. It complements the final WHAM-derived free-energy profiles in `pmf/free_energy/txt_files/` and the representative simulation inputs in `pmf/input_files/`.

This directory includes compact coordinate snapshots saved from PMF simulations.

## 2. Thermodynamic integration (`ti/`)

TI calculations were used to validate the PMF-derived interactions through alchemical transformations between divalent metal ions, considering systems with and without coordinated hydroxide.

- [`ti/input_files/opc/`](ti/input_files/opc/) and [`ti/input_files/tip3p/`](ti/input_files/tip3p/) contain metal-transformation preparation and input materials for the two water models.
- [`ti/input_files/hfe/`](ti/input_files/hfe/) documents the separate hydroxide hydration free-energy setup.
- [`ti/input_files/ti_mdin_files_representative/`](ti/input_files/ti_mdin_files_representative/) provides representative AMBER TI control files.
- [`ti/restraint_files/`](ti/restraint_files/) contains an example restraint definition used where a metal–hydroxide contact needs to be maintained.

The metal-transformation protocol described in the manuscript used **12 Gauss–Legendre λ windows**:

```text
0.00922  0.04794  0.11505  0.20634  0.31608  0.43738
0.56262  0.68392  0.79366  0.88495  0.95206  0.99078
```

The reported protocol used 2 ns per TI window and three independent repeats. The transformation free energy is obtained by integrating the ensemble-averaged `dV/dλ` values over λ with the corresponding quadrature weights.

The full thermodynamic cycles and their comparison with the PMF-derived free energies are described in the manuscript and Supporting Information.

## TI numerical data (`ti_data/`)

The [`ti_data/`](ti_data/) directory holds supporting TI numerical data, separate from the AMBER setup and representative control files in `ti/`. The collected `dV/dλ` tables for **OPC** and **TIP3P** summarize the average potential-energy derivative extracted at each of the 12 alchemical windows for Zn(II)-reference metal transformations in the presence of hydroxide.

## 3. Quantum-chemical structures (`quantum_opt/`)

The [`quantum_opt/inputs/`](quantum_opt/inputs/) folder provides ORCA inputs used for metal–hydroxide geometry optimizations. The corresponding optimized Cartesian coordinates are in [`quantum_opt/opt_xyz/`](quantum_opt/opt_xyz/) as `.xyz` files.

These structures enable comparisons between quantum-chemical metal–oxygen distances and those obtained from classical PMF calculations. Consult the manuscript for the functional, basis set, solvent treatment, and spin-state choices before adapting these calculations.

## 4. Modified 12-6-4 LJ parameter setup

The modified potential introduces pair-specific ion–hydroxide `C4` interactions in addition to the conventional Lennard-Jones and electrostatic terms. Using the parameters requires compatible AMBER topologies and the correct hydroxide–water and metal–hydroxide parameter settings for **the selected water model**.

The provided system-preparation materials include representative LEaP/ParmEd inputs and associated parameter files. When reusing a setup, check that the intended `C4` modifications are present in the final topology; the OPC and TIP3P setups should not be interchanged without the appropriate parameters.

For an implementation walkthrough, see the [AMBER tutorial on the modified 12-6-4 LJ potential using ParmEd](https://ambermd.org/tutorials/advanced/tutorial20/m1264.php).

## 5. Software

The study used the following principal software and methods:

| Software or method | Purpose |
| --- | --- |
| **AMBER 24 / AmberTools** | System construction and molecular dynamics/TI |
| **LEaP and ParmEd** | Topology preparation and nonbonded-parameter modification |
| **PMEMD.cuda** | GPU-accelerated molecular dynamics |
| **WHAM** | Analysis of umbrella-sampling trajectories to obtain PMFs |
| **ORCA** | Quantum-chemical geometry optimization |
| **OPC / TIP3P** | Explicit-solvent water models |


## Citation

If you use these data or the modified parameters, please cite the accompanying article:

> Bhowmik, S.; Li, Z.; Merz, K. M., Jr. *Thermodynamics of Metal-Hydroxide Interactions.* **Journal of Chemical Theory and Computation**. [Year, volume, pages, and DOI to be updated upon publication.]

When appropriate, please also cite AMBER, ORCA, WHAM, and the methodological references in the article.
