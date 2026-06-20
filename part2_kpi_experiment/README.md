# Part 2: KPI Framework, Business Experiment Analysis & Decision Recommendation

## Business Context
A subscription-based digital product company launched a new onboarding and activation campaign (Treatment) to test against its existing onboarding experience (Control). Leadership needs a data-backed decision on whether to launch the new experience to all users, reject it, launch it only for selected segments, or continue testing.

## Dataset Description
`data/campaign_experiment_data.xlsx` contains 1,408 raw user-level rows (1,400 after removing 8 exact duplicate rows) with the following fields: `user_id`, `signup_date`, `experiment_group` (Control/Treatment), `region`, `device_type`, `traffic_source`, `plan_type`, and behavioral/outcome fields (`visited_landing_page`, `started_trial`, `completed_onboarding`, `converted_to_paid`, `revenue_30d`, `support_tickets_30d`, `refund_requested`, `days_to_convert`, `engagement_score`).

**Data quality checks performed** (see `analysis/experiment_analysis.xlsx`):
- 8 exact duplicate rows identified (via full-row fingerprint) and removed, keeping the first occurrence
- Missing values found in `device_type` (18 rows) and `traffic_source` (24 rows) — filled with "Unknown"
- All binary fields (`visited_landing_page`, `started_trial`, `completed_onboarding`, `converted_to_paid`, `refund_requested`) confirmed to contain only valid 0/1 values
- Group counts confirmed balanced: Control = 690, Treatment = 710 (post-cleaning)
- Revenue outliers checked among converted users using the IQR method; 4 high-revenue values were flagged but retained, since they reflect genuine high-value customers rather than data entry errors
- Segment distribution (region, device type, traffic source, plan type) checked across both groups to confirm randomization held

## North Star Metric
**Paid conversion rate.** This is the main success metric because it is the closest proxy to actual revenue impact — it tells us not just whether users engaged, but whether they became paying customers. Funnel metrics like landing page visits or trial starts are supporting/diagnostic metrics: necessary steps, but not proof of business value on their own. Optimizing paid conversion rate blindly carries risk (e.g., converting low-intent or low-value users, or accepting more refunds/support burden for the sake of a higher number), which is why guardrail metrics were tracked alongside it.

## KPI Tree Summary
See `outputs/kpi_tree.png`. Paid conversion rate breaks into three primary drivers — Acquisition & Engagement, Activation Quality, and Conversion Efficiency — each with two sub-drivers. Three guardrail metrics (refund rate, support ticket rate, revenue quality) sit outside the tree to ensure the North Star isn't optimized at the expense of customer experience.

## Experiment Analysis Approach
Cleaned data was used to compute Control vs Treatment summary statistics for 11 required metrics (`outputs/experiment_summary.xlsx`, sheet `Group_Summary`), plus segment-level breakdowns for 3 metrics:
- Paid conversion rate by region
- Support ticket rate by device type
- Average revenue per user by traffic source

All formulas reference a `cleaned_data_reference` sheet within the same workbook so the file is self-contained and fully auditable (no external links).

## Hypothesis Test Summary
A two-proportion z-test was run on paid conversion rate (Control vs Treatment), two-tailed, α = 0.05.
- z = 3.26, p = 0.0011 → statistically significant
- Treatment conversion rate (7.04%) is more than double Control (3.19%), a +121% relative lift
- Full details: `analysis/hypothesis_test_notes.md`

## Guardrail Metrics Considered
- **Refund rate:** 0.00% (Control) → 0.42% (Treatment) — small increase, worth monitoring
- **Support ticket rate:** 14.78% (Control) → 24.79% (Treatment) — the most significant guardrail risk identified
- **Average days to convert:** 8.86 → 6.40 days — improvement, not a risk
- **Average engagement score:** 57.03 → 62.94 — improvement, not a risk

## Final Recommendation
**Launch**, with two conditions: (1) prepare for higher support ticket volume post-launch, and (2) investigate why Basic-plan users showed minimal conversion lift (+0.3pp) compared to Free-plan (+6.2pp) and Premium-plan (+3.5pp) users before assuming uniform benefit across the customer base. Full reasoning in `outputs/recommendation_memo.md`.

## Assumptions and Limitations
- Exact-duplicate rows (all 16 fields identical) were treated as data entry/export errors and removed; no conflicting duplicates (same `user_id`, different data) were found in this dataset.
- Missing `device_type` and `traffic_source` values were assumed to be legitimately unknown (e.g., ad blockers or referrer-stripping) rather than data errors, and were labeled "Unknown" rather than imputed.
- Revenue outliers among converted users were retained as genuine high-value customers rather than removed, since there was no evidence they were data errors.
- The experiment design (random assignment to Control/Treatment) is assumed to have held; segment distribution checks support this.
- This analysis is descriptive/comparative based on a single A/B test window; it does not control for seasonality or external factors outside the test period.
- Segment sample sizes are uneven in places (e.g., Social traffic source), so some segment-level findings are flagged as directional rather than conclusive.

## Screenshots Included
- `screenshots/summary_metrics.png` — Control vs Treatment summary table
- `screenshots/hypothesis_test_output.png` — Z-test calculation and decision
- `screenshots/kpi_tree_preview.png` — KPI tree diagram
