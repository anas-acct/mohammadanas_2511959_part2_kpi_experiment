## 1. Business Problem Statement

### Context & Decision
The leadership team of our subscription-based digital product company must decide whether to deploy the newly designed user onboarding and activation campaign across the entire global user base (Treatment group), or maintain the current onboarding experience (Control group). 

### Audience & Impact
This decision directly impacts all incoming newly registered users by altering their first-time user experience. Indirectly, it impacts the product, customer support, and finance teams, as changes in early user behavior will influence downstream customer support volume, infrastructure stability, and recurring subscription revenue.

### Core Objectives & Metrics
The primary business objective is to accelerate early user engagement and maximize the conversion pipeline from free or trial sign-ups to long-term paying subscribers. The core metric targeted for improvement is the Paid Conversion Rate.

### Monitored Risks
While optimizing for conversion, we must actively monitor for negative downstream effects (guardrails). Key risks include:
- Artificially inflating trial sign-ups that lead to a spike in cancellation/refund rates.
- Confusing or frustrating users, resulting in a surge of customer support tickets.
- Lowering overall revenue quality by attracting low-engagement segments with unsustainable discount or trial structures.

### Evidence Required for Recommendation
Before making a final launch recommendation to leadership, the following empirical evidence is required:
1. Statistical significance (p-value < 0.05) demonstrating a meaningful increase in the primary conversion metric for the Treatment group.
2. Stability or improvement in specified guardrail metrics (Refund Rate, Support Ticket Rate, and Engagement Score) to ensure no segment-level degradation.
3. Clean, verified experiment data free of significant duplicates, anomalies, or structural imbalances.

## 2. Dataset Description

The dataset (`campaign_experiment_data.xlsx`) contains user-level behavior, conversion, and operational data collected during the onboarding campaign experiment. It tracks the end-to-end journey of newly registered users, from initial landing page visits to revenue generation and post-activation interactions.

**Key Data Categories included in the dataset:**
* **Experiment Assignment:** Indicates whether the user was placed in the `Control` group (legacy onboarding experience) or the `Treatment` group (new onboarding campaign).
* **User & Segment Characteristics:** Includes unique user identifiers and categorical segments required for deeper analysis, such as Region, Device Type, Traffic Source, and Plan Type.
* **Funnel & Conversion Flags (Binary):** Boolean indicators showing if a user reached specific funnel milestones (e.g., visited landing page, started trial, completed onboarding, and converted to a paid subscription).
* **Financial Metrics:** Continuous data tracking the actual Revenue generated per user and the Days to Convert.
* **Guardrail & Operational Metrics:** Includes continuous and binary data for Average Engagement Score, Refund Requests, and Support Ticket creations to monitor product friction and revenue quality.

## 3. North Star Metric Definition

**Selected North Star Metric:** Paid Conversion Rate (Total Users Converted to Paid / Total Users)

**Why this is the primary success metric:**
The core objective of the new onboarding campaign is to improve early user activation. While getting users to start a trial or complete a profile is important, the ultimate proof of a successful onboarding experience in a subscription model is the user's willingness to pay. The Paid Conversion Rate directly measures the campaign's effectiveness at bridging the gap between initial curiosity (sign-up) and realized product value (payment). 

**Why other metrics are supporting/guardrails instead of the North Star:**
* **Leading Indicators (Drivers):** Metrics like *Trial Start Rate* or *Onboarding Completion Rate* are leading indicators. A user might complete onboarding but still churn before paying. They drive the North Star but do not guarantee business value on their own.
* **Lagging Indicators (Value):** *Average Revenue Per User (ARPU)* is heavily influenced by plan pricing, cross-sells, or external marketing factors, making it too broad to isolate the specific impact of the onboarding campaign.
* **Guardrail Metrics:** Metrics like *Refund Rate* or *Support Ticket Rate* measure friction and operational cost. They are necessary to ensure we aren't breaking the product, but they do not measure top-line growth.

