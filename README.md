# Ukraine Fronts Forecasting

Machine learning and causal inference on ACLED conflict event data from the Russia–Ukraine war (Aug 2023 – Sep 2025). Individual assignments for INSY 674, McGill Master of Management in Analytics (2026).

| Assignment | Method | Question | Finding |
|---|---|---|---|
| [1 — Fatality forecast](assignment-1-fatality-forecast/) | Random Forest regression with time-series CV | Can a model forecast weekly deaths on a front that opens suddenly (Kursk, Aug 2024)? | It tracks steady fronts well but can't anticipate sudden escalation. In the first week of Kursk it forecast 3 deaths; 136 occurred. |
| [2 — Drones vs. artillery](assignment-2-drones-vs-artillery/) | Causal inference (CausalML meta-learners, DoWhy) | Do drone/air strikes cause more deaths per strike than artillery? | No. Drone strikes cause about 0.17–0.19 fewer deaths per strike (about 55% fewer). Strikes in Kursk during the incursion were deadlier than similar strikes elsewhere. |

## Data

The notebooks use an export from the [Armed Conflict Location & Event Data project (ACLED)](https://acleddata.com/). The raw data is **not included in this repository**. To reproduce the analysis:

1. Create a free account at [acleddata.com](https://acleddata.com/) and open the data export tool.
2. Export all events for **Ukraine** and **Russia** from **1 August 2023 to 19 September 2025**.
3. Save the file as `acled.csv` in the folder of the notebook you want to run.

Fatalities in ACLED are conservative, total reported deaths per event across all parties. They are not broken down by side.

## Setup

```bash
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate
pip install -r requirements.txt
```

Assignment 1 was run on Python 3.14 and Assignment 2 on Python 3.13 (CausalML 0.18, DoWhy 0.14).

## Citations

- Raleigh, C., Kishi, R. & Linke, A. (2023). Political instability patterns are obscured by conflict dataset scope conditions, sources, and coding choices. *Humanities and Social Sciences Communications*, 10, 74. Data: ACLED, [acleddata.com](https://acleddata.com/).
- Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly. Code: [github.com/ageron/handson-ml3](https://github.com/ageron/handson-ml3).
- Chen, H., Harinen, T., Lee, J.-Y., Yung, M. & Zhao, Z. (2020). CausalML: Python Package for Causal Machine Learning. arXiv:2002.11631. Code: [github.com/uber/causalml](https://github.com/uber/causalml).
- Sharma, A. & Kiciman, E. (2020). DoWhy: An End-to-End Library for Causal Inference. arXiv:2011.04216. Code: [github.com/py-why/dowhy](https://github.com/py-why/dowhy).
- Natural Earth borders: [github.com/nvkelso/natural-earth-vector](https://github.com/nvkelso/natural-earth-vector).

## Author

Anto Abousefian — [github.com/AntoA301](https://github.com/AntoA301)
