
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

The annual FIA inventory samples approximately 10% of the FIA plots in
each state. The 10% of plots are intentionally spread out across the
state. We pulled FIA data for all 50 states for inventory year 2021
(2021 was the most recent year where data was available for every
state), representing roughly 10% of FIA trees, spread fairly evenly
across the country.

FIA internally applies the NSVB framework to estimate biomasses for
their trees. We ran the 2021 subset of trees through the `BiomassNSVB()`
function in BerkeleyForestsAnalytics. For each tree and biomass
variable, we checked that the FIA NSVB estimate and the BFA NSVB
estimate were consistent to 10g (0.01kg). For foliage biomass (which has
much smaller values in general) we checked for consistency to 1g
(0.001kg). Given computer precision and rounding, it is not practical to
get an exact estimate match.

### Main results

The table below shows the main takeaway results. For all biomass
variables, FIA and BFA NSVB estimates matched (by our consistency
criteria described above) for \>99% of the trees.

| total_n_trees | total_wood | total_bark | total_branch | foliage | merch_wood | merch_bark | stump_wood | stump_bark |
|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 23537 | 99.669 | 99.703 | 99.643 | 99.622 | 99.615 | 99.652 | 99.689 | 99.694 |

FIA fuzzes all plot locations and swaps a small subset of plot
locations. Therefore, for a small percent of plots, the publicly
available data assigns a different ecodivision and province than the
private/internal data. The NSVB framework centers around the assignment
of ecodivision. This explains almost all of the small inconsistencies
between the FIA and BFA estimates. Because of the fuzzy and swapping
100% agreement is not possible.

We consulted with FIA personnel on the issue of fuzzing and swapping in
relation to the ecodivision assignments and NSVB biomass estimates.
However, we also sanity checked that the fuzzing and swapping was the
primary culprit by making sure that the non-matching trees were
clustered in a small set of plots.

### Description of files in repository

- **NAME:** description

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
