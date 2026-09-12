# Disaster Instability Early Warning Engine

<p align="center">

  <img src="https://img.shields.io/badge/Project-Disaster_Instability_EWS-7B1FA2?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Domain-Disaster_Analytics-0277BD?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-Early_Warning_Not_Prediction-D84315?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Framework-Equilibrium_%26_Instability-283593?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Method-Force_Decomposition-1565C0?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Signal-Instability_Is_Leading-red?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Python-3.10%2B-blue?style=for-the-badge&logo=python" />
  <img src="https://img.shields.io/badge/Streamlit-App-red?style=for-the-badge&logo=streamlit" />
  <img src="https://img.shields.io/badge/ML-Escalation_Risk_Model-purple?style=for-the-badge" />

</p>

> **Hazards become disasters when buffering capacity collapses under accumulated pressure.**
> This project models disaster escalation as a **transition**, not as a one-off event.

Instead of predicting “damage” directly, the engine answers a more actionable question:

> **Is this event’s system equilibrium becoming unstable and which forces are driving it?**

---

## What This Project Is (and is not)

### What it is

A **diagnostic + early-warning** engine that:

* computes an interpretable **Instability Index** (leading signal)
* decomposes each event into **pressure forces vs buffer forces**
* trains an ML model to estimate **Major Disaster Escalation Risk** (P(major))
* provides a Streamlit UI for:

  * single-event diagnosis
  * counterfactual scenario testing
  * cohort / map pressure-field visualization

### What it is not

* Not a “Kaggle damage prediction model”
* Not a black-box catastrophe forecaster
* Not an automated decision maker

This is a **human-centered decision-support system**.

---

## Dataset

