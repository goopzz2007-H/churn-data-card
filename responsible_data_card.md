# Responsible Data Card

## Dataset purpose
**May support:** prioritising retention outreach; learning which behaviours precede cancellation.
**Must not:** deny service, change prices, restrict support, or penalise any individual customer. A churn score is a priority signal, not a judgement about a person.

## Provenance and permission
Supplied as "Small labeled dataset for responsible baseline modeling" by the internship programme. How it was collected, whether it is real or synthetic, the snapshot date and the licence are **not documented**. IDs look pseudonymous (C001 to C012); there are no names or contact details. Before real use, confirm: customer consent for analytics, the legal basis (India's DPDP Act 2023 for Indian customers), the permitted purpose, and the retention period. Until then, use is limited to this training exercise.

## Population and representation
12 customers: 5 Basic, 4 Standard, 3 Pro. Tenure 1 to 30 months, so customers with more than 30 months are absent. Region, age, gender and device are not recorded, so representation across them cannot be checked. With 12 rows no subgroup estimate is reliable. The sampling method is unknown and a 42% churn rate is likely higher than a typical subscriber base, suggesting an unrepresentative sample.

## Features and target
| Column | Meaning | Concern |
|---|---|---|
| `customer_id` | Identifier | Exclude from modelling; linkable |
| `tenure_months` | Months since signup | Account age, not a person's age |
| `support_tickets` | Tickets raised (period undefined) | Possible leakage if counted after the cancellation request |
| `monthly_spend_inr` | Monthly spend in rupees | Tied to plan; proxy for income |
| `last_login_days` | Days since last login | **Highest leakage risk:** inactivity may result from the decision to leave |
| `plan_type` | Basic / Standard / Pro | All churners are Basic; may proxy income |
| `churned` | Target, 1 = churned | Window and definition not stated |

No directly sensitive attributes (gender, religion, caste, health, location) are present. Sensitive proxies are indirect (spend and plan can reflect income). Adding location, language or age later requires a fairness review first.

## Quality checks
- Missingness: 0 of 84 cells.
- Duplicates: 0 rows, 0 IDs.
- Outliers: none apparent; maxima tenure 30, tickets 5, spend 1,499, inactivity 30 days (looks capped).
- Class balance: 7 stayed, 5 churned (41.7%).
- Suspicious separation: login gap, ticket count and Basic plan each split the classes perfectly. Treat as toy data or leakage.
- Train/test separation: none exists. A random split of 12 rows is meaningless; future splits must be by time and by customer.

## Risks and safeguards
See [`risk_register.md`](risk_register.md).

## Intended evaluation (fixed before training)
- **Baseline:** always "stay"; the 10-day inactivity rule.
- **Performance:** recall, precision at capacity, PR-AUC, expected cost at the chosen threshold.
- **Calibration:** Brier score and reliability curve.
- **Fairness:** recall and false-positive rate by plan tier and spend band; review any gap above 10 percentage points.
- **Error analysis:** manual review of every false negative; errors by tenure band and plan.
- **Uncertainty:** confidence intervals; under 100 churn events they will be too wide to support deployment.
