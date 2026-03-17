
# NSVB framework validation

This repository provides a validation of the `BiomassNSVB()` function in
our R package
[BerkeleyForestsAnalytics](https://github.com/kearutherford/BerkeleyForestsAnalytics?tab=readme-ov-file#nsvb-bio)
(BFA). Specifically, we compare our implementation of the national-scale
volume and biomass (NSVB) framework (Westfall et al. 2024) for
estimating tree biomass with FIA’s implementation of NSVB.

### FIA vs BFA names

| FIA name          | BFA name     |
|:------------------|:-------------|
| DRYBIO_STEM       | total_wood   |
| DRYBIO_STEM_BARK  | total_bark   |
| DRYBIO_BRANCH     | total_branch |
| DRYBIO_BOLE       | merch_wood   |
| DRYBIO_BOLE_BARK  | merch_bark   |
| DRYBIO_STUMP      | stump_wood   |
| DRYBIO_STUMP_BARK | stump_bark   |
| DRYBIO_FOLIAGE    | foliage      |

### General workflow

The annual FIA inventory samples ~10-20% of the FIA plots in each state.
The ~10-20% of plots are intentionally spread out across the state. We
pulled FIA data for the continental US states for inventory year 2021
(2021 was the most recent year where data were available for every
state), representing ~10-20% of FIA trees, spread fairly evenly across
the country.

FIA internally applies the NSVB framework to estimate various biomass
components for each tree. We ran the 2021 subset of trees through the
`BiomassNSVB()` function in BerkeleyForestsAnalytics. For each tree and
biomass component, we verified that the FIA NSVB estimate and the BFA
NSVB estimate were consistent to within 10 g (0.01 kg). For foliage
biomass, whose values are generally much smaller, we sued a tighter
tolerance of 1 g (0.001 kg). Because of computer precision limits and
minor rounding differences, obtaining exact numerical agreement is not
practical. The specified tolerances reflect an appropriate and
operationally meaningful standard of consistency.

### Main results

The table below shows the main takeaway results. For all biomass
variables, FIA and BFA NSVB estimates matched (by our consistency
criteria described above) for ~99% of the trees.

| total_n | merch_stump_n | total_wood | total_bark | total_branch | foliage | merch_wood | merch_bark | stump_wood | stump_bark |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 494376 | 404758 | 99.077 | 99.122 | 99.303 | 99.68 | 98.958 | 99.008 | 99.582 | 99.627 |

FIA fuzzes all publicly released plot locations and swaps a small subset
of plot locations. As a result, for a small percentage of plots, the
publicly available data has a different ecodivision assigned than the
internal FIA data. The NSVB framework relies heavily on ecodivision
assignment. Thus, the fuzzing and swapping of plot locations makes it
impossible to achieve 100% agreement between FIA NSVB estimates (which
use the ecodivisions associated with the internal plot locations) and
BFA NSVB estimates (which use the ecodivisions associated with the
publicly released plot locations). We found that almost all non-matching
trees are concentrated within a small subset of plots, supporting that
the fuzzing and swapping of plot locations is the primary source of
discrepancy, as expected.

### Description of files in repository

- **all_state_validation.Rmd:** workflow for validating biomass
  estimates from all continental US states.
- **ca_validation.Rmd:** workflow for validating biomass estimates from
  California.
- **data folder:**
  - bfa_state_data: used in the all_state_validation.Rmd workflow
  - ca_fia_data: CA data pulled from the FIA DataMart
  - match_results.csv: main takeaway results from the
    all_state_validation.Rmd. These results are presented above in the
    “Main results” section of the README.

### References

Westfall, James A.; Coulston, John W.; Gray, Andrew N.; Shaw, John D.;
Radtke, Philip J.; Walker, David M.; Weiskittel, Aaron R.; MacFarlane,
David W.; Affleck, David L.R.; Zhao, Dehai; Temesgen, Hailemariam;
Poudel, Krishna P.; Frank, Jereme M.; Prisley, Stephen P.; Wang,
Yingfang; Sánchez Meador, Andrew J.; Auty, David; Domke, Grant M. 2024.
A national-scale tree volume, biomass, and carbon modeling system for
the United States. Gen. Tech. Rep. WO-104. Washington, DC: U.S.
Department of Agriculture, Forest Service. 37
p. <https://doi.org/10.2737/WO-GTR-104>.