**Connection to Business Growth:**
In a subscription-based business, top-of-funnel user acquisition is expensive. Increasing the Paid Conversion Rate directly increases Monthly Recurring Revenue (MRR) and decreases the Customer Acquisition Cost (CAC) payback period. A higher conversion rate means the business can scale faster and invest more heavily in product development and marketing.

**Risks of Blind Optimization:**
If the Paid Conversion Rate is optimized blindly without monitoring guardrails, the business is exposed to severe risks. For example, the campaign could use aggressive "dark patterns" (e.g., hiding the cancel button, forcing immediate billing) or offer unsustainable, massive discounts to spike the conversion rate. This would artificially inflate conversions in the short term but lead to a massive surge in **Refund Rates**, immediate first-month churn, overwhelmed customer support, and permanent damage to brand trust.

## 4. KPI Tree Summary

The KPI Tree (located in `outputs/kpi_tree.png`) visually breaks down the North Star Metric (Paid Conversion Rate) into actionable and measurable drivers. 

**Tree Structure:**
* **Top-of-Funnel Acquisition:** Driven by Landing Page Visit Rate and Trial Start Rate, measuring the initial volume of users entering the funnel.
* **User Activation & Engagement:** Driven by Onboarding Completion Rate and Average Engagement Score, tracking how effectively the new campaign drives product adoption.
* **Conversion Efficiency:** Driven by Average Days to Convert and Average Revenue per Converted User, ensuring conversions happen in a timely, profitable manner.

**Guardrail Metrics:** 
To ensure the conversion lift is healthy and sustainable, the tree explicitly outlines three guardrails: Refund Rate, Support Ticket Rate, and Segment-Level Performance. These ensure we are not optimizing conversion at the expense of operational stability or overall revenue quality.

## 5. Experiment Analysis Approach & Data Cleaning

Before analyzing the experiment results, the dataset (`campaign_experiment_data.xlsx`) underwent rigorous data cleaning and preparation to ensure statistical validity:
* **Duplicate Handling:** Identified and removed 8 duplicate `user_id` records to prevent double-counting.
* **Missing Values:** `days_to_convert` contained expected nulls (non-converted users). Minor nulls in `device_type` (18), `traffic_source` (24), and `engagement_score` (14) were retained as "Unknown" to preserve the size of the control/treatment groups.
* **Group Counts & Segment Distribution:** Confirmed a balanced randomization split (Treatment: 715 users, Control: 693 users) with no major segment-level assignment biases.
* **Invalid Binaries:** Validated that all boolean funnel stages (landing page visits, trial starts, conversions) contained only strictly valid 0/1 integers.
* **Revenue Outliers:** Identified 4 extreme upper-bound outliers in `revenue_30d` (exceeding \$2,340, with a max of \$8,610). While retained for total revenue calculations, these were noted as potential skews for ARPU (Average Revenue Per User) interpretation.

## 6. Hypothesis Test Summary

To rigorously evaluate the experiment, a one-tailed Two-Proportion Z-Test was framed around the North Star metric (Paid Conversion Rate). 
* **Null Hypothesis (H0):** The Treatment group's conversion rate is less than or equal to the Control group.
* **Alternate Hypothesis (HA):** The Treatment group's conversion rate is strictly greater than the Control group.
* **Significance Level:** Alpha = 0.05.

The interpretation logic dictates that a p-value below 0.05 is required to reject the null hypothesis. A successful rejection, combined with stable guardrail metrics, forms the required evidence to recommend a global launch.

## 7. Guardrail Metrics Considered

To ensure a holistic business evaluation, the following guardrails were analyzed:
* **Support Ticket Rate:** Acted as a proxy for user friction. Identified a major operational risk (rate spiked from 14.7% to 24.7% in the treatment group).
* **Average Days to Convert:** Monitored pipeline velocity. Showed a positive reduction of 2.4 days.
* **Refund Rate:** Monitored revenue quality and buyer's remorse. Remained stable and negligible (<0.5%).
* **Segment Decline (Region):** Ensured demographic stability. Confirmed all regions experienced a conversion lift.

