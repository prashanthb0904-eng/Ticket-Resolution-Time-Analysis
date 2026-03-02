# Ticket Resolution Time Analysis
**Survival Modeling | SLA Intelligence | Support Operations Analytics**

Modeling Support Ticket Resolution as a Time-to-Event Problem Using Kaplan-Meier Survival Estimation

# Executive Overview

This project applies **Survival Analysis** to evaluate support ticket resolution performance across Tier 1, Tier 2, and Tier 3 teams.

Instead of using traditional averages (which ignore open tickets and create bias), this analysis:

- Correctly incorporates **censored (open) tickets**
- Models resolution as a **time-to-event process**
- Compares tiers using **statistical hypothesis testing**
- Quantifies **SLA risk exposure**
- Provides **operational recommendations**

# Core Insight

Ticket resolution is not just a duration metric — it is a probability distribution over time.

This project demonstrates advanced analytical thinking expected from a Data Analyst in IT Services, Consulting, or Operations Analytics roles.

# Business Context

**Support organizations face three recurring challenges:**

**1)** 📉 Rising SLA breach rates

**2)** ⏳ Long-tail unresolved backlog

**3)** 📊 Inconsistent tier performance

**Traditional analysis methods:**

- Ignore open tickets
- Use biased averages
- Fail to model time uncertainty
- Cannot statistically compare teams

**Strategic Objective:**

To apply survival modeling techniques to support operations data in order to:

- Quantify resolution probability over time
- Statistically compare performance across support tiers
- Measure SLA breach risk exposure
- Evaluate operational standardization effectiveness
- Provide data-driven recommendations to reduce backlog and improve service efficiency

# 📊Dataset Summary

- 🎫 250 Support Tickets

- 🏷️ Tier 1, Tier 2, Tier 3

- 📅 Created Timestamp

- 📅 Closed Timestamp

- 🔁 Open / Resolved Status

# Derived Analytical Variables

| Feature           | Description                                |
| ----------------- | ------------------------------------------ |
| `resolution_time` | Days from creation to resolution/censoring |
| `event_resolved`  | 1 = Resolved, 0 = Open (censored)          |
| `sla_breached`    | Boolean SLA violation flag                 |

# Analytical Framework
## 1️⃣ Time-to-Event Modeling

Resolution treated as:

Event: Ticket Closed

Censored Observation: Ticket Still Open

Duration: Time in days 
```
df["event_resolved"] = df["closed_at"].notna().astype(int)

df["resolution_time"] = (
    df["closed_at"].fillna(pd.Timestamp.today()) 
    - df["created_at"]
).dt.days
```
Why this matters:

Ignoring open tickets underestimates resolution time and misrepresents performance.

## 2️⃣ Kaplan-Meier Survival Estimation

We estimate:

                                          S(t)=P(T>t)

Probability a ticket remains open beyond time t.
 **Kaplan-Meier estimation is ideal because it:**

Ticket resolution is a time-to-event process with incomplete observations (open tickets).

- Accurately models time-to-resolution without discarding open cases
- Correctly accounts for censored observations
- Avoids distributional assumptions
- Provides survival probabilities at any time point
- Enables statistically valid comparison across support tiers

Using Kaplan-Meier prevents biased performance metrics and delivers operationally reliable insights.

## 3️⃣ Multi-Tier Survival Curve Comparison

Separate survival curves plotted for each tier to assess:

- Resolution velocity
- Backlog persistence
- Operational consistency

Interpretation logic:

Steeper curve → Faster resolution

Plateau → Long-tail backlog

Curve overlap → Performance similarity

## 4️⃣ Statistical Validation (Log-Rank Test)

Hypothesis:

H₀: Resolution distributions are identical across tiers
H₁: At least one tier differs

Result:

p = 0.60

Interpretation:

No statistically significant difference between Tier 1 and Tier 3.

Operational meaning:

Standardization across tiers appears consistent.

## 5️⃣ SLA Risk Modeling

SLA threshold: 5 days

| SLA Breach % | Operational Status |
| ------------ | ------------------ |
| < 20%        | Excellent          |
| 20–40%       | Acceptable         |
| 40–60%       | Needs Improvement  |
| > 60%        | Critical           |

