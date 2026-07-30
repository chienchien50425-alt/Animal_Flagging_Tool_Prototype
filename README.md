# Shelter Long-Stay Screening — Deployment Prototype

A single-file, interactive prototype showing how a trained classification model would
actually be *used* inside a shelter's intake system — not a metrics report, but a
working picture of the workflow and where the model's value sits.

> **What this is in one line:** an animal is entered at intake → a quiet flag surfaces
> for the ones worth an early closer look → staff decide what to do next. The tool does
> exactly that one thing.

**[▶ Open the prototype](./shelter_intake_prototype.html)** — download the HTML and open
it in any browser. No install, no server, no build step.

---

## What you're looking at

The prototype mimics a **desktop staff intake screen**, laid out as a queue plus a
detail panel:

- **Left — recent intake.** A plain list of animals. A small amber dot marks the ones
  the model flagged. It's a subtle marker, not an alarm, and flagged animals are *not*
  jumped to the top — the list stays in intake order.
- **Right — the record.** Click an animal to see the seven details known at intake and
  a single **flag / no-flag** result.
- **+ New intake.** Enter a hypothetical animal and get a live result as you fill in the
  form — scored in your browser by the same model that scores the list.

Switch between **Dogs** and **Cats** in the top right; each species has its own model.

---

## The idea: route attention, don't prescribe actions

The model does one honest thing — it **surfaces** an animal. It deliberately stops
there. It doesn't recommend "arrange a foster" or "schedule a photo," because the right
action depends entirely on a given shelter's resources, and this tool has no way of
knowing those. What to do with a flag is left to the people who actually know their own
context. The value is **triage of attention**, not a replacement for operational
judgment.

That restraint is the whole design stance: be genuinely useful at the one thing the
model can support, and don't pretend to know the rest.

---

## Reading the flag correctly

The result is intentionally **binary** — flag or no-flag — with no percentage shown
anywhere. The underlying probabilities aren't calibrated, so a precise-looking "72%"
would invite false confidence. A flag means *the model crossed its real decision
threshold*, nothing more.

Two things the interface is careful about, and you should be too:

- **A flag is a prompt to look, not a prediction.** It means "worth an early closer
  look," not "this animal will be held long."
- **No-flag does not mean "safe to ignore."** The model is tuned to catch most true
  long-stays, which means it casts a wide net — a large share of animals get flagged,
  and the ones that *don't* are simply the cases the model was more confident about. It
  can still be wrong. Normal judgment still applies.

---

## The model, briefly

- **Target:** whether an animal's length of stay will exceed **30 days**, predicted **at
  intake**.
- **Separate models per species** (Dog, Cat), gradient-boosted trees.
- **Seven intake features:** intake reason, breed, mixed-breed, sex, age, intake health
  condition, and prior spay/neuter status. (Colour and intake month were dropped during
  feature selection.)
- **Honest framing:** this is a **modest ranker**, not a precise per-animal oracle. Its
  accuracy is close to the ceiling *for these seven coarse features* — the real drivers
  of long stays (post-intake behaviour, medical progress, exposure) simply aren't in the
  data. Better predictions would need richer inputs, not a fancier model.

The trained model is **embedded directly in the HTML file** (the tree structure is
serialised into the page), so the flags you see come from the real model rather than a
mock-up. It was validated during development against the original training run before
being ported into the browser. Full data sourcing, cleaning, EDA, and modelling live in
the companion repository:

**→ `<link-to-your-modeling-repo>`**

---

## Data note

The animals shown are real records from a public "Austin-style" animal-shelter dataset,
displayed as-is. Because the source is public, names and found-location text are shown
verbatim; note that a found-location can point to a real address, so in a production
deployment you'd likely de-identify that field in the display layer. Full details on the
dataset and how it was obtained are documented in the modelling repository linked above.

---

## Design notes

The interface is deliberately plain — it's meant to read like real internal software, so
attention goes to the workflow and not to visual flourish. Colour is spent in exactly
one place: the flag. Everything else stays quiet on purpose. Honesty is carried by these
design choices (binary flag, no false-precision percentages, careful no-flag wording,
human-in-the-loop framing) rather than by disclaimer text bolted on top.

---

## Running it

There is nothing to run. Download `shelter_intake_prototype.html` and open it in a
browser — everything (UI, data, and model) is contained in that one file. It works
offline.

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

<!--
  Placeholders to fill in before publishing:
    <link-to-your-modeling-repo>   → your existing modeling repo URL
  Optional: add your name / license / a screenshot near the top.
-->