Kaggle: **Disaster Events 2025** by **emirhanakku**
[https://www.kaggle.com/datasets/emirhanakku/disaster-events-2025](https://www.kaggle.com/datasets/emirhanakku/disaster-events-2025)

> Note: The dataset is synthetic/constructed for 2025-style disaster analysis.
> This project focuses on **mechanistic interpretation and counterfactual stress testing**, not historical truth claims.

---

## Core Concept: Disaster as an Equilibrium Transition

A “disaster” is not the earthquake, flood, or wildfire by itself.

A disaster is the moment when:

* **pressure increases**
* **buffers fail**
* **systems cannot recover**
* outcomes (loss, displacement, casualties) **materialize**

So we model disaster escalation as:

### 1) Pressure forces (destabilizing)

* **Hazard pressure** (severity / intensity)
* **Exposure pressure** (affected population, density)
* **Response latency pressure** (slower response amplifies damage)
* **Infrastructure fragility pressure** (damage index indicates weak structure)

### 2) Buffer capacity (stabilizing)

* **Aid/response capacity** and the system’s ability to absorb shocks

This creates an instability lens:

> **Instability rises before outcomes fully appear.**
> That’s why it’s a leading signal.

---

## Outputs Produced by the Engine

For every event, the system computes:

### Instability Index

A continuous score: higher = more fragile equilibrium.

### Early-Warning Zone

Quantile-based zones (dataset-relative):

* 🟢 Stable
* 🟡 Fragile
* 🟠 Unstable
* 🔴 Critical

### Force Decomposition

A bar decomposition showing:

* negative bars → pressures pulling the system toward collapse
* positive bars → buffering forces resisting collapse

### ML Escalation Risk (P(major))

A trained classifier estimates:

* **Probability of major escalation**
* Used as a *secondary signal* to confirm or contest the force-based reading

**Important:** ML is not replacing the force model.
ML is an additional layer that learns nonlinear interactions.

---

## System Architecture

### Data pipeline

1. Load raw CSV
2. Clean types + normalize key features
3. Compute engineered “force signals”
4. Save processed dataset (cache)
5. Train ML model on “major disaster” target proxy
6. App loads:

   * processed data
   * trained model
   * produces diagnostics and simulations

### Why two layers (forces + ML)?

Because:

* force model = **explainable mechanism**
* ML model = **pattern learner**
* together = **interpretable + adaptive**

---

## Streamlit App

Run:

```bash
streamlit run app/app.py
```

Tabs:

1. **Event Diagnostic**
2. **Scenario Simulator**
3. **Map / Cohort View**

Each tab is a different “lens” on the same underlying model.

---

# 1) Event Diagnostic

> This view explains why a single event escalates by decomposing it into pressures and buffers.

<img width="1281" height="610" alt="Screenshot 2025-12-15 at 15-28-48 Disaster Instability Early Warning Engine" src="https://github.com/user-attachments/assets/20df8440-415c-408f-9b9b-1d5b81cc6729" />

### What you are seeing (top to bottom)

#### **Event selector**

You choose one event row from the dataset.

This keeps the system grounded in real records:

* event type
* country/region
* date
* zone

#### **Instability Index**

This number answers:

> *How fragile is the event’s equilibrium right now?*

It is not “damage.”
It is a **leading stress indicator**.

#### **Early-Warning Zone**

This converts the continuous instability into human-friendly interpretation:

* **Stable:** buffers dominate
* **Fragile:** stress rising, buffers still holding
* **Unstable:** competing pressures, recovery weak
* **Critical:** collapse likely under small additional shocks

#### **Buffer Capacity**

This represents stabilizing strength.
In screenshot it is high (~0.980), which explains why the event can be “Stable” even if hazards exist.

#### **Observed Loss (USD)**

This is shown as context only:

* it’s an outcome
* it’s lagging
* it’s not the decision signal

#### **ML risk model loaded**

This indicates the system has loaded the trained model successfully and can compute P(major).

#### **ML Escalation Risk**

The probability that this event belongs to the “major escalation” regime.

You’ll notice in screenshot it shows **1.000**, that suggests the model is extremely confident for that data region.
(If you later want, we can calibrate probability output or adjust class definition.)

#### **Force Decomposition**

This is the heart of the framework.

* Each bar represents a force.
* Direction indicates whether it destabilizes or stabilizes.
* Magnitude shows leverage.

**Interpretation rule:**

> If the negative pressures dominate and buffer is weak → instability rises.

This makes the diagnostic view **explainable by design**.

---

# 2) Scenario Simulator (Counterfactual Stress Test)

> This simulator tests what-if interventions on the same event: faster response, aid delivery, reduced exposure, etc.

<img width="1285" height="636" alt="Screenshot 2025-12-15 at 15-30-51 Disaster Instability Early Warning Engine" src="https://github.com/user-attachments/assets/213c61a7-1b2b-4c13-b6fa-adcc23fd85cd" />

### Why this view exists

Most systems predict outcomes *after* interventions.

This simulator evaluates interventions **directly** by asking:

> **Which lever reduces instability the most?**

### Controls explained

#### **Row index**

Selects which event you are stress-testing.

#### **Δ severity_level**

Simulates escalation in hazard intensity.

This tests:

* how sensitive the system is to stronger shocks

#### **Δ response_time_hours**

This is a critical lever.

In disaster systems, response time often behaves like a nonlinear amplifier:

* small delays → large consequences

#### **Aid provided**

This is a discrete buffer toggle:

* Keep / Increase / Decrease (depending on app options)

This is where you test buffer collapse vs reinforcement.

#### **Δ affected_population**

A proxy for exposure magnitude.

#### **Δ infrastructure_damage_index**

A structural fragility adjustment.

### Outputs explained

#### Instability (Before / After)

Shows how intervention shifts equilibrium.

#### Δ Instability

The key number for decision-making.

If Δ is negative:

* intervention improves stability
  If positive:
* scenario makes the system more fragile

#### Zone (After)

This shows whether the event crosses a threshold into a worse regime.

#### ML Risk (Before / After)

This measures how ML “agrees” with the scenario change.

Even if instability shifts slightly, ML may remain saturated (e.g., 1.0).
That’s not a bug, it means ML sees the event still in the same learned regime.

---

# 3) Map / Cohort View

> This view treats all events as a pressure field over geography.

<img width="1273" height="613" alt="Screenshot 2025-12-15 at 15-31-46 Disaster Instability Early Warning Engine" src="https://github.com/user-attachments/assets/cd855bf4-d7ad-4e53-8e42-6675728913bb" />

### Why this view matters

The diagnostic view explains one event.

This view explains the **system shape**:

* clusters
* hotspots
* fragility regimes
* geographic concentration

### How to read the plot

#### Axes: Latitude / Longitude

Each dot = one event’s geo location.

#### Color: Zone

Color indicates the event’s early-warning zone.

This makes hotspots visible:

* concentration of Critical/Unstable
* stable regions with occasional spikes

#### Size: Instability

Size is proportional to instability magnitude.

So the map encodes two signals:

* categorical (zone)
* continuous (instability)

### Filters

You can filter by:

* disaster type(s)
* warning zone(s)
* max points plotted (performance)

This is not just visual, it’s analytic:
you can isolate, for example:

* only floods
* only critical
* only one region cluster

---

## CLI Usage

Prepare processed dataset:

```bash
python -m src.cli prepare-data
```

Train ML model:

```bash
python -m src.cli train
```

---

## Why This System Is Different

Most disaster analytics produce outputs like:

* top affected regions
* predicted losses
* ranked countries

This system produces:

* instability fields
* force decomposition
* counterfactual intervention leverage
* interpretable regime shifts

It’s **action-first**, not leaderboard-first.

---

## Ethics & Proper Use

This tool should be used for:

* research and education
* prototyping decision-support concepts
* studying instability mechanics

It should **not** be used for:

* automated emergency response decisions
* high-stakes policy without validation
* real-world forecasting claims (dataset is synthetic)
