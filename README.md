# DS 4320 Project 2: Agricultural Intensity vs. Air Quality 

## Executive Summary: 
This repository contains the materials necessary for running DS 4320 Project 2. The project addresses the problem of air pollution running rampant, by using county-level AQI and USDA farming metrics to predict the air pollution of counties. 

**Name:** Yuthi Madireddy

**Computing ID:** hva4zb

**DOI:** https://doi.org/10.5281/zenodo.19362629

**PRESS RELEASE LINK:**  [Read the Press Release](/press_release.md)

**PIPELINE LINK:** [View Pipeline](./pipeline/pipeline.ipynb)


**License:** MIT — see [LICENSE](./LICENSE)

**DOI:** https://doi.org/10.5281/zenodo.19866741

---


## Problem Definition:

**General Problem:** Predicting Air Quality
**Refined Specific Problem Statement:** Can county level AQI (air quality index) scores be predicted using agricultural metrics such as harvested cropland acreage, livestock count, number of farms, etc.?

**Motivation:** Recent conversations regarding air pollution has been mainly centered on the emissions contributed by cars, factories, and power plants. However, agriculture is the leading cause of fine matter pollution in the U.S. emissions from fertilizers and pesticides along with livestock waste have result in dangerous aerosol particles to form in our atmosphere. Understanding if regional level metrics at the county level can help provide policymakers, health officials and rural residents with a practical tool for identifying at risk communities before air quality grows into a larger crisis. 

**Rationale for Refinement:** The general problem of agricultural air pollution encompasses a large range of variables such as crop types, fertilizer usage, weather patterns, soil conditions, proximity to urban areas and more. Accounting for all these factors would require specialized datasets that may be more difficult to interpret for someone like me who doesn't have vast knowledge of agricultural metrics. Therefore, I am choosing to narrow the focus to three main metrics available in the USDA Census of Agricultural such as cropland acres, livestock counts, and farm density. Pairing these metrics with EPA's county level annual AQI summaries can then allow for prediction tasks that minimize confounding temporal mismatches as well. If these simpler metrics demonstrate a meaningful correlation with AQI, then the finding can be deemed to be statistically demonstrable. 


**Press Release Headline:**


---

## Domain Exposition:

### Terminology:
| Term | Definition |
|---|---|
| AQI (Air Quality Index) | EPA's 0–500 scale measuring daily air pollution; values above 100 are unhealthy for sensitive groups |
| PM2.5 | Fine particulate matter smaller than 2.5 microns; the primary pollutant linked to agricultural emissions and the most harmful to human lungs |
| Ammonia (NH3) | Gas released by livestock waste and fertilizer; reacts atmospherically to form PM2.5 particles |
| FIPS Code | Federal Information Processing Standards code — a unique numeric identifier for every US county, used to join datasets |
| Harvested cropland | Acres of farmland that produced a crop in a given year; a proxy for fertilizer application intensity |
| Livestock inventory | Count of cattle, hogs, and poultry on farms in a county; a proxy for manure-based ammonia emissions |
| Census of Agriculture | USDA survey conducted every 5 years counting every US farm and ranch at the county level |
| Annual AQI summary | EPA dataset reporting median, max, and category-day counts for AQI per county per year |
| Median AQI | The midpoint AQI value across all measured days in a year for a county — the primary outcome variable in this project |

### Project Domain:
This project sits at the intersection of environmental data science and agricultural policy. The domain involves understanding how land use decisions , specifically the scale and intensity of farming operations translate into measurable atmospheric outcomes at the county level. Agriculture contributes to air quality degradation through two primary pathways: ammonia volatilization from nitrogen fertilizers and animal waste, which reacts with atmospheric nitrogen and sulfur compounds to form secondary PM2.5 particles; and dust and smoke from tillage, harvesting, and field burning. The EPA monitors air quality through a national network of ground sensors and publishes annual county-level AQI summaries, while the USDA independently tracks agricultural activity through its Census of Agriculture. These two federal data streams have rarely been analyzed together at scale, creating an opportunity to surface connections that existing siloed reporting misses. The audience for this work is primarily rural public health officials, state environmental agencies, and agricultural policymakers who need accessible evidence to justify intervention.


