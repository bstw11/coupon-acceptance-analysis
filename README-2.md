# coupon-acceptance-analysis
Required Assignment 5.1: Will the Customer Accept the Coupon?

**Jupyter notebook:** [prompt-8.ipynb](https://github.com/bstw11/coupon-acceptance-analysis/blob/main/prompt-8.ipynb)
**Full write-up:** [Findings_and_Recommendations.md](https://github.com/bstw11/coupon-acceptance-analysis/blob/main/Findings_and_Recommendations.md)
**Data:** [coupons.csv](https://github.com/bstw11/coupon-acceptance-analysis/blob/main/coupons.csv) (raw), [coupons_clean.csv](https://github.com/bstw11/coupon-acceptance-analysis/blob/main/coupons_clean.csv) (cleaned)

## Overview

This project looks at what makes a driver accept a coupon sent to their phone for a nearby business. The data is the UCI "In-Vehicle Coupon Recommendation" survey: 12,684 responses collected on Amazon Mechanical Turk.

A coupon counts as accepted (Y = 1) if the driver would use it right away or before it expires. Across all coupons, **56.8%** were accepted.

The independent investigation focuses on **Coffee House coupons** (3,996 responses), the most common coupon type.

**Data cleaning:** `car` was dropped (99.1% missing). Small gaps (under 2%) in the visit-frequency columns were filled with each column's most common value.

## Summary of Findings

### Coffee House coupons: 49.9% accepted
- **Coffee habit is the strongest predictor.** Drivers who visit coffee houses at least monthly accept **66.0%** of coupons. Drivers who never go accept **18.9%**. The chi-square test gives χ² = 560, p < 0.001, Cramér's V = 0.37.
- **Trip context adds 15 to 20 points.** Each of these raises acceptance, and every difference is significant (p < 0.001):
  - time of day: 64.1% at 10AM, 41–42% in the evening
  - destination: 58.1% with no urgent place to be, 36.2% on the way home
  - passengers: 59.2% with friends or a partner, 43.8% alone
  - expiration: 58.4% for a 1-day coupon, 43.2% for a 2-hour coupon
- **Shorter drives help.** Acceptance is 54.8% for coffee houses under 15 minutes away and 34.5% for those 25+ minutes away.
- **Demographics are weak predictors.** Age and income have small effects. Weather is negligible, and marital status is not significant (p = 0.15).
- **Best segment: 81.4% acceptance.** These are regular coffee drinkers with no urgent destination, at 10AM or 2PM, with a 1-day coupon.

## Recommendations
1. **Target by habit first.** Send Coffee House coupons to drivers who already visit coffee houses at least monthly.
2. **Send in the daytime** (10AM/2PM) on non-urgent trips, and cut evening sends.
3. **Make 1-day expiration the default** instead of 2 hours.
4. **Favor nearby coffee houses**, under 15 minutes away.
5. **Don't target on income, marital status or weather.**

## Next Steps
- Build a predictive model, such as logistic regression and then gradient boosting, to separate overlapping factors and score individual drivers.
- Run A/B tests on the expiration and timing rules to confirm cause and effect.
- Extend the analysis to the restaurant and carry-out coupon types.
