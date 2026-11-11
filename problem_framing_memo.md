# ML Problem-Framing Memo

## Decision
Which active subscribers should the retention team contact this week with a save offer or support call? ML is only useful if it ranks customers better than a simple rule, so limited outreach capacity goes to those most likely to leave.

## Prediction target
`churned` = 1 if the customer cancels or lets the subscription lapse within 30 days after the snapshot date, else 0. The file does not state the window, so this is a proposed definition to confirm with the data owner.

## Unit of observation
One row per customer (`customer_id`) at one snapshot date. Features use only information available on that date.

## Action window
Score weekly; the team acts within 5 working days, leaving about three weeks of the 30-day window for the offer to work. A score that arrives after cancellation has no value.

## Non-ML baseline
Flag any customer with `last_login_days >= 10`, reviewed by a person. It is explainable, free to run, and is the bar a model must beat on held-out, later-in-time data.

## Metrics and error costs (assumptions, need finance sign-off)
| Error | What happens | Assumed cost |
|---|---|---|
| False negative | Churner not flagged, leaves | about INR 6,000 (about six months of average spend, INR 916) |
| False positive | Loyal customer gets an unneeded offer or call | about INR 400, plus mild annoyance |

A miss costs about 15 times a wasted offer, so the business metric is **total expected cost** at the chosen threshold versus doing nothing. Model metrics: recall of churners (priority), precision at outreach capacity, PR-AUC, Brier score and calibration curve, on a time-based hold-out. Accuracy alone is not used: always predicting "stay" already scores 58%.

## Verdict
Do not train for production on 12 rows. Use the rule now; revisit ML when the conditions in the README are met.
