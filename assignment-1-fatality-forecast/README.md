# Assignment 1 — Forecasting weekly fatalities on the Russia–Ukraine fronts

**Question:** Can a model trained on past conflict activity forecast deaths on a front that opens suddenly (the August 2024 Ukrainian incursion into Kursk) as well as on long-running fronts like Donetsk and Kherson?

Adapted from the end-to-end workflow in Aurélien Géron's *Hands-On Machine Learning* (chapter 2), applied to a regression problem on ACLED conflict event data.

## Approach

- **Data:** ACLED events for Ukraine and Russia, 1 Aug 2023 – 19 Sep 2025 (169,578 events, 31 columns), filtered to 11 frontline regions (8 Ukrainian oblasts + Kursk, Belgorod, Bryansk).
- **Unit of analysis:** one row per region-week. About 83% of single events report zero deaths, so events are summed into weekly totals (events, battles, explosions, civilian-targeting events, fatalities).
- **Target:** next week's total fatalities in each region.
- **Features:** this week's activity counts, fatality lags (1, 2, 4 weeks), 4-week rolling averages, and the region (one-hot encoded). Count features and the target are log-transformed.
- **Split:** chronological instead of random. The earliest 80% of weeks are for training and the most recent 20% (Apr–Sep 2025) are for testing, so the model never learns from the future.
- **Model:** Random Forest regressor in a scikit-learn pipeline, evaluated with `TimeSeriesSplit` cross-validation against a naive "next week = this week" baseline.

## Results

| | RMSE | MAE |
|---|---|---|
| Random Forest — train | 52.0 | 16.4 |
| Random Forest — time-series CV (mean) | 126.6 | — |
| Random Forest — test | 72.1 | 30.3 |
| Naive baseline — test | 85.5 | 32.3 |

- The forest beats the naive baseline on the test set (about 16% lower RMSE), mostly by cutting the large misses.
- It works as a smoothed persistence forecast. It tracks steady fronts well (Zaporizhia, Kherson, Donetsk) but reacts late to sudden changes in either direction.
- **Kursk:** in the first week of the incursion, the model forecast 3 deaths when 136 occurred. It caught up a week later but then stayed about one step behind, and it kept over-forecasting for around six weeks after the fighting eased in November 2024. Kharkiv (May 2024) and Sumy showed the same weakness, so the problem is sudden change in general, not Kursk itself.
- **Answer to the question:** no. A model trained on past event data cannot anticipate a sudden offensive, but it is still useful as a short-term guide on active fronts.

## Running the notebook

1. Install the dependencies listed in the repo's `requirements.txt`.
2. Export the data from ACLED (see the main README) and save it as `acled.csv` in this folder.
3. The map cell downloads Natural Earth border files (`countries.geojson`, `regions.geojson`) the first time it runs.
