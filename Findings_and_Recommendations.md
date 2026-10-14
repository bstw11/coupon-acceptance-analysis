# Findings and Recommendations: Coffee House Coupons

**Assignment:** Required Assignment 5.1, Will the Customer Accept the Coupon? (Independent Investigation)
**Data:** UCI "In-Vehicle Coupon Recommendation" survey (Amazon Mechanical Turk), 12,684 responses. 3,996 of them are Coffee House coupons.
**Notebook:** `prompt-8.ipynb`, section "Independent Investigation"

---

## 1. Problem Statement

Coffee House coupons are the most common coupon in the data (3,996 of 12,684, 31.5%), but only **49.9%** are accepted (1,995 of 3,996). The rate for all coupons is 56.8%.

**Question:** Which driver traits and trip conditions separate drivers who **accept** a Coffee House coupon (Y = 1) from drivers who **reject** it (Y = 0)? Which segment should be targeted to raise acceptance well above 50%?

**Data preparation:** `car` was dropped (99.1% missing). Small gaps (0.8% to 1.7%) in the visit-frequency columns were filled with each column's mode. A readable `distance` column was built from `toCoupon_GEQ15min` and `toCoupon_GEQ25min`. `toCoupon_GEQ5min` was ignored because it equals 1 in every row.

---

## 2. Visualizations: Accepted vs. Rejected

Each bar chart in the notebook shows the acceptance rate for each group. A dashed line marks the overall Coffee House rate (49.9%), so bars above the line are groups that accept more often than average. Bars below the line are groups that reject more often.

| Notebook figure | What it compares | What it shows |
|---|---|---|
| Driver attributes (4 panels) | Coffee House visits per month, age, income, marital status | Visit frequency separates accepters from rejecters far more than any demographic does. |
| Trip context (6 panels) | Time of day, destination, passenger, expiration, weather, driving time | Time, destination, passenger and expiration each move acceptance by 15 to 20 points. Weather barely moves it. |
| Combined segments | Habit plus context segments vs. the 49.9% baseline | The best segment (81.4%) is more than four times the rate of drivers who never visit coffee houses (18.9%). |

---

## 3. Descriptive Statistics

### Driver traits

| Factor | Highest acceptance | Lowest acceptance |
|---|---|---|
| Coffee House visits per month | 4–8: **68.6%**, more than 8: 65.8%, 1–3: 64.8% | never: **18.9%** (n = 927) |
| Age | under 21: 69.7% (n = 155) | 50+: 42.0% |
| Income | $87.5K–$100K: 55.7% | $75K–$87.5K: 29.7% (no consistent trend) |
| Marital status | Single: 51.6% | Widowed: 35.1% (n = 37 only) |

### Trip and coupon context

| Factor | Highest acceptance | Lowest acceptance |
|---|---|---|
| Time of day | 10AM: **64.1%**, 2PM: 54.8% | 6PM: 41.3%, 10PM: 42.4% |
| Destination | No urgent place: **58.1%** | Home: 36.2% |
| Passenger | Friend(s): 59.7%, Partner: 57.0% | Alone: 43.8% |
| Expiration | 1 day: **58.4%** | 2 hours: 43.2% |
| Driving time to venue | Under 15 min: 54.8% | 25+ min: 34.5% |
| Weather | Rainy: 52.2%, Sunny: 50.4% | Snowy: 43.2% |

### Combined segments

"Regular" means the driver visits a coffee house at least once a month.

| Segment | Accepted | Acceptance |
|---|---|---|
| Regular + no urgent place + 10AM/2PM + 1-day coupon | 302 of 371 | **81.4%** |
| Regular + no urgent place + 10AM/2PM | 628 of 811 | **77.4%** |
| Regular, any context | 1,269 of 1,922 | 66.0% |
| Not regular (never or less than once a month) | 726 of 2,074 | 35.0% |
| Never visits coffee houses | 175 of 927 | **18.9%** |

**Interpretation:** The coffee habit sets the ceiling. Regular visitors accept about two-thirds of coupons, and non-visitors accept about one in five. Trip context then adds or removes 10 to 20 points within each group. Demographics matter much less than either.

---

## 4. Inferential Statistics

**Method:** Chi-square tests of independence (each factor vs. accept/reject) and two-proportion z-tests with 95% confidence intervals for the key comparisons. α = 0.05. Cramér's V measures effect size: about 0.1 is small, 0.3 is moderate and 0.5 is large. Counts come from the notebook's group sizes and rates.

### Chi-square tests (each factor vs. acceptance)

