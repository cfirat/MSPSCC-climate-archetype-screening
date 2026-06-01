# Climate-Dependent Performance of Solar-Powered Spray Cooling Canopies

This repository contains the data-processing workflow, processed results, clustering outputs, sensitivity-analysis outputs, and figure-generation files associated with the manuscript:

**Climate-dependent performance of solar-powered spray cooling canopies: A climate-archetype-zone framework for pre-deployment feasibility assessment**

Authors:
Coskun Firat, Istanbul Technical University, Energy Institute, Istanbul, Türkiye
Asfaw Beyene, San Diego State University, Department of Mechanical Engineering, San Diego, CA, USA

## Overview

This study evaluates the pre-deployment feasibility of modular solar-powered spray cooling canopies for outdoor public spaces. The analysis uses hourly Typical Meteorological Year extended (TMYx) weather files for 110 locations across Türkiye. The framework combines photovoltaic (PV) energy estimation, adaptive misting control, electrical-load calculation, water-use estimation, climate-archetype-zone classification, and sensitivity analysis.
The objective is not to predict site-specific pedestrian thermal comfort directly, but to provide a reproducible system-level screening method for identifying where PV-powered evaporative spray cooling is climatically suitable, where water or PV sizing may become limiting, and where alternative outdoor heat-mitigation strategies may be more appropriate.

## Repository Structure


MSPSCC-climate-archetype-screening/
├── README.md
├── LICENSE
├── requirements.txt
├── notebooks/
│   └── MSPSCC_TMYx_analysis.ipynb
├── data_processed/
│   ├── results_by_city.csv
│   ├── results_by_city_with_clusters.csv
│   ├── results_by_cluster.csv
│   ├── final_cluster_verification.csv
│   ├── cluster_validation_metrics.csv
│   ├── cluster_k4_k5_k6_summary.csv
│   ├── cluster_robustness_checks.csv
│   ├── sensitivity_by_city.csv
│   ├── sensitivity_by_archetype.csv
│   ├── sensitivity_overall.csv
│   ├── sensitivity_baseline_check.csv
│   └── pv_area_scaling_by_archetype.csv
├── station_list/
│   └── onebuilding_epw_station_list.csv
├── figures/
│   ├── Figure_1_Station_Map.png
│   ├── fig_cluster_validation_metrics.png
│   ├── fig2_Ta_vs_RH_archetypes.png
│   ├── fig3_PV_vs_Load.png
│   ├── fig4_autonomy_by_archetype.png
│   ├── fig5_water_vs_mist_hours.png
│   ├── fig_sensitivity_tornado_misting_hours.png
│   ├── fig_sensitivity_tornado_water_use.png
│   └── fig_sensitivity_tornado_autonomy.png
└── supplementary/
    ├── Supplementary_Table_S1.csv
    └── Interactive_Station_Map.html


Folder names may be adjusted depending on the final archive structure.

## Climate Data Source

The raw weather files used in this study are publicly available from the OneBuilding repository:

**Climate.OneBuilding.org**

The analysis uses TMYx weather files representing a typical meteorological year constructed from 2009–2023 source data. These files are not continuous 15-year hourly simulations. The raw EPW/TMYx files are not redistributed in this repository due to file-size considerations and because they are already publicly available from OneBuilding.

The station identifiers and file names needed to reproduce the analysis are provided in:

station_list/onebuilding_epw_station_list.csv


or, depending on the final repository structure:

supplementary/Supplementary_Table_S1.csv


## Main Outputs

### City-level performance results

data_processed/results_by_city_with_clusters.csv


This file contains city-level climate descriptors, system-performance indicators, cluster labels, and climate archetype-zone assignments.

Key columns include:

* `city`
* `epw_file`
* `latitude`
* `longitude`
* `altitude_m`
* `summer_mean_Ta_C`
* `summer_p95_Ta_C`
* `summer_mean_RH_pct`
* `summer_mean_wind_ms`
* `summer_mean_GHI_Wm2`
* `summer_precip_metric`
* `summer_mist_hours_h`
* `summer_water_use_L_per_module`
* `summer_pv_energy_Wh_per_m2pv`
* `summer_load_energy_Wh_per_module`
* `summer_autonomy_ratio`
* `cluster`
* `archetype`

### Archetype-level performance results

data_processed/results_by_cluster.csv

This file summarizes mean climate and performance metrics for each climate archetype zone.

The final archetype zones are:

* Hot–Dry Inland
* Humid Black Sea
* Humid Mediterranean Coastal
* Semi-Arid Plateau
* Temperate / Windy Coastal
* Temperate Humid Transitional