# 📈 Results & Business Insights
 
 **Resolution Behavior**

~75% tickets resolved

~25% remain open (active backlog)

Median resolution ≈ 5 days

Distribution is right-skewed (long-tail cases)

**Key Insight:**

Median resolution is reasonable —
but long-tail tickets inflate average resolution time dramatically.

This indicates backlog concentration in complex cases.

# 📊 Tier Performance Comparison

| Tier   | Resolved % | SLA Breach % | Interpretation            |
| ------ | ---------- | ------------ | ------------------------- |
| Tier 1 | ~74%       | ~47%         | Needs process improvement |
| Tier 2 | ~75%       | ~47%         | Similar performance       |
| Tier 3 | ~74%       | ~39%         | Slightly better long-term |

**Strategic Finding**

While tiers appear similar statistically, SLA breach rate (~45%) indicates systemic performance risk.

The issue is not tier inconsistency —
the issue is overall SLA compliance weakness.

# 📉 Survival Curve Interpretation

Early Sharp Drop:
→ Many tickets resolved quickly.

Gradual Flattening:
→ Hard-to-resolve backlog accumulates.

Plateau at ~25%:
→ Persistent unresolved segment.

This plateau represents operational inefficiency or complexity clustering.

# Visual Outputs 

The analysis generates:

- Overall Kaplan-Meier survival curve
- Tier-wise survival comparison curves
- SLA breach distribution summary

These visualizations highlight resolution velocity, backlog persistence, and cross-tier performance patterns.

# Statistical Depth

**Confidence Intervals**

Kaplan-Meier provides CI bands showing uncertainty range.

Observation:

Tier curves overlap within confidence bands → reinforces statistical similarity.

**Limitations**

- No covariates modeled (priority, category, severity)
- Static SLA threshold
- Limited dataset size (250 tickets)

Future modeling could incorporate Cox Regression to analyze influencing factors.

# 💡 Business Impact Quantification

If SLA breach rate reduces from 45% → 30%:

 - Faster ticket throughput
 - Reduced escalation load
 - Improved customer satisfaction
 - Lower operational overhead

Even a 10% reduction in breach rate significantly reduces long-tail backlog.

# 🎯 Operational Recommendations

 -  Introduce early-warning alerts at Day 4
 - Weekly aging-ticket review
 - Backlog root cause clustering
 - Tier-specific performance dashboards
 - Monitor survival probability at Day 5 KPI

# Conclusion

This project demonstrates how survival analysis can transform traditional ticket resolution reporting into statistically rigorous operational intelligence.

By modeling ticket resolution as a time-to-event process:

 - Open tickets were correctly treated as censored observations
 - Resolution probability was measured over time
 - Support tiers were statistically compared
 - SLA risk exposure was quantified

Although tier performance appears statistically consistent, the overall SLA breach rate highlights systemic efficiency challenges.

The analysis shifts the conversation from simple averages to probabilistic performance modeling — enabling data-driven decision-making for support operations.

This reflects the analytical depth, business interpretation, and structured thinking required of a Data Analyst in IT Services or Consulting environments.

# Tech Stack

- Python
- Pandas
- NumPy
- Matplotlib
- Lifelines (Survival Analysis)
- Jupyter Notebook

# Dependencies

```
# Data Processing
pandas>=2.0.0
numpy>=1.24.0

# Visualization
matplotlib>=3.7.0
seaborn>=0.12.0

# Survival Analysis
lifelines>=0.27.0

# Notebook Environment
jupyter>=1.0.0
ipython>=8.0.0
```
# Installation

**Clone the Repository**

```
git clone https://github.com/prashanthb0904-eng/Ticket-Resolution-Time-Analysis.git
cd Ticket-Resolution-Time-Analysis
```
**Install Dependencies**

```
pip install -r requirements.txt
```

**Run the Notebook**

```
jupyter notebook "Ticket Resolution Time Analysis.ipynb"
```

# 📂 Project Structure

```
Ticket-Resolution-Time-Analysis/
│
├── Ticket Resolution Time Analysis.ipynb
├── tickets_250.csv
├── README.md
├── requirements.txt
```



