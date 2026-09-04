# Shelter Long-Stay Screening — Deployment Prototype

> An animal is entered at intake, and a quiet flag surfaces the ones worth an early closer look.  

**[▶ Open the Code](https://github.com/chienchien50425-alt/Animal_Flagging_Tool_Prototype/blob/main/shelter_intake_prototype.html)** — download the HTML and open
it in any browser.  
**[▶ Open the Demonstration Video on YouTube](https://youtu.be/xw7UkQaCvNw)**

## The problem

Animal shelter resources are always limited, and capacity is ultimately dictated by the animals that get stuck. Some stay for months, and on the day they arrive, nobody can tell which ones. This prototype shows how a trained model would actually be used at the intake desk, flagging the top 30% of high-risk, long-stay animals.

## What you see

- **Left — recent intake.** The last 14 days of 2024 arrivals. An amber dot marks the flagged ones.
- **Right — the record.** The intake-day details, a single flag / no-flag result, and what moved the score.
- **+ New intake.** Enter a hypothetical animal and get a live result as you type.

## The model

Predicts whether a stay will exceed **30 days**, from what is known on intake day only:
intake reason, breed, mixed-breed, sex, age, health, prior spay/neuter, intake month and
year, plus body size for dogs. Trained on 2013–2023 and tested on **2024**.

| | AUC | precision | recall | F1 | flagged | n (2024) |
|---|---|---|---|---|---|---|
| Dog | 0.747 | 0.517 | 0.511 | 0.514 | 29.2% | 5,168 |
| Cat | 0.758 | 0.513 | 0.560 | 0.536 | 32.5% | 6,039 |


## What the HTML file is made of

| # | Component | Language | What it does |
|---|---|---|---|
| 1 | **Stylesheet** | CSS | Design tokens, two-pane flexbox frame, CSS-grid rows, amber flag colour |
| 2 | **Page shell** | HTML | Top bar, species switch, empty queue pane, empty detail pane |
| 3 | **Embedded payload** | JSON | Both models, their encoders, the reference scores, the sample queue |
| 4 | **Scoring engine** | JavaScript | Feature encoder, tree walk, rolling cut-off, TreeSHAP |
| 5 | **UI layer** | JavaScript | Draws the queue, record and form; handles every click |


## Data

Real records from the public Austin Animal Center dataset, shown as-is. The dataset,
the modelling are documented in the modelling repository:

**→ [Long Stay Prediction Model](https://github.com/chienchien50425-alt/Long-Stay-Prediction-Austin-Animal-Shelter)**

