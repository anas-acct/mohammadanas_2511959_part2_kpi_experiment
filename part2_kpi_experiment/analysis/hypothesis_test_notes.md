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
