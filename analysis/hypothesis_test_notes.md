# Hypothesis Test Notes

## Metric Being Tested
**Paid conversion rate** (`converted_to_paid`) — the primary success metric tied directly to the North Star metric.

## Reason for Choosing This Metric
Paid conversion rate is the closest measurable proxy to the business decision at hand: whether the new onboarding/activation campaign actually turns more signups into paying customers. Upstream metrics (landing page visits, trial starts, onboarding completion) are useful diagnostics, but they do not by themselves prove the campaign drives revenue-generating behavior. Paid conversion is the metric leadership ultimately cares about when deciding whether to launch.

## Hypotheses

- **Null hypothesis (H0):** There is no difference in paid conversion rate between the Control group and the Treatment group. (p_treatment = p_control)
- **Alternate hypothesis (H1):** The paid conversion rate differs between the Control group and the Treatment group. (p_treatment ≠ p_control)

## Test Type
- **Two-proportion z-test**
- **Two-tailed** test was used, since at the design stage we could not assume the campaign would only ever help — a two-tailed test protects against missing a case where the new campaign actually *hurts* conversion.
- **Significance level (α):** 0.05

## Test Inputs (from cleaned_data, n = 1,400 after deduplication)

| Group | n | Converted | Conversion rate |
|---|---|---|---|
| Control | 690 | 22 | 3.19% |
| Treatment | 710 | 50 | 7.04% |

## Test Calculation

- Pooled proportion: p̂ = (22 + 50) / (690 + 710) = 0.0521
- Standard error: SE = √[p̂(1−p̂)(1/690 + 1/710)] = 0.0117
- Z statistic = (0.0704 − 0.0319) / 0.0117 = **3.26**
- Two-tailed p-value = **0.0011**

## Decision Rule
If p-value < α (0.05), reject H0 and conclude the difference in conversion rate is statistically significant.

## Result
p-value (0.0011) < α (0.05) → **Reject the null hypothesis.**

## Business Interpretation
The Treatment group's paid conversion rate (7.04%) is more than double the Control group's rate (3.19%) — a relative lift of approximately **+121%**. This difference is very unlikely to be due to random chance (p = 0.0011, well below the 0.05 threshold). On the primary success metric alone, the new onboarding/activation campaign shows a statistically significant improvement.

However, this result only tells us the *primary metric* moved favorably — it does not, by itself, confirm the change is safe to launch. See `outputs/recommendation_memo.md` for how this result was weighed against guardrail metrics (refund rate, support ticket rate) before reaching a final recommendation.

## Note on Segment-Level Variation
While the overall effect is significant, the lift was not perfectly uniform across regions (ranging from +1.7pp in West to +5.4pp in North) and was notably larger for Free-plan users (+6.2pp) than Basic-plan users (+0.3pp). This is discussed further in the recommendation memo and should temper a blanket "launch to everyone" interpretation.
