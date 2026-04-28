# America's Fresh Air Problem Starts on the Farm

<a id="press-release-anchor"></a>

---

## The Hook

Everyone knows highways and smokestacks dirty the air — but in hundreds of American counties, the biggest invisible threat to clean air isn't a factory. It's the farm down the road. New data analysis reveals that counties with the heaviest agricultural footprint consistently breathe dirtier air, and the public health toll has been hiding in plain sight inside two federal datasets that had never been matched together.

---

## Problem Statement

The EPA measures air quality in over 2,000 US counties every year, publishing a public Air Quality Index (AQI) score that tracks how safe the air is to breathe. At the same time, the USDA tracks every farm, field, and feedlot in the country through its Census of Agriculture. These two federal data streams sit in separate silos — and that separation has cost us.

Agriculture is the single largest contributor to fine particulate matter (PM2.5) pollution in the United States. Ammonia released from fertilizer application and livestock waste drifts into the atmosphere and reacts with sulfur and nitrogen compounds to form microscopic particles that lodge deep in human lungs. Estimated to cause roughly 17,000 premature deaths per year, this pollution disproportionately affects rural communities — the very communities growing our food — yet agricultural air pollution receives a fraction of the regulatory attention given to cars and power plants.

The result: rural public health officials have no simple, accessible tool to identify which counties are most at risk, and policymakers have lacked the evidence needed to act.

---

## Solution Description

This project merges the EPA's 2022 Annual AQI by County dataset with the USDA's 2022 Census of Agriculture to build the first county-level map linking agricultural intensity directly to air quality outcomes across the United States.

Each county is characterized by three agricultural intensity metrics that any rural health official can understand:

- **Harvested cropland acreage** — a proxy for fertilizer application
- **Livestock inventory** (cattle + hogs) — the primary source of ammonia emissions
- **Number of farms** — a measure of agricultural density

These three metrics are combined into a composite Agricultural Intensity Score and compared against each county's median annual AQI. A linear regression model trained on the merged dataset reveals that **agricultural intensity is a statistically significant predictor of poor air quality**, even after controlling for population size. Counties scoring in the top quartile for agricultural intensity have a measurably higher median AQI than low-intensity counties — meaning their residents experience more days of unhealthy air every year.

The finding is immediately actionable: the same two free, public federal datasets updated every year could power a dashboard that flags high-risk counties before an air quality crisis emerges.

---

## Chart

The chart below plots every US county with sufficient AQI monitoring data (≥ 50 measurement days in 2022). Each point represents one county. The horizontal axis shows its Agricultural Intensity Score; the vertical axis shows its Median Annual AQI. The red trend line and AQI threshold references (50 = Moderate, 100 = Unhealthy) make the pattern unmistakable: as agricultural intensity rises, so does the air quality index.

![Agricultural Intensity vs. Median AQI — US Counties 2022](ag_aqi_chart.png)

*Figure: US County-Level Agricultural Intensity Score vs. Median Annual AQI (2022). Each point is one county. Color encodes AQI severity (green = good, red = unhealthy). The crimson trend line shows a positive correlation: higher agricultural intensity predicts worse air quality. Orange and red dashed lines mark the EPA's "Moderate" and "Unhealthy" AQI thresholds, respectively. Source: EPA Annual AQI by County 2022; USDA Census of Agriculture 2022.*

