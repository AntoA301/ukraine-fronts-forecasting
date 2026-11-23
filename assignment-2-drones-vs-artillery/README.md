# Assignment 2 — Are drones deadlier than artillery? A causal inference study

**Question:** Do air/drone strikes cause more fatalities per strike than shelling/artillery/missile attacks on the Russia–Ukraine front, holding the front, time period and attacking side constant?

**Hypothesis:** No. Despite the media narrative that drones have revolutionized the war, artillery is still the deadlier weapon per strike.

## Approach

- **Data:** the same ACLED export as Assignment 1, filtered to the 11 frontline regions and the two remote-violence sub-event types. That leaves 107,869 strikes: 57,763 drone/air strikes (treatment) and 50,106 shelling/artillery/missile attacks (control).
- **Outcome:** fatalities per strike.
- **Confounders:** oblast (weapon choice and lethality both vary by front), month (drone use grew over the war), and attacking side (Russia vs. Ukraine).
- **Deliberately excluded:** civilian targeting and exact coordinates, which partly depend on the weapon chosen, and the attacking unit, because ACLED codes almost every drone strike to an Air Force unit, so the unit nearly duplicates the treatment.
- **Methods:**
  - **CausalML:** S-learner (`LRSRegressor`) and T-learner (`XGBTRegressor`) for the average effect, per-strike effects (CATE) by oblast, and feature importance for effect heterogeneity.
  - **DoWhy:** a causal graph, backdoor identification, linear regression, propensity score weighting and matching, and three refutation tests (random common cause, placebo treatment, data subset).
- **Second treatment:** whether a strike happened in Kursk vs. on other fronts, estimated both over the full period and after the incursion began (6 Aug 2024), and split by attacking side.

## Results

| Estimate (drone vs. artillery) | Effect on deaths per strike |
|---|---|
| Naive difference in means | −0.157 |
| CausalML S-learner (LRSRegressor) | −0.174 (95% CI −0.191 to −0.156) |
| CausalML T-learner (XGBTRegressor) | −0.187 (95% CI −0.204 to −0.171) |
| DoWhy linear regression | −0.174 (95% CI −0.192 to −0.155) |
| DoWhy propensity score weighting | −0.193 |
| DoWhy propensity score matching | −0.286 |

- **Hypothesis supported:** every method finds that drone/air strikes cause fewer deaths per strike than artillery, about 55% fewer relative to artillery's average of 0.324.
- **Confounding:** the gap grew once oblast, month and attacker were controlled for. This is because drones were used more where strikes tend to be deadlier.
- **Robustness:** the estimate passed all three DoWhy refutation tests. When the treatment labels were shuffled (placebo), the effect dropped to about zero.
- **Where it holds:** the effect is consistently negative on the Kharkiv, Zaporizhia, Dnipropetrovsk and Kherson fronts and close to zero in the quiet border regions.
- **Kursk:** after the incursion began, a strike in Kursk caused **+0.383** more deaths than a similar strike elsewhere (95% CI 0.339–0.427). Kursk was deadlier for both Russian and Ukrainian strikes.

## Limitations

- The estimate is only causal if oblast, month and attacking side are the main confounders. Target type (vehicles vs. infantry positions) isn't recorded by ACLED and partly depends on the weapon, so the result is the effect of the weapon together with how it is typically used.
- ACLED fatality counts are conservative, and reporting may differ across fronts and between sides. Most strikes kill no one, so a few mass-casualty events drive the averages.
- Location can't be assigned like a weapon, so the Kursk result describes how deadly conditions were there, not the effect of an intervention.

## Running the notebook

1. Install the dependencies listed in the repo's `requirements.txt`.
2. Export the data from ACLED (see the main README) and save it as `acled.csv` in this folder.
3. The map cells download Natural Earth border files the first time they run.

Built following the CausalML meta-learner and feature-interpretation examples and DoWhy's "simple example" (common-causes interface).
