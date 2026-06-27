# Hypothesis Test Notes

**1. Metric Being Tested**
Paid Conversion Rate (The proportion of top-of-funnel users who successfully converted to a paid subscription).

**2. Reason for Choosing This Metric**
As outlined in the KPI Tree framework, the Paid Conversion Rate is our North Star Metric. While leading indicators like trial starts are helpful, the core business objective of this campaign is to accelerate revenue generation. This metric is the definitive proof of business value.

**3. Null Hypothesis (H0)**
The new onboarding campaign (Treatment) does not improve the Paid Conversion Rate compared to the existing experience (Control). The difference in conversion rates is less than or equal to zero.
* H0: P_treatment <= P_control

**4. Alternate Hypothesis (HA)**
The new onboarding campaign (Treatment) significantly increases the Paid Conversion Rate compared to the existing experience (Control).
* HA: P_treatment > P_control

**5. Test Type (Tailedness)**
One-tailed test (specifically, a Two-Proportion Z-Test). We are using a one-tailed test because leadership's decision relies strictly on whether the new campaign is *better* (an increase). We do not need to test if it is merely "different."

**6. Significance Level (Alpha)**
0.05 (5%). We require a 95% confidence level, meaning we are willing to accept a maximum 5% probability of a false positive (concluding the campaign works when it actually doesn't).

**7. Interpretation Logic & Business Decision Mapping**
* **If P-value < 0.05:** We reject the Null Hypothesis. This provides statistical evidence that the Treatment campaign drives a meaningful lift in conversions. Assuming guardrail metrics (refunds, support tickets) remain healthy, the recommendation will be to **Launch**.
* **If P-value >= 0.05:** We fail to reject the Null Hypothesis. We lack sufficient evidence that the new campaign is an improvement. Because rolling out new code carries engineering and operational costs, the recommendation will be **Do not launch** or **Continue testing**.


**8. Test Execution & Results Summary**
A Two-Proportion Z-Test was conducted to compare the Paid Conversion Rates of the Control and Treatment groups.
* **Control Group Inputs:** 22 conversions out of 690 total users (3.19% conversion rate).
* **Treatment Group Inputs:** 50 conversions out of 710 total users (7.04% conversion rate).

**Statistical Output:**
* **Z-Statistic:** 3.2640
* **P-Value:** 0.0005

**9. Decision Rule & Business Interpretation**
* **Decision Rule Applied:** Because the p-value (0.0005) is significantly less than the Alpha threshold of 0.05, we successfully reject the Null Hypothesis.
* **Business Interpretation:** There is overwhelming statistical evidence that the new onboarding campaign (Treatment) drives a meaningful and substantial increase in Paid Conversion Rates. The new campaign more than doubled the conversion rate (from 3.19% to 7.04%). From a top-of-funnel perspective, this test is a definitive success. Pending a final review of the operational guardrail metrics, the quantitative evidence strongly supports a "Launch" recommendation.
