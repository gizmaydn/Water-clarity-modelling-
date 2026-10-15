# Water-clarity-modelling
This repository contains some example code scripts from part of my thesis where I tried to model water clarity as a response of biomanipulation intervention.



## Bayesian Hierarchical Model Data Preparation Pipeline

'thesis_data_preperation_recreated.RMD' file contains data preparation workflow for a Bayesian Hierarchical Model (BHM) structured in JAGS. The pipeline harmonizes multi-frequency limnological monitoring data—aligning irregularly sampled chemical replicates, continuous monthly meteorological/physical averages, and annual whole-lake biomanipulation doses into a unified discrete-time structure.

## Workflow

Long-term ecological time series routinely combine data collected at differing temporal resolutions:

1. Monthly Discrete Time Grid: 
   - A complete-case factorial grid of growing season months (April–October, Months 4–10) across study years.
   - Months missing core continuous physical drivers (`mean_temp` or `mean_depth`) are excluded.
   - Each surviving month receives a unique 1-based index 'slot' and a year mapping 'yr'. 

2. Irregular Observation Mapping:
   - Sample-level observations (Chlorophyll-a, SRP, DIN, Secchi depth, and Suspended Solids) are log-transformed.
   - Replicates within the same month are preserved: each measurement is matched to its corresponding monthly `slot` index (`ix`), allowing JAGS to model within-month observation error without premature aggregation.

3. Covariate Standardization & Priors:
   - Continuous variables are standardized (and log10-transformed where appropriate). So the regression slopes are comparable. 
   - Empirical means and standard deviations are computed and passed into 'jags_data' to serve as data-informed prior centers for latent state equations.

4. Intervention Regimes:
   - Biomanipulation I (Level Shift): Modeled as a centered binary indicator for intervention years.
   - Biomanipulation II (Continuous Dose): Modeled as z-scored growing-season fish removal biomass (kg).