| Factor | χ² (df) | p-value | Cramér's V | Conclusion |
|---|---|---|---|---|
| Coffee House visits per month | 560.4 (4) | < 0.001 | **0.37** | Significant, **moderate**, the strongest factor |
| Destination | 137.9 (2) | < 0.001 | 0.19 | Significant, small to moderate |
| Time of day | 129.4 (4) | < 0.001 | 0.18 | Significant, small to moderate |
| Expiration | 91.1 (1) | < 0.001 | 0.15 | Significant, small |
| Passenger | 87.2 (3) | < 0.001 | 0.15 | Significant, small |
| Income | 60.4 (8) | < 0.001 | 0.12 | Significant but small, with no consistent direction |
| Driving time to venue | 52.6 (2) | < 0.001 | 0.12 | Significant, small |
| Age | 43.9 (7) | < 0.001 | 0.11 | Significant, small |
| Weather | 6.2 (2) | 0.045 | 0.04 | Barely significant, negligible effect |
| Marital status | 6.8 (4) | 0.15 | 0.04 | **Not significant** |

### Two-proportion z-tests

| Comparison | Rates | Difference (95% CI) | p-value |
|---|---|---|---|
| Regular vs. not regular | 66.0% vs. 35.0% | **+31.0 pts** [28.1, 34.0] | < 0.001 |
| Best segment vs. all other coffee coupons | 77.4% vs. 42.9% | **+34.5 pts** [31.2, 37.9] | < 0.001 |
| 10AM/2PM vs. 6PM/10PM | 59.7% vs. 41.5% | +18.2 pts [14.7, 21.7] | < 0.001 |
| No urgent place vs. home/work | 58.1% vs. 40.4% | +17.7 pts [14.7, 20.8] | < 0.001 |
| Friends/partner vs. alone | 59.2% vs. 43.8% | +15.4 pts [12.2, 18.6] | < 0.001 |
| 1-day vs. 2-hour expiration | 58.4% vs. 43.2% | +15.2 pts [12.1, 18.3] | < 0.001 |
| Under 26 vs. 26 and older | 55.0% vs. 48.1% | +6.9 pts [3.3, 10.4] | < 0.001 |

**Interpretation:**
- The large gaps are not due to chance. Even the lowest bound of the habit gap is 28 points.
- With nearly 4,000 responses, small effects still reach significance, so effect size matters more than the p-value. Income and age are statistically significant but practically weak. Weather is negligible. Marital status has no reliable effect.
- These are associations from survey answers, not causal effects. Several factors overlap. For example, "no urgent place" trips happen more often at midday and with friends, so their separate effects need a multivariable model.

---

## 5. Key Findings (Actionable Items)

> **1. Coffee habit is the best predictor.** Drivers who visit a coffee house at least monthly accept 66% of coupons. Drivers who never go accept 19%.
>
> **2. The right moment adds 15 to 20 points.** Daytime sends (10AM/2PM), no urgent destination, and friends or a partner in the car each raise acceptance.
>
> **3. 2-hour expirations cost 15 points.** One-day coupons are accepted 58.4% of the time vs. 43.2% for 2-hour coupons.
>
> **4. Evening sends and non-coffee drinkers are mostly wasted.** 6PM/10PM sends are accepted only about 41% of the time, and drivers who never visit coffee houses accept 19%.
>
> **5. Demographics alone are weak targeting tools.** Income and age have small effects, and marital status and weather have none that matter.
>
> **6. Combining the factors works.** Regular visitors on a non-urgent daytime trip with a 1-day coupon accept **81.4%** of coupons, vs. the 49.9% average.

---

## 6. Recommendations

1. **Target by habit first.** Send Coffee House coupons mainly to drivers who visit coffee houses at least once a month. Stop or sharply limit sends to drivers who never go.
2. **Send in the daytime.** Prioritize 10AM, then 2PM. Cut 6PM and 10PM sends.
3. **Send on non-urgent trips.** Favor drivers with no urgent destination. Reduce sends on the way home.
4. **Make 1-day expiration the default** instead of 2 hours.
5. **Favor social trips.** Prioritize drivers with friends or a partner in the car.
6. **Promote nearby shops.** Favor coffee houses under 15 minutes away. Venues 25+ minutes away are accepted only 34.5% of the time.
7. **Do not target on income, marital status or weather.**

**Expected impact:** Within the targeted segment, acceptance would rise from about 50% to about 77–81%. Sends to low-likelihood drivers would also drop.

---

## 7. Next Steps

1. **Build a predictive model.** Start with logistic regression, then try a tree-based model such as random forest or gradient boosting. This estimates each factor's effect while controlling for the others and scores individual drivers.
2. **Test interactions.** Check, for example, habit × time of day and passenger × destination to confirm which factors matter independently.
3. **Run A/B tests** on the rules above (1-day vs. 2-hour expiration, daytime vs. evening sends) to confirm causal effects. Survey answers may not match real behavior.
4. **Repeat the analysis** for Carry out & Take away, Restaurant (< $20) and Restaurant ($20–50) coupons.
5. **Collect better data:** actual redemption records, exact driving distance in minutes, and the missing `car` information.