### Clustering validation outputs

data_processed/cluster_validation_metrics.csv
data_processed/cluster_k4_k5_k6_summary.csv
data_processed/cluster_robustness_checks.csv


These files report the clustering-validation and robustness analyses, including:

* within-cluster sum of squares / inertia,
* silhouette coefficient,
* Calinski–Harabasz index,
* Davies–Bouldin index,
* comparison of (k=4), (k=5), and (k=6),
* comparison between precipitation-inclusive and non-precipitation clustering,
* comparison between k-means and Ward hierarchical clustering.

### Sensitivity-analysis outputs

data_processed/sensitivity_by_city.csv
data_processed/sensitivity_by_archetype.csv
data_processed/sensitivity_overall.csv
data_processed/sensitivity_baseline_check.csv
data_processed/pv_area_scaling_by_archetype.csv


The sensitivity analysis uses single-factor perturbations of key model parameters:

* (T_{set}) ±10%
* (RH_{max}) ±10%
* (G_{min}) ±10%
* water flow rate ±10%
* pump power ±10%
* active PV area ±10%
* additional PV-area cases of 2 m² and 3 m² per module

The `sensitivity_baseline_check.csv` file confirms that the sensitivity-analysis baseline reproduces the main results before perturbation.

## Method Summary

The workflow performs the following steps:

1. Read hourly EPW/TMYx weather files for 110 Türkiye locations.
2. Filter the data to summer daytime conditions:

   * June–August
   * 10:00–18:00 local time
3. Estimate PV generation using a PVWatts-type model and SAPM cell-temperature correction.
4. Apply adaptive misting control based on:

   * air temperature,
   * relative humidity,
   * plane-of-array irradiance.
5. Calculate:

   * feasible misting duration,
   * water consumption,
   * electrical load,
   * PV generation,
   * PV-to-load autonomy ratio.
6. Construct climate descriptors for each location.
7. Classify locations into summer climate archetype zones using k-means clustering.
8. Validate clustering using WCSS, silhouette, Calinski–Harabasz, and Davies–Bouldin metrics.
9. Perform robustness checks against alternative feature sets and Ward hierarchical clustering.
10. Conduct single-factor sensitivity analysis for thresholds and system parameters.
11. Export processed data tables and manuscript figures.

## Software Requirements

The analysis was performed in Python. The main packages are:

python
numpy
pandas
matplotlib
scikit-learn
scipy
pvlib
tqdm
folium


A minimal `requirements.txt` file may include:

numpy
pandas
matplotlib
scikit-learn
scipy
pvlib
tqdm
folium


Exact package versions may differ depending on the computing environment. The notebook was developed for execution in Google Colab, but it can also be run locally if the EPW/TMYx files are available and the input/output paths are updated.

## How to Reproduce the Analysis

1. Clone this repository:

```bash
git clone https://github.com/<username>/<repository-name>.git
cd <repository-name>
```

2. Install the required packages:

```bash
pip install -r requirements.txt
```

3. Download the raw TMYx EPW files from Climate.OneBuilding.org using the station list provided in the repository.

4. Place the raw EPW files in the expected input directory, or update the `ROOT_DIR` path in the notebook.

5. Run the notebook:

notebooks/MSPSCC_TMYx_analysis.ipynb


6. The notebook will generate processed CSV outputs and figures in the specified output folder.

## Important Notes

* The raw EPW/TMYx files are not redistributed in this repository.
* The TMYx files represent a typical meteorological year constructed from 2009–2023 source data, not a continuous 15-year weather record.
* The analysis is a system-level pre-deployment screening framework.
* The model does not resolve pedestrian-level airflow, droplet dispersion, local mean radiant temperature, or UTCI.
* PV-to-load autonomy is a seasonal cumulative energy-balance indicator and does not guarantee hour-by-hour off-grid operation.
* Rainwater harvesting is estimated at seasonal scale and does not resolve storage routing or rainfall timing relative to misting demand.

## Citation

If you use this repository, please cite the associated manuscript:

Firat, C.; Beyene, A. Climate-dependent performance of solar-powered spray cooling canopies: A climate-archetype-zone framework for pre-deployment feasibility assessment. Manuscript under review.


After publication, replace this placeholder with the final journal citation and DOI.

## License
MIT License for code
CC BY 4.0 for processed data and documentation


## Contact

For questions about the analysis or processed outputs, please contact:

Coskun Firat
Energy Institute, Istanbul Technical University
Istanbul, Türkiye
Email: [coskun.firat@itu.edu.tr](mailto:coskun.firat@itu.edu.tr)
