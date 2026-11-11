# Risk Register

| Risk | Likelihood / impact | Mitigation | Owner |
|---|---|---|---|
| Overfitting: 12 rows, perfect separation | High / High | No deployment; gather more data; report intervals; prefer the simple rule | Data scientist |
| Leakage from login gap or tickets recorded after the decision to leave | High / High | Confirm feature timestamps; rebuild features as of the snapshot date; drop failing features | Data engineer |
| Bias against low-spend or Basic customers (income proxy) | Medium / High | Compare error rates by plan and spend band; same support and offer quality for Basic | Analyst with ethics reviewer |
| False negative: churner missed | Medium / Medium (about INR 6,000 each) | Choose threshold by expected cost; weekly review of misses | Retention lead |
| False positive: unneeded offer, margin loss, annoyance | Medium / Low (about INR 400 each) | Cap outreach volume; 60-day cool-down between offers | Retention lead |
| Privacy: behavioural data used beyond consented purpose | Medium / High | Confirm consent and DPDP Act basis; pseudonymous IDs; restricted access; retention period | Privacy officer |
| Misuse: score used to cut support or raise prices | Low / High | Written prohibition; audit log of score use | Product owner |
| Concept drift: pricing change or competitor alters behaviour | Medium / Medium | Monitor drift and monthly recall; scheduled retraining | ML engineer |
| Automation bias: staff follow the score without thought | Medium / Medium | Show reasons beside the score; human decision on every flagged account | Retention lead |

## Abstention
No automatic prediction; route to a human when: under 1 month of history; any feature missing or outside the training range; score between 0.35 and 0.65; or the account is in a complaint or dispute.

## Human review
Every flagged customer is reviewed by a retention agent before contact. The agent sees the score and top reasons, may override, and records why. A 5% sample of unflagged customers is reviewed monthly to find missed churners.

## Monitoring
- Weekly: flag rate, feature drift against training, missing-value rate.
- Monthly: recall, precision and calibration once outcomes are known; error rates by plan tier.
- Quarterly: fairness review; confirm the model still beats the 10-day rule.

## Rollback conditions
Revert to the inactivity rule if any occurs: monthly recall falls below the rule's recall; flag rate moves more than 50% from baseline for two weeks; a subgroup's false-positive rate exceeds twice the overall rate; a data or privacy incident; or total expected cost exceeds the rule's. The rule stays in production as the fallback throughout.
