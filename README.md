# Healthcare Analytics and Patient Journey Analysis (MySQL + Python)

Analysis of a hospital network's patient journey, built on a five-table
relational model in MySQL with an insight and visualization layer in Python
(Google Colab). The project answers a focused set of business questions about
demand, visit mix, cost, insurance coverage, and patient readmissions, and turns
each finding into a recommendation.

## Problem statement

A multi-hospital group across Delhi NCR and Bihar, with rising patient volume,
could not see which operational and financial levers actually mattered. It needed
clarity on how demand has changed, how much billed value goes uncovered by
insurance and through whom, where the money is spent, and how many patients are
readmitted within 30 days.

## Data model

Five interconnected tables, with `encounters` as the central fact table linking
patients, procedures, payers, and organizations. See `ER_Diagram.png`.

| Table | Rows | Role |
|---|---|---|
| patients | 1,000 | Patient demographics |
| encounters | 23,050 | Fact table: every visit, cost, coverage |
| procedures | 37,086 | Procedures performed per encounter |
| payers | 10 | Insurers, government schemes, self-pay |
| organizations | 5 | Hospitals across NCR and Bihar |

Total billed value across the network: 19.61 Cr INR, spanning 2014 to 2024.

## Business questions answered (MySQL)

1. Encounters per year
2. Encounter class mix
3. Share of encounters lasting more than 24 hours
4. Encounters with zero insurance coverage
5. Most frequently performed procedures
6. Highest average-cost procedures
7. Payer with the highest average claim cost
8. Patients readmitted within 30 days

All eight queries are in `analysis_queries.sql`, each with its verified result
in a comment. Every result was confirmed by execution against the database.

## Key findings

- Demand nearly tripled, from 1,200 encounters in 2014 to 3,300 in 2024 (175%
  growth), with a dip to 1,700 in 2020.
- Ambulatory care drives activity at 49.9 percent of all encounters; inpatient is
  only 5.9 percent.
- 8.1 percent of encounters last more than 24 hours.
- 54.4 percent of encounters carry zero insurance coverage, and 55.8 percent of
  billed value (10.94 Cr of 19.61 Cr) is uncovered.
- Leakage is led by Private payers (4.55 Cr) and Self-Pay (4.10 Cr), not by the
  government schemes (2.29 Cr).
- Routine care dominates volume, while the top three procedure types carry 41
  percent of spend, led by CT and MRI imaging and ICU admission.
- Self-Pay carries the highest average claim cost, ahead of ICICI Lombard and CGHS.
- 109 thirty-day readmission events occur across just 90 patients, a small and
  traceable cohort.

## Insight layer (Python, Colab)

`Healthcare_Insights_Colab.ipynb` sits on top of the SQL analysis and converts the
findings into decisions across three levers: demand (encounters per year), revenue
(uncovered amount by payer type), and cost (top procedures by total spend). Each
chart ends in a recommendation. Open in Google Colab, upload encounters.csv,
procedures.csv, and payers.csv, then Run all.

## Recommendations

- Demand: index staffing and outpatient capacity to the ambulatory line, not beds.
- Revenue: point-of-care eligibility checks and Self-Pay collection at discharge,
  targeting the 8.65 Cr leakage in Private and Self-Pay first.
- Cost: imaging utilization review and ICU stay protocols, renegotiate high-volume
  CT and MRI rates.
- Operations: post-discharge follow-up for the 90 high-readmission patients.

## SQL techniques used

Aggregation and grouping, percentage calculation with scalar subqueries,
multi-table joins, conditional aggregation with CASE, and a self-join for the
30-day readmission cohort. Intermediate level, no window functions.

## Repository

- `create_hospital_db.sql` schema, keys, indexes, and bulk-load statements
- `analysis_queries.sql` eight analysis queries with verified results
- `Healthcare_Insights_Colab.ipynb` Python insight and visualization layer
- `patients.csv`, `encounters.csv`, `procedures.csv`, `payers.csv`, `organizations.csv` source data
- `data_dictionary.csv` field definitions
- `ER_Diagram.png` schema diagram

## Data note

The dataset is synthetic and self-generated for analytical demonstration. It is
modelled to reflect realistic Indian hospital and payer-mix patterns (PMJAY,
CGHS, private insurers) and contains no real patient information. Every figure in
this README is computed from the dataset and reproducible by running the queries
or the notebook.

Author: Prachi
