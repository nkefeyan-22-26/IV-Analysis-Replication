# Audit 02: Deconstructing Statistical Lies

## Overview
This audit exposes three common statistical deceptions used in tech and finance: **Latency Skew**, **False Positive Paradox**, and **Survivorship Bias**. Through simulation and calculation, I demonstrate how misleading metrics can distort reality.

---

## 1. Latency Skew: Why Standard Deviation Lies

**Scenario:** Network latency logs with 980 normal requests (20-50ms) and 20 spike outliers (1000-5000ms).

### Results:
- **Standard Deviation (SD):** 422.17
- **Median Absolute Deviation (MAD):** 8.00
- **SD/MAD Ratio:** 52.77x

### Finding:
The standard deviation **explodes** due to outliers because it uses **squared deviations** from the mean. A spike 3000ms away contributes 3000² = 9,000,000 to the calculation, while a normal deviation of 15ms contributes only 15² = 225—a 40,000x difference.

In contrast, MAD uses **absolute deviations** from the median and takes the median of those deviations. Outliers contribute linearly (3000 vs 15), and because MAD selects the median value, extreme outliers don't dominate the metric.

**Takeaway:** SD is hypersensitive to outliers. MAD provides a robust, stable measure of spread in skewed distributions.

---

## 2. False Positive Paradox: When 98% Accuracy Fails

**Scenario:** "IntegrityAI" plagiarism detector with 98% sensitivity and 98% specificity tested across three environments with different base rates of cheating.

### Results:
| Scenario | Base Rate | P(Cheater \| Flagged) |
|----------|-----------|----------------------|
| **A: Bootcamp** | 50% | 98.00% |
| **B: Econ Class** | 5% | 72.06% |
| **C: Honors Seminar** | 0.1% | 4.68% |

### Finding:
Despite 98% accuracy, the system is **nearly useless** in the Honors Seminar context. When only 0.1% of students cheat, a flagged student has only a **4.68% chance** of actually being a cheater—a 95.32% false positive rate!

This occurs because the **base rate** (prior probability) dominates the posterior probability calculation via Bayes' Theorem. In low-prevalence environments, even highly accurate tests generate mostly false positives.

**Takeaway:** Accuracy metrics are meaningless without context. Always consider the base rate.

---

## 3. A/B Test Validity: Chi-Square Goodness of Fit

**Scenario:** "FinFlash" A/B test with 100,000 users (expected 50/50 split). Control received 50,250 users, Treatment received 49,750 users—500 fewer than expected.

### Results:
- **Observed:** [50,250, 49,750]
- **Expected:** [50,000, 50,000]
- **Chi-Square Statistic:** 2.50
- **Critical Value (p < 0.05):** 3.84

### Finding:
Chi-Square = 2.50 < 3.84, so the deviation is **not statistically significant**. The observed data does not show evidence of bias from random assignment. The "missing" 500 users in treatment can be attributed to random chance.

**Takeaway:** Random assignment produces natural variation. Statistical tests prevent us from seeing patterns in noise.

---

## 4. Survivorship Bias: The Crypto Graveyard

**Scenario:** Simulated 10,000 token launches using a Pareto distribution (power law) where 99% fail near zero market cap.

### Results:
- **Total Tokens:** 10,000
- **Survivors (Top 1%):** 100
- **Mean Market Cap (All Tokens):** $174,084.21
- **Mean Market Cap (Survivors Only):** $4,363,352.12
- **Survivorship Bias Multiplier:** 25.06x

### Finding:
Looking only at successful tokens creates a **25x distortion** of reality. The typical crypto investor sees survivor stories worth $4.3M and assumes this represents the average outcome. The truth: the typical token is worth $174K—and 99% of launches fail to reach even modest valuations.

This is the **survivorship bias**—we only see the winners. The 9,900 tokens in the graveyard are invisible in media coverage, investment pitches, and success narratives.

**Takeaway:** Success stories hide mass failure. Always ask: "What am I not seeing?"

---

## Conclusion

Statistical deception thrives on three weaknesses:
1. **Using the wrong metric** (SD instead of MAD for skewed data)
2. **Ignoring base rates** (accuracy without prevalence context)
3. **Sampling only survivors** (seeing winners, missing losers)

These audits demonstrate that **numbers don't lie, but liars use numbers**. Question the metric, demand the context, and always look for what's hidden.
