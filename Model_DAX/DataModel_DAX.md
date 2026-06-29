# 🧠 Data Model & DAX Logic – Energy Consumption

## 🎯 Model Objective

The data model is designed to:

- ensure consistent metrics  
- enable multi-dimensional analysis  
- optimize performance  
- simplify DAX logic  

👉 Priority: clarity and scalability over complexity  

---

## 🏗️ Model Architecture

A **Star Schema** has been implemented.

---

### 🔹 Fact Table

**Fact_Consumption**
- Date  
- Plant  
- Equipment  
- Consumption_kWh  
- Cost  

---

### 🔹 Dimension Tables

**Dim_Date**
- Date  
- Day  
- Month  
- Year  

**Dim_Plant**
- Plant  

**Dim_Equipment**
- Equipment  

---

### 📌 Model Relationships

- Fact_Consumption → Dim_Date (Many-to-One)  
- Fact_Consumption → Dim_Plant (Many-to-One)  
- Fact_Consumption → Dim_Equipment (Many-to-One)  

👉 Single-direction relationships  
👉 Clean and unambiguous model  

---

## ✅ Modeling Decisions

### ✔️ Simple model
- No overengineering  
- No unnecessary tables  

👉 Improves maintainability and performance  

---

### ✔️ Fact & dimension separation
- Enables reusability  
- Ensures analytical consistency  

---

### ✔️ Scalability-ready
The model supports:
- new plants  
- additional equipment  
- time expansion  

---

## 🧠 DAX Principles Applied

- Proper use of filter context  
- Clear separation between measures and calculated columns  
- Avoid duplicated logic  
- Use of CALCULATE to modify context  

👉 Focus is on **context, not formulas**

---

## 📊 Core Measures

These measures form the base of the analysis:

```DAX
Total Consumption =
SUM(Fact_Consumption[Consumption_kWh])

Total Cost =
SUM(Fact_Consumption[Cost])

📈 Derived Measures
Average Cost =
DIVIDE(
    [Total Cost],
    [Total Consumption]
)
👉 Uses DIVIDE to prevent division-by-zero errors

🔥 Key Business Indicator
Peak Consumption % =
DIVIDE(
    CALCULATE(
        [Total Consumption],
        Fact_Consumption[Level] = "Peak"
    ),
    [Total Consumption]
)
👉 Critical metric for operational efficiency

🧠 Consumption Classification (Core Logic)
Consumption Level =
VAR AvgEquipment =
    CALCULATE(
        AVERAGE(Fact_Consumption[Consumption_kWh]),
        ALLEXCEPT(
            Fact_Consumption,
            Fact_Consumption[Equipment]
        )
    )
RETURN
    SWITCH(
        TRUE(),
        Fact_Consumption[Consumption_kWh] > AvgEquipment, "Peak",
        Fact_Consumption[Consumption_kWh] < AvgEquipment * 0.5, "Low",
        "Normal"
    )
🎯 What this solves

Avoids arbitrary thresholds
Enables comparison within each group
Produces coherent and actionable analysis

👉 Transforms raw data into analytical context

⚙️ Context Handling (Critical)
Key concepts applied:
✅ Filter Context

Segmentation by plant, equipment, and date
Dynamic KPIs based on user selection


✅ CALCULATE

Context modification
Foundation for advanced measures


✅ ALLEXCEPT

Preserves relevant context
Prevents aggregation errors


⚠️ Common Mistakes Avoided

Mixing measures and calculated columns without purpose
Metrics dependent on visuals
Incorrect global context usage
Poorly defined KPIs (totals vs averages)


🚀 Improvement Proposal (High Impact)
Time Intelligence
Previous Period Consumption =
CALCULATE(
    [Total Consumption],
    DATEADD(Dim_Date[Date], -1, MONTH)
)

Variation % =
DIVIDE(
    [Total Consumption] - [Previous Period Consumption],
    [Previous Period Consumption]
)

👉 Adds essential time comparison context

🚀 Advanced Improvement
Cost Deviation Analysis
Expected Cost =
[Total Consumption] * AVERAGE(Price_Energy[Price])
Deviation =
[Total Cost] - [Expected Cost]

👉 Enables real-time inefficiency detection

✅ Model Outcome
✔️ Simple and efficient model
✔️ Robust metrics
✔️ Scalable design
✔️ Easy to maintain

🧪 Technical Evaluation

Is the model scalable? ✅
Are metrics consistent? ✅
Is context correctly handled? ✅
Are ambiguities avoided? ✅


🔚 Conclusion

A strong dashboard does not start with visuals, but with the data model and how context is defined in DAX.
