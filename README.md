# Energy-Consumption-Analytics-Dashboard
End-to-end Power BI dashboard for energy consumption analysis, identifying inefficiencies and enabling data-driven decisions.
# ⚡ Energy Consumption Analysis in Power BI

## 📌 Business Problem

Industrial plants consume energy unevenly, but there is no clear visibility into:
- where consumption is concentrated  
- which equipment drives the highest costs  
- when peak usage occurs  
- what actions should be taken  

👉 This limits the ability to optimize costs and improve operational efficiency.

---

## 🎯 Objective

Answer a key business question:

👉 **Where is energy being consumed the most and what decisions should be made to optimize it?**

---

## 👤 Target Users

- Operations managers  
- Facility managers  
- Industrial leadership  

👉 They need to understand the situation quickly and make data-driven decisions.

---

## 🏗️ Dashboard Structure

The report is designed as an analytical application with three levels:

### 🟢 1. Overview

**Objective:** understand the overall situation in seconds  

**Main KPIs:**
- ⚡ Total Consumption  
- 💶 Total Cost  
- ⚙️ Average Cost (€/kWh)  
- 🔥 Peak Consumption (%)  

👉 Answers: **What is happening?**

---

### 🔵 2. Diagnosis

**Objective:** identify root causes  

**Analysis by:**
- Plant  
- Equipment  
- Time (hour)  

**Enables detection of:**
- consumption patterns  
- peak demand  
- plant distribution  
- critical equipment  

👉 Answers: **Where is the problem?**

---

### 🔴 3. Action

**Objective:** support decision-making  

**Includes:**
- detailed table with consumption and cost  
- classification by level (Peak / Normal / Low)  
- ✅ Top 3 equipment per plant  

👉 Answers: **What should I do?**

---

## 🧠 Data Model

Simplified **star schema model**:

Fact_Consumption

Date
Plant
Equipment
Consumption_kWh
Cost

Dim_Date
Dim_Plant
Dim_Equipment

---

👉 Intentionally simple design to prioritize analysis and performance.

---

## 📊 DAX Metrics

### Core Measures

```DAX
Total Consumption = SUM(Consumption[Consumption_kWh])

Total Cost = SUM(Consumption[Cost])

Average Cost = DIVIDE([Total Cost], [Total Consumption])

### Key Indicator
Peak Consumption % =
DIVIDE(
    CALCULATE([Total Consumption], Consumption[Level] = "Peak"),
    [Total Consumption]
)

### Consumption Classification (Smart Analysis)

Consumption Level =
VAR AvgEquipment =
    CALCULATE(
        AVERAGE(Consumption[Consumption_kWh]),
        ALLEXCEPT(Consumption, Consumption[Equipment])
    )
RETURN
    SWITCH(
        TRUE(),
        Consumption[Consumption_kWh] > AvgEquipment, "Peak",
        Consumption[Consumption_kWh] < AvgEquipment * 0.5, "Low",
        "Normal"
    )
👉 Enables comparison of each equipment within its own context.

🧠 Analytical Concepts Applied

Proper use of DAX filter context
Clear distinction between totals and averages
Multi-dimensional segmentation
Dynamic classifications (no hardcoded rules)

👉 Analytical approach, not just technical implementation.

🎨 Dashboard Design
Principles

Simplicity and clarity
Strong visual hierarchy
Consistency across pages
Decision-focused design

Key Decisions

Use of HTML for custom KPIs
Maximum of 5–6 visuals per page
Clean grid-based layout
Semantic colors:

Green → positive
Red → alert




⚠️ Common Pitfalls Avoided

KPIs without context
Mixing totals with averages
Overloaded dashboards
No clear business objective

👉 Every visual answers a specific question.

📈 Key Insights

A significant portion of consumption occurs during peak hours
Consumption is not evenly distributed across plants
A small number of equipment accounts for most of the usage

👉 The problem is localized, not global.

✅ Recommended Actions

Review energy usage during peak hours
Reschedule equipment operation
Prioritize optimization of high-consumption equipment


🧪 Stack

Power BI
DAX
HTML (custom visuals)
Star schema data model


✅ Final Evaluation
✔️ Understandable in less than 10 seconds
✔️ Enables decision-making
✔️ Clear analytical flow:
➡️ What → Why → Action

🔚 Conclusion

The value is not in the tools, but in how you model, analyze, and communicate data.
