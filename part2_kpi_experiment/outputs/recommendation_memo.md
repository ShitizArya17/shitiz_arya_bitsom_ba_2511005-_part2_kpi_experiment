# Recommendation Memo: Onboarding & Activation Campaign

## Executive Summary
The Treatment group (new onboarding/activation campaign) showed a statistically significant **+121% relative lift** in paid conversion rate versus Control (7.04% vs 3.19%, p = 0.0011). Upstream funnel metrics (landing page visits, trial starts, onboarding completion) and engagement score also improved, and time-to-convert shortened. However, the support ticket rate rose substantially (14.8% → 24.8%) and a small refund rate appeared in Treatment that did not exist in Control (0% → 0.42%). The lift was also uneven across plan types, with Free-plan users seeing nearly all of the benefit. **Recommendation: launch, but with monitoring on support load and a closer look at the Basic-plan segment before declaring full success.**

## Business Problem
**Decision needed:** Whether to roll out the new onboarding/activation experience to all users, reject it, launch it only for selected segments, or continue testing.
**Who it impacts:** All new signups to the subscription product, and the support/operations team who will absorb any change in ticket volume.
**Metric that should improve:** Paid conversion rate (North Star), without materially degrading refund rate, support load, or revenue quality.
**Risks to monitor:** Refunds, support tickets, days-to-convert, and whether gains are concentrated in low-value segments (e.g., Free plan users who may convert but generate less revenue).
**Evidence required:** A statistically significant improvement in the primary metric, plus an absence of material guardrail degradation, before a full launch is justified.

## North Star Metric
**Paid conversion rate** — selected because it is the metric most directly tied to revenue impact and reflects the full journey from signup to paying customer, not just an intermediate engagement step. Optimizing it blindly (e.g., through aggressive trial prompts) risks inflating conversions with low-intent or low-quality users — which is exactly why guardrails were tracked alongside it.

## KPI Tree
See `outputs/kpi_tree.png`. The North Star (paid conversion rate) breaks down into three primary drivers: Acquisition & Engagement, Activation Quality, and Conversion Efficiency, each with two measurable sub-drivers. Three guardrail metrics (refund rate, support ticket rate, revenue quality) sit outside the tree and are tracked independently to ensure the North Star isn't being optimized at the expense of customer experience or revenue health.

## Experiment Result Summary

| Metric | Control | Treatment | Change |
|---|---|---|---|
| User count | 690 | 710 | — |
| Landing page visit rate | 63.62% | 72.39% | +8.8pp |
| Trial start rate | 25.07% | 29.01% | +3.9pp |
| Onboarding completion rate | 15.65% | 21.13% | +5.5pp |
| **Paid conversion rate** | **3.19%** | **7.04%** | **+3.9pp (+121% relative)** |
| ARPU | $51.97 | $54.25 | +$2.28 |
| Avg. revenue per converted user | $1,630.10 | $770.41 | −$859.69 |
| Avg. days to convert | 8.86 | 6.40 | −2.46 days (faster) |
| Avg. engagement score | 57.03 | 62.94 | +5.91 |

Every funnel stage improved in Treatment, and conversion happened faster, not just more often.

## Hypothesis Test Interpretation
A two-proportion z-test on paid conversion rate produced z = 3.26, p = 0.0011 — well below the 0.05 significance threshold. We reject the null hypothesis that Control and Treatment convert at the same rate. This is strong statistical evidence, not just a favorable-looking sample difference. Full detail in `analysis/hypothesis_test_notes.md`.

## Guardrail Analysis

| Guardrail | Control | Treatment | Risk? |
|---|---|---|---|
| Refund rate | 0.00% | 0.42% | Small but new — was zero in Control. Worth watching, not yet alarming at this sample size (3 refunds). |
| Support ticket rate (≥1 ticket) | 14.78% | 24.79% | **Notable increase (+10pp).** This is the clearest risk signal in the data — more conversions are arriving with more support burden. |
| Avg. days to convert | 8.86 days | 6.40 days | Improvement, not a risk — faster conversion is a positive guardrail signal. |
| Avg. engagement score | 57.03 | 62.94 | Improvement — suggests converted users in Treatment are also more engaged, not just rushed through. |

**Conclusion:** The support ticket rate increase is the one guardrail that creates genuine operational risk and should be addressed (e.g., via support team staffing or onboarding UX review) before or during a full rollout, even though it does not outweigh the conversion gain.

## Segment-Level Insight

**By region** — conversion lift held across all four regions but was not uniform: North (+5.4pp) and South (+4.4pp) saw the strongest lift, while West (+1.7pp) was comparatively muted.

**By plan type** — this is the most important segment finding: Free-plan users drove almost all of the conversion lift (3.06% → 9.29%, +6.2pp), Premium-plan users saw a solid lift (2.75% → 6.25%, +3.5pp), but Basic-plan users barely moved (3.62% → 3.88%, +0.3pp). The campaign appears to be much more effective at converting users who haven't yet committed to a paid tier than at upgrading existing Basic subscribers.

**By traffic source** — ARPU directionally improved for most sources (Email, Organic, Paid Search, Referral), but Social traffic showed an ARPU decline in Treatment (from $137 to $52). Sample sizes for Social are likely small, so this should be treated as a flag for further investigation rather than a conclusion.

## Final Recommendation
**Launch — with two conditions:**
1. **Monitor and resource for higher support ticket volume** post-launch, since this is the clearest guardrail risk in the data.
2. **Do not assume uniform benefit across plan types.** Consider a follow-up, segment-specific experiment for Basic-plan users, since the current campaign shows little effect there — full-population rollout may not be the most efficient way to convert that segment.

This is not a "Launch only for selected segment" recommendation, since the lift is positive and significant overall and the upside (more than doubling conversion) substantially outweighs the guardrail concerns observed so far. But it is also not an unconditional "ship it and move on" — the support ticket increase needs an operational response, not just a data footnote.

## Risks and Limitations
- The experiment data may not capture seasonality; results reflect the specific test window only.
- Revenue-per-converted-user is lower in Treatment, partly because Treatment converts more lower-intent (often Free-plan) users — this is a quality-mix effect, not a sign each converted user is worth less.
- Refund rate, while statistically small, has not been observed over a long enough post-conversion window to rule out delayed refund risk.
- Segment sample sizes (e.g., Social traffic) are too small in places to draw firm conclusions; these are flagged, not acted on.
- Regression analysis was not performed here — these are descriptive/comparative results, not causal estimates beyond the randomized A/B design itself.

## Next Steps
1. Launch the new onboarding/activation experience to all users, with monitoring dashboards for refund rate and support ticket rate in the first 30–60 days.
2. Open a follow-up investigation into why Basic-plan users responded so little to the campaign — consider a separate, targeted experiment for that segment.
3. Re-run the guardrail analysis after a larger post-launch sample is available, particularly for refund rate (only 3 refund events were observed in this test).
4. Investigate the Social-traffic-source ARPU decline before drawing conclusions about that channel.
