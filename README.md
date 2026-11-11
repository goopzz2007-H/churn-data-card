# ML Problem Framing & Responsible Data Card: Customer Churn

Internship task 02. Decide whether machine learning is justified for a churn decision, and document the data, harms and fallback before any model is trained.

**Verdict:** with 12 labelled rows (5 churners), ML is *not yet justified*. A transparent inactivity rule is the right first step.

## Contents
| File | What it is |
|---|---|
| [`problem_framing_memo.md`](problem_framing_memo.md) | Decision, target, unit, action window, baseline, metrics and error costs |
| [`responsible_data_card.md`](responsible_data_card.md) | Data card using the supplied template |
| [`risk_register.md`](risk_register.md) | Risks, mitigations, owners; abstention, human review, monitoring, rollback |
| [`baseline_notebook.ipynb`](baseline_notebook.ipynb) | Executed baseline notebook (leave-one-out) |
| [`customer-churn-training.csv`](customer-churn-training.csv) | Supplied training data (12 rows) |

## Key findings
- 0 missing cells, 0 duplicates; 7 stayed, 5 churned (41.7%).
- Three features each separate the classes perfectly (`last_login_days`, `support_tickets`, `plan_type = Basic`). That is a leakage or toy-data warning, not a success.
- Leave-one-out: always "stay" catches 0 of 5 churners; the 10-day inactivity rule catches 5 of 5 (threshold fitted on the same rows, so optimistic); logistic regression catches 5 of 5 with 1 false alarm.
- Cost figures (INR 6,000 per missed churner, INR 400 per wasted offer) are stated assumptions, not measured values.

## Conditions to proceed to ML
1. Leakage review of `last_login_days` and `support_tickets` against the snapshot date.
2. At least 1,000 customers with 100 or more churn events.
3. A time-based, customer-disjoint train/test split.
