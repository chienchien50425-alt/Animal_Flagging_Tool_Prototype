# Shelter Long-Stay Screening — Deployment Prototype

**[▶ Open the Code](https://github.com/chienchien50425-alt/Animal_Flagging_Tool_Prototype/blob/main/shelter_intake_prototype.html)** — download the HTML and open
it in any browser. No install, no server, no build step.  
**[▶ Open the Demonstration Video on YouTube](https://youtu.be/xw7UkQaCvNw)**

> An animal is entered at intake, and a quiet flag surfaces the ones worth an early closer look.

## The problem

Shelter capacity is set by the animals that get stuck. Some stay for months, and on the day
they arrive nobody can tell which ones. This prototype shows how a trained model would
actually be **used** at the intake desk — not a notebook, not a dashboard, but the screen a
staff member would be looking at.

## What you see

- **Left — recent intake.** The last 14 days of 2024 arrivals. An amber dot marks the flagged ones.
- **Right — the record.** The intake-day details, a single flag / no-flag result, and what moved the score.
- **+ New intake.** Enter a hypothetical animal and get a live result as you type.

Dogs and cats have their own models — switch at the top right. Nothing on screen is
pre-computed: the models are embedded in the page and every number is calculated in your browser.

## The model

Predicts whether a stay will exceed **30 days**, from what is known on intake day only:
intake reason, breed, mixed-breed, sex, age, health, prior spay/neuter, intake month and
year — plus body size for dogs. Trained on 2013–2023 and tested on **2024, a year it never saw**.

| | AUC | precision | recall | F1 | flagged | n (2024) |
|---|---|---|---|---|---|---|
| Dog | 0.747 | 0.517 | 0.511 | 0.514 | 29.2% | 5,168 |
| Cat | 0.758 | 0.513 | 0.560 | 0.536 | 32.5% | 6,039 |

## The flag is a rank, not a fixed score

An animal is flagged when its score lands in the **top 30% of everything scored in the
previous 90 days**. Capacity sets that 30%, not a metric target.

This matters more than it sounds. Predicted probabilities drift upward year over year, so a
threshold frozen at one value quietly selects a different share of animals every season.
Across 2024 the cut-off actually travelled:

| | lowest | median | highest |
|---|---|---|---|
| Dog | 0.767 | 0.797 | 0.849 |
| Cat | **0.388** | 0.597 | **0.695** |

The cat range is the argument by itself — a constant anywhere in it would be wrong for most
of the year. So the page carries the 2024 score distribution and recomputes the percentile
for whichever intake date you are on. Change the date on the form and watch the cut-off move.

## "What moved this score"

Per-field **TreeSHAP** contributions in log-odds, summed from the model's encoded columns
back onto the fields a person actually filled in.

**Intake year is listed separately, on purpose.** It is usually the single largest term, but
every animal arriving that year carries the same amount — it is the model's recency
baseline, not a fact about the animal in front of you.

## How a model fits inside a static HTML file

The trained pipeline is an sklearn `ColumnTransformer` → `XGBClassifier`. Three parts were
hand-ported to JavaScript, each checked against the Python original on all **11,207**
held-out 2024 records:

| ported | verified |
|---|---|
| fitted encoders + booster | max &#124;Δ probability&#124; **2.4e-7** (dog) / **3.3e-7** (cat), **0 flag mismatches** |
| trailing-window cut-off | max &#124;Δ threshold&#124; **8e-7** vs `np.quantile` |
| TreeSHAP | max &#124;Δ&#124; **1.4e-4**, and the decomposition closes exactly (7.5e-7) |

## Limits, stated rather than hidden

- **Reference scores stop at 2024** — a later date reuses the last full 90-day window, and the page says so.
- **An intake year past 2024 saturates** at the trees' last split; the model has no rule for a year it never saw.
- **Intake-day information only** — no behavioural assessment, no photo. Deliberate, and it likely caps AUC at 0.75–0.80.
- **The queue is a 14-day slice**, so its flagged share need not sit at 30%; the full-year figures are in the table above.

## Data

Real records from the public Austin Animal Center dataset, shown as-is. The dataset,
the modelling and the operating-point choice are documented in the modelling repository:

**→ [Long Stay Prediction Model](https://github.com/chienchien50425-alt/Long-Stay-Prediction-Austin-Animal-Shelter)**

---

*A portfolio piece about deployment thinking: taking a trained model and making a
deliberate, honest decision about how it shows up in front of the people who would use it.*
