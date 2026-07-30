# Shelter Long-Stay Screening — Deployment Prototype

A single-file, interactive prototype showing how a trained classification model would
actually be *used* inside a shelter's intake system.

> **What this is in one line:** an animal is entered at intake → a quiet flag surfaces
> for the ones worth an early closer look → staff decide what to do next.

**[▶ Open the Code](https://github.com/chienchien50425-alt/Animal_Flagging_Tool_Prototype/blob/main/shelter_intake_prototype.html)** — download the HTML and open
it in any browser. No install, no server, no build step.  
**[▶ Open the Demonstration Video on Youtube](https://youtu.be/rBSQtaRDyzg)** 

---

## What you're looking at

The prototype mimics a **desktop staff intake screen**, laid out as a queue plus a
detail panel:

- **Left — recent intake.** A plain list of animals. A small amber dot marks the ones
  the model flagged.
- **Right — the record.** Click an animal to see the seven details known at intake and
  a single **flag / no-flag** result.
- **+ New intake.** Enter a hypothetical animal and get a live result as you fill in the
  form. Scored in your browser by the same model that scores the list.

Switch between **Dogs** and **Cats** in the top right; each species has its own model.

---

## The idea: route attention, don't prescribe actions

The model does one honest thing: it **surfaces** an animal.  The value is **triage of attention** and allows staff to intervene early based on the shelter's resources.

---

## The model, briefly

- **Target:** whether an animal's length of stay will exceed **30 days**, predicted **at
  intake**.
- **Separate models per species** (Dog, Cat), gradient-boosted trees.
- **Seven intake features:** intake reason, breed, mixed-breed, sex, age, intake health
  condition, and prior spay/neuter status.

The trained model is **embedded directly in the HTML file** (the tree structure is
serialised into the page), so the flags you see come from the real model rather than a
mock-up.

**→ [Long Stay Prediction Model](https://github.com/chienchien50425-alt/Long-Stay-Prediction-Austin-Animal-Shelter)**

---

## Data note

The animals shown are real records from a public "Austin-style" animal-shelter dataset,
displayed as-is. Full details on the dataset and how it was obtained are documented in the modelling repository linked above.

---

## Repository contents

```
shelter_intake_prototype.html   # the entire prototype — UI + embedded model + sample data
README.md
```

---

*Built as a portfolio piece to demonstrate deployment thinking: taking a trained model
and making a deliberate, honest decision about how it should show up in front of the
people who'd actually use it.*
