# 📞 PwC Call Center Performance Dashboard

An end-to-end business intelligence dashboard built in Power BI analyzing call center operations across January–March 2021. The project goes beyond surface-level volume metrics to surface systemic service failures, agent-level performance gaps, and response time inefficiencies.

---

## 📁 Dataset

- **Source:** PwC Call Center Dataset (Kaggle)
- **Size:** 5,000 calls | 3 months (Jan–Mar 2021) | 8 agents | 5 topics
- **Fields:** Call ID, Agent, Date, Time, Topic, Answered (Y/N), Resolved (Y/N), Speed of Answer (seconds), Talk Duration, Satisfaction Rating

---

## 🛠️ Tools Used

- **Power BI Desktop** — data modeling, DAX measures, dashboard design
- **Power Query** — data transformation and calculated columns
- **DAX** — KPI measures, dynamic rankings, MoM calculations, conditional logic

---

## 📊 Dashboard Structure (4 Pages)

### 1. Call Center Operations Overview
High-level executive summary of call center health.

**KPIs:** Total Calls (5,000) | Answer Rate (81.08%) | Resolution Rate (72.92%) | Avg Satisfaction (3.40/5) | Abandonment Rate (18.92%)

**Visuals:**
- Monthly call volume trend with MoM change callouts (-8.8% Jan→Feb, -0.2% Feb→Mar)
- Resolved vs Unresolved calls by Topic with Abandonment Rate overlay (combo chart)

---

### 2. Operational Efficiency & Performance
Operational deep dive into SLA compliance, response times, and time-of-day patterns.

**KPIs:** SLA Compliance (44.30%) | SLA Breach Rate (55.70%) | Avg Speed of Answer (67.52s) | Avg Talk Duration (3:45)

**Visuals:**
- Agent Performance Matrix — scatter plot mapping Resolution Rate vs Speed of Answer per agent
- Answered vs Abandoned calls by Time of Call (Morning/Afternoon/Evening) with SLA line overlay

**Notable finding:** More than half of all answered calls breach the 60-second SLA threshold, indicating a systemic response time problem rather than individual agent failure.

---

### 3. Agent Performance Analysis
Individual agent accountability with composite ranking logic.

**KPIs:** Answered Calls (4,054) | Not Answered (946) | Resolved (3,646) | Unresolved (1,354) | Top Performer: Jim

**Visuals:**
- Combo chart: Calls by agent (Resolved vs Unresolved stacked bars) + Avg Satisfaction line
- Agent Performance Summary table: Agent | Avg Satisfaction | Answer Rate | Resolution Rate | Abandonment Rate

**DAX Highlight — Composite Agent Ranking:**
```
Top Agent Name = 
CALCULATE(
    SELECTEDVALUE(Sheet1[Agent], "Multiple Agents"),
    TOPN(1,
        ALLSELECTED(Sheet1[Agent]),
        [Answered Call], DESC,
        [Resolution Rate of answered calls], DESC,
        [SLA], DESC,
        [Avg Sat. Rating], DESC,
        [Avg Speed of answer], ASC
    )
)
```
Rather than ranking agents on a single metric, this ranks across 5 dimensions simultaneously — call volume, resolution rate, SLA compliance, satisfaction, and speed.

---

### 4. Topic Failure Breakdown
Where is the call center actually failing customers?

**KPIs:** Slow Response Rate (30.39%) | Abandoned Calls (946) | Worst Abandonment Topic: Technical Support | Slowest Topic: Payment Related

**Visuals:**
- Response Time Distribution bar chart (Very Fast / Fast / Medium / Slow buckets)
- Topic Failure table: Topic | Total Calls | Abandoned Calls | Unresolved Issues | SLA%

**DAX Highlight — Response Time Bucketing:**
```
Response Time Category = 
SWITCH(
    TRUE(),
    Sheet1[Speed of answer in seconds] <= 30, "Very Fast",
    Sheet1[Speed of answer in seconds] <= 60, "Fast",
    Sheet1[Speed of answer in seconds] <= 90, "Medium",
    "Slow"
)
```

---

## 🔍 Key Findings

- **55.7% SLA breach rate** — the majority of answered calls exceed the 60-second response threshold
- **Technical Support** leads in abandoned calls (214 out of 946 total) — a likely understaffing or complexity issue
- **30.39% of calls classified as Slow response** despite Very Fast calls being the largest bucket — the middle tier is dragging overall SLA down
- **946 calls never answered** — nearly 1 in 5 calls dropped entirely
- **408 calls answered but not resolved** — agents picking up without solving, a training or process gap
- **Satisfaction at 3.40/5** — below acceptable threshold for most call center benchmarks
- **Call volume declining** (-8.8% Jan→Feb) but service quality metrics not improving in parallel

---

## ⚠️ Data Limitations

This dataset is synthetically generated, which produces artificially low variance across agents and topics. Real call center data would show more dramatic outliers. Findings reflect overall operational patterns rather than individual anomalies. Recognizing data quality issues is part of the analytical process.

---

## 📐 DAX Measures Built

| Measure | Purpose |
|---|---|
| `Resolution Rate` | Resolved calls / Answered calls |
| `Abandonment Rate` | Not answered / Total calls |
| `SLA` | Calls answered within 60s / Answered calls |
| `SLA Breach` | 1 - SLA |
| `Calls Within SLA` | COUNT where Speed of Answer <= 60s AND Answered = Y |
| `MoM_Calls` | Month-over-month % change in call volume |
| `MoM_Res%` | Month-over-month % change in resolution rate |
| `MoM_Sat` | Month-over-month % change in satisfaction |
| `MoM_Aband%` | Month-over-month % change in abandonment rate |
| `Top Agent Name` | Dynamic TOPN ranking across 5 dimensions |
| `Worst Agent Name` | Inverse composite ranking |
| `Slowest Topic` | TOPN to surface lowest SLA topic |
| `Worst Abandonment Topic` | TOPN to surface highest abandonment topic |
| `Slow Response %` | % of answered calls in Slow bucket |
| `Calls Trend UI` | Formatted MoM indicator with direction arrow |

---

## 🚀 How to Use

1. Download the `.pbix` file
2. Open in Power BI Desktop
3. Use the Agent and Topic slicers on any page to filter the full dashboard
4. Navigate pages using the arrow buttons top right of each page

---

*Built by Ahmed El Desoky Gaffer | https://github.com/AHMEDGAFFER421*