## 8. Final Recommendation & Business Justification
**Decision: Continue Testing (Do Not Scale Globally Yet)**

While the quantitative data strongly validates the core mechanic of the new onboarding campaign, deploying it globally in its current state poses a severe operational risk. The recommendation to "Continue Testing" is supported by the following business trade-offs:

* **The Revenue Win (Why we don't discard the campaign):** The Treatment group demonstrated a massive, statistically significant lift in the Paid Conversion Rate (from 3.19% to 7.04%, $p = 0.0005$). It also reduced the average time-to-conversion by 2.4 days. This proves that the underlying marketing message and value proposition are highly effective at driving immediate revenue.
* **The Operational Crisis (Why we don't launch today):** The Treatment group caused the Customer Support Ticket Rate to surge from an acceptable 14.78% to a critical 24.79%. A 10% absolute increase in top-of-funnel support volume would immediately overwhelm our current customer service headcount. The financial cost of expanding the support team, combined with the brand damage of a confusing user experience, threatens to cannibalize the new recurring revenue.
* **Strategic Compromise:** The campaign’s conversion engine works perfectly, but the User Interface (UI) or User Experience (UX) execution is flawed. We must isolate the friction causing the tickets, patch the onboarding flow, and validate the fix through a subsequent A/B test.

## 9. Assumptions, Risks, and Data Limitations
To maintain analytical transparency, this recommendation acknowledges several constraints and assumptions within the data:

* **Extreme Revenue Outliers (Limitation):** The dataset contained four extreme high-value conversions (maxing at $8,610). While these were retained to accurately calculate absolute total revenue, they heavily right-skew the Average Revenue Per User (ARPU). ARPU should not be used for strict revenue forecasting until these outliers are normalized.
* **Novelty Effect (Risk):** The 30-day testing window may be capturing a "novelty effect," where users interact more frequently simply because the interface is new. We are assuming the 7.04% conversion rate will hold steady over time, but long-term cohort retention must be monitored.
* **Missing Data Assumption (MCAR):** Minor missing values in categorical segments (`device_type`, `traffic_source`) were assumed to be Missing Completely at Random (MCAR). We assumed these nulls did not systematically hide a specific demographic failure.
* **Support Ticket Uniformity (Assumption):** This analysis assumes all support tickets cost the same amount of time and money to resolve. If the Treatment group's tickets are simple (e.g., password resets) versus complex (e.g., billing disputes), the operational cost impact might be slightly lower or higher than projected.

## 10. Next Steps & Supporting Documentation
To move forward and safely capture the revenue lift, the following action plan is required:

**Immediate Next Steps:**
1. **Qualitative Ticket Audit:** The Product team must pull a sample of the support tickets generated by the Treatment group to diagnose the exact point of confusion (e.g., a broken link, unclear pricing terms, or a confusing UI button).
2. **Iterate and Patch:** Engineering must deploy a hotfix to the new onboarding flow addressing the identified friction points.
3. **Launch Test V2:** Initiate a secondary A/B test against the Control group to verify that the conversion rate remains above 6.5% while the support ticket rate drops back below 15%.

**Supporting Project Files:**
To verify the data models, statistical tests, and framework design, the following artifacts are included in this repository:
* `mohammadanas_2511959_part2_kpi_experiment
/part2_kpi_experiment/outputs/kpi_tree.png`: Visual architecture breaking down the North Star metric into actionable drivers.
* `screenshots/summary_metrics.png`: A snapshot of the aggregated Excel pivot tables comparing Control vs. Treatment performance.
* `screenshots/hypothesis_test_output.png`: The mathematical evidence and Z-Statistic calculation verifying the conversion lift is not due to random chance.
