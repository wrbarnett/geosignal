<img width="256" height="256" alt="image" src="https://github.com/user-attachments/assets/dcbb07dd-e5f5-4d44-8cc3-35ca97bb8df7" />

<img width="512" height="512" alt="Screenshot 2026-04-07 082936" src="https://github.com/user-attachments/assets/c4a0a928-ae65-4cf3-a6bd-95cc9d8d01e6" />

<img width="512" height="512" alt="Screenshot 2026-04-07 083059" src="https://github.com/user-attachments/assets/0e19ce55-36dd-4b44-835c-9ddb2848c99b" />

# 📡 GeoSignal

## Overview

This application implements a geometric distribution–based method to evaluate rare event occurrence over time using **days between events**.

The method is adapted from:

> Assaly R, Barnett WR, Safi F, Khuder S, Macko J. Assessment of ventilator-associated events using the geometric distribution. Am J Infect Control. 2017;45(5):566-568. doi:10.1016/j.ajic.2016.12.004

## Model Description

### Inputs

* Event dates (required)
* Optional grouping (e.g., unit)

Each row represents a **single event occurrence**.

## Mathematical Formulation

### 1\. Days Between Events

k = difference in days between consecutive events

### 2\. Data Value

n = running count of events (1, 2, 3, ...)

### 3\. Mean Days Between Events

x̄ = running mean of k

### 4\. Daily Event Probability

p = (1 / (x̄ + 1)) \* ((n - 1) / n)

### 5\. Interval Probability (Geometric CDF)

P(X ≤ k) = 1 - (1 - p)^k

## Signal Detection

### Threshold

Special cause variation is flagged when:

P(X ≤ k) ≥ 0.99865

### Interpretation

* Below threshold → expected variation
* Above threshold → **special cause variation (true change likely)**

## Current State Evaluation

The model also evaluates the **ongoing event-free period**:

k = days since last event

P(current) = 1 - (1 - p)^k

If this exceeds the threshold, it suggests **sustained improvement beyond chance**.

## Interpretation

This model evaluates:

* Whether a gap between events is statistically unusual
* Whether a prolonged absence of events represents real improvement

It does NOT evaluate:

* Clinical causation
* Severity of events
* Patient-level risk

## Use Cases

* Infection prevention (CLABSI, CAUTI, VAE)
* Falls with harm
* Medication safety events
* Any low-frequency QI metric

## Model Characteristics

* Designed for **rare events**
* Based on **time-between-events analysis**
* Equivalent to a simplified survival/time-to-event framework
* Highly interpretable for operational teams

## Limitations

* Assumes constant daily event probability
* Sensitive to small sample sizes early in the series
* Does not adjust for exposure unless modeled separately
* Not suitable for high-frequency events

## Disclaimer

For quality improvement and surveillance use only.  
Not intended for direct clinical decision-making without validation.