### Background Reading (OneDrive Link):

### Table Summary:
| # | Title | Brief Description | Link |
|---|---|---|---|
| 1 | Agriculture is the Leading Cause of Pollution-Related Deaths in the US (MIT News) | Reports that agricultural ammonia emissions cause more US deaths from air pollution than any other sector, including transportation | [news.mit.edu](https://news.mit.edu/2021/agriculture-leading-cause-pollution-deaths-us-0510) |
| 2 | EPA AirData Download Files Documentation | Official EPA documentation describing the structure and fields of the annual AQI by county dataset used in this project | [aqs.epa.gov](https://aqs.epa.gov/aqsweb/airdata/download_files.html) |
| 3 | 2022 Census of Agriculture Highlights (USDA NASS) | Overview of the 2022 agricultural census methodology, coverage, and available county-level variables | [nass.usda.gov](https://www.nass.usda.gov/Publications/AgCensus/2022/) |
| 4 | Livestock Farming and Air Quality: What the Research Shows (American Lung Association) | Plain-language summary of how livestock operations contribute to particulate matter and ozone formation at regional scales | [lung.org](https://www.lung.org/clean-air/outdoors/who-is-at-risk/agriculture) |
| 5 | Ammonia Emissions from Agriculture and Their Contribution to PM2.5 (EPA) | Technical EPA report quantifying ammonia's role in secondary aerosol formation and identifying agricultural source regions | [epa.gov](https://www.epa.gov/air-research/research-air-quality-agricultural-operations) |

---


## Data Creation

### Data Acquisition Process:

This project uses two publicly available federal datasets, both covering the year 2022.

**EPA Annual AQI by County (2022):** The U.S. Environmental Protection Agency maintains a national network of groundlevel air quality monitoring stations. Each station records daily readings that are summarized into the Air Quality Index (AQI). At the end of each calendar year, the EPA publishes a countylevel summary CSV that aggregates these readings per county, reporting the number of days in each AQI category (good, moderate, unhealthy, etc.), the median AQI, the maximum AQI, and the number of days with valid measurements. The 2022 dataset was downloaded directly from the EPA AirData bulk download portal at `https://aqs.epa.gov/aqsweb/airdata/annual_aqi_by_county_2022.zip`. Not every US county has a monitoring station, with coverage skewed toward more populated or industrialized counties meaning that rural agricultural counties may be underrepresented.

**USDA Census of Agriculture (2017):** County-level cattle inventory data was downloaded directly from the USDA NASS QuickStats web interface as a CSV file (Census 2017, County geographic level, TOTAL domain) and saved to `data/usda_nass_data.csv`. The 2017 Census is used as it has full county-level coverage in QuickStats.

### Code Table

| File / Script | Description | Location |
|---|---|---|
| `data_acquisition.ipynb` (Cell: EPA Download) | Downloads the EPA Annual AQI by County 2022 ZIP file from the AirData portal and reads it into a pandas DataFrame | Cell 23 in this notebook |
| `data_acquisition.ipynb` (Cell: USDA API — Cropland) | Queries the USDA NASS QuickStats API for county-level harvested cropland acres (2022) using `AG LAND, CROPLAND - ACRES` | Cell 24 in this notebook |
| `data_acquisition.ipynb` (Cell: USDA API — Livestock) | Queries the USDA NASS QuickStats API for county-level cattle inventory and hog inventory (2022) | To be added in analysis notebook |
| `data_merge.ipynb` | Joins the EPA AQI DataFrame and USDA agricultural metrics on 5-digit county FIPS code; drops counties with fewer than 50 AQI measurement days | Analysis notebook |
| `eda_visualization.ipynb` | Produces scatter plots, histograms, and the agricultural intensity score; calculates Pearson correlation coefficients | Cell 25 in this notebook |


### Rationale:
**Choosing county as the unit of analysis:** FIPS-coded counties are the finest spatial resolution at which both EPA AQI summaries and USDA Census data are publicly available without special data use agreements. Census tracts or zip codes would be preferable for precision but are not available for both datasets simultaneously.

**50-day AQI measurement threshold:** Counties with fewer than 50 days of valid AQI readings were dropped because sparse coverage produces median AQI estimates with very high variance, a county with 10 readings could appear extremely clean or extremely polluted purely by chance. The 50day cutoff balances data retention (keeping ~80% of monitored counties) against estimate reliability. This threshold is a judgment call that introduces uncertainty: counties just below the cutoff are excluded even if their data is adequate.

**Agricultural intensity score construction:** Combining cropland acres, livestock count, and farm density into a single composite score makes the visualization interpretable but obscures which component drives any observed correlation. Individual feature regressions are retained in the analysis notebook so readers can attribute effects to specific agricultural activities rather than the composite.

**Year selection (2017):** USDA cattle inventory uses the 2017 Census of Agriculture, as 2017 has complete county-level 
coverage in QuickStats. This introduces a 5-year temporal gap which is noted as a limitation since agricultural land use patterns are relatively stable at the county level 
over 5-year periods, partially mitigating this mismatch.

**Excluding wildfire counties:** Counties in the western US that experienced major wildfires in 2022 (identified by >30 "Very Unhealthy" or "Hazardous" AQI days) will be flagged as potential outliers. Including them without annotation would overstate the relationship between agriculture and AQI in high fire risk states like California and Oregon.


### Bias Identification:
**Geographic coverage bias:** EPA monitoring stations are not uniformly distributed. Urban and suburban counties are far more likely to have operating monitors than rural agricultural counties. Since the project specifically investigates agricultural counties, many of the most relevant counties may be absent from the EPA dataset entirely, creating a systematic gap precisely where the signal of interest is strongest.

**Survivorship / reporting bias (USDA):** The USDA Census relies on farmers self-reporting. Large commercial operations are reliably captured, but smaller farms and subsistence operations may be undercounted, particularly in communities with language barriers or distrust of government data collection.

**Temporal mismatch:** The USDA Census of Agriculture is conducted every five years. The 2022 Census captures a single snapshot in time, while AQI is measured daily. Agricultural land use may have shifted between the census snapshot and the specific days when air quality readings were taken.

**Confounding variable exclusion:** By restricting features to cropland, livestock, and farm count, the model excludes major known drivers of AQI such as wildfire smoke, traffic emissions, industrial point sources, and weather. Counties with high agricultural intensity that also have urban cores or significant industry may show inflated AQI values that are only partially attributable to agriculture.


### Bias Mitigation:
**Coverage gap transparency:** All counties dropped due to fewer than 50 AQI measurement days are flagged and reported separately. A choropleth map of monitoring coverage will be included to make the spatial gap visible to readers, rather than silently omitting rural counties.

**USDA undercounting:** The analysis uses only the largest and most reliably reported commodity categories (total cropland acres, cattle inventory, hog inventory) which are mandatory reporting items for operations above USDA size thresholds. Sub-threshold farm counts will be treated as approximate and flagged in uncertainty fields.

**Temporal mismatch quantification:** Because both datasets nominally cover 2022, temporal bias is bounded to intra-year variation. Sensitivity analysis will compare results using 2017 USDA Census data against 2017 EPA AQI to confirm the direction of effects is consistent across census years.

**Confounding:** The model will explicitly acknowledge that correlation does not imply causation and will include population density as a control variable to partially isolate agricultural signal from urban industrial signal. Residuals will be mapped spatially to identify systematic confounding by geography.



## Metadata:

### Implicit Schema Guidelines:
Because this project uses MongoDB to store merged countylevel records, the following guidelines govern document structure for all team members:

**Top-level required fields (every document must contain):**
- `fips` (string, 5-digit zero-padded): Primary identifier used for all joins. Must be stored as a string to preserve leading zeros (e.g., `"01001"` not `1001`).
- `state` (string): Full state name as returned by the EPA dataset (e.g., `"Alabama"`).
- `county` (string): County name without the word "County" appended.
- `year` (integer): Data year, always `2022` for this project.

**Nested sub-document — `aqi` (required if EPA data is available):**
- `median` (float): Median AQI across all measurement days.
- `max` (integer): Maximum AQI recorded in the year.
- `days_measured` (integer): Number of days with valid AQI readings.
- `days_good`, `days_moderate`, `days_unhealthy_sensitive`, `days_unhealthy`, `days_very_unhealthy`, `days_hazardous` (integers): Day counts per AQI category.

**Nested sub-document — `agriculture` (required if USDA data is available):**
- `cattle_inventory` (integer): Head of cattle; `null` if not reported.


**Optional derived fields (added during analysis, not ingestion):**
- `ag_intensity_score` (float): Computed composite score — not stored in raw collection, only in the `county_merged` collection.
- `outlier_wildfire` (boolean): `true` if the county was flagged as a wildfire outlier.

**Schema versioning:** All documents must include a `schema_version` field set to `"1.0"` during initial ingestion. Any structural changes require incrementing to `"1.1"`, `"2.0"`, etc., with a team-wide announcement.

### Data Summary:
| Collection | Source | Records (approx.) | Key Fields | Notes |
|---|---|---|---|---|
| `aqi_county_2022` | EPA AirData | ~3,100 | `fips`, `median_aqi`, `max_aqi`, `days_measured`, 6 day-count fields | Not all US counties have monitors; ~800 counties absent |
| `usda_cattle_2017` | USDA NASS QuickStats API | ~2,700 | `fips`, `cattle_inventory` | Sparse in western states with low livestock density |
| `county_merged` | Joined (EPA + USDA) | ~2,400 | All above fields + `ag_intensity_score`, `outlier_wildfire` | Inner join on FIPS; counties missing either source are excluded |

**Total unique US counties:** 3,143  
**Counties with EPA AQI data:** ~3,100 (monitored)  
**Counties with USDA agricultural data:** ~2,900 (reported)  
**Counties in merged analysis dataset:** ~2,400  
**Counties dropped (< 50 AQI days):** ~250  
**Counties flagged as wildfire outliers:** ~80 (primarily CA, OR, WA)

### Data Dictionary:
| Feature | Min | Max | Mean | Std Dev | Uncertainty Source | Uncertainty Level |
|---|---|---|---|---|---|---|
| `aqi.median` | ~12.0 | ~130.0 | ~38.5 | ~12.0 | Sensor calibration drift (~±2 AQI units); spatial interpolation error for counties with few monitors | **Low–Medium** |
| `aqi.max` | 15 | 500 | ~75 | ~45 | Single-day extremes are highly sensitive to wildfire events and sensor anomalies; high variance in fire-prone states | **High** (fire-prone counties) |
| `aqi.days_measured` | 1 | 366 | ~280 | ~80 | Gaps from sensor downtime, maintenance, or QA rejection; fewer days = less reliable median | **Medium** (inversely with coverage) |**Medium** |
| `agriculture.cattle_inventory` | 0 | ~1,200,000 | ~28,000 | ~65,000 | Self-reported census data; undercounting likely for small-scale operations; zero values may be true zeros or suppressed data | **Medium–High** | **High** (skewed) |**Low–Medium** |
| `ag_intensity_score` | 0.0 | 1.0 | ~0.18 | ~0.15 | Propagates uncertainty from all three input variables; normalization method (min-max) is sensitive to outliers | **Medium** |

**Propagated uncertainty:**  
The merged analysis dataset inherits uncertainty from both source datasets. The most consequential/serious uncertainty is the **non-random missingness** of EPA monitors: counties without monitoring stations are not missing at random. They tend to be rural, lower income, and more agricultural, meaning the counties most relevant to this hypothesis are disproportionately absent. Any aggregate statistics (means, correlations) computed from the merged dataset should be interpreted as representative of monitored agricultural counties, not all agricultural counties in the US.





