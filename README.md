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
