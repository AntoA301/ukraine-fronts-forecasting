# INSY 674 — Individual Assignments

Individual assignments for INSY 674 (McGill Master of Management in Analytics, 2026). Both assignments use ACLED conflict event data on the Russia–Ukraine war.

| Assignment | Topic |
|---|---|
| [Assignment 1](assignment-1-fatality-forecast/) | Forecasting next-week fatalities on each front, and whether a model can anticipate a sudden offensive (Kursk, Aug 2024) |

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

The notebooks were run on Python 3.14.

## Citations

- Raleigh, C., Kishi, R. & Linke, A. (2023). Political instability patterns are obscured by conflict dataset scope conditions, sources, and coding choices. *Humanities and Social Sciences Communications*, 10, 74. Data: ACLED, [acleddata.com](https://acleddata.com/).
- Géron, A. (2022). *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow* (3rd ed.). O'Reilly. Code: [github.com/ageron/handson-ml3](https://github.com/ageron/handson-ml3).
- Natural Earth borders: [github.com/nvkelso/natural-earth-vector](https://github.com/nvkelso/natural-earth-vector).

## Author

Anto Abousefian — [github.com/AntoA301](https://github.com/AntoA301)
