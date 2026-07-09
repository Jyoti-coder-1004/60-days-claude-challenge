# Day 17: AI Vehicle Cost & Fuel Analysis Dashboard

## 📌 Project Overview
Built an interactive, glassmorphism-styled HTML dashboard utilizing Claude to analyze raw vehicle dataset metrics. The dashboard computes key performance indicators (KPIs) across various fuel types (Petrol, Diesel, CNG, E85, EV), highlights specific vehicle parameters, visualizes aging metrics, and deep-dives into the **E85 Paradox**.

### My Vehicle Configuration
- **Vehicle:** [e.g., Honda Civic / Maruti Swift]
- **Fuel Type:** [e.g., Petrol]
- **Usage Profile:** [e.g., Mixed / City / Highway]
- **Monthly Kilometers:** [e.g., 1200 km]
- **Vehicle Age:** [e.g., 3 years]

---

## 📊 Key Findings & Data Analysis

### 1. Fuel & Maintenance Metrics
* **Cost per KM:** [Summarize which fuel emerged as the cheapest vs most expensive based on your dashboard findings]
* **Environmental Impact:** [Comment on the CO₂ emissions/km comparison, highlighting EV/CNG vs Petrol/Diesel]
* **Maintenance Trends:** As the vehicle transitions through age buckets (*New 0-2y* ➡️ *Mid-life 3-5y* ➡️ *Aged 6-9y* ➡️ *Old 10+y*), maintenance costs per kilometer increased by approximately `[X]%`.

### 2. Deconstructing the E85 Paradox
* **Pump Savings:** E85 offers a theoretical pump discount of `[X]%` compared to standard Petrol.
* **Running Penalty:** Due to lower energy density and mileage drops, the actual running cost penalty for E85 is `[X]%` higher per kilometer.
* **Break-even Price:** For E85 to make economic sense for my vehicle, the pump price needs to drop below `[X] INR/Litre`.
* **Overall E85 Score:** `[X]/10` (Weighted on Cost, CO₂, Refuel Time, and Maintenance).

---

## 📸 Dashboard Screenshots

### Main KPI Dashboard & SVG Visualizations
![Dashboard Overview](./dashboard_overview.png)

### Fuel Comparison & E85 Score Gauge
![Metrics & Gauges](./metrics_gauge.png)

---

## 💡 Key Learnings
1. **No-Code Data Engineering:** Learned how to efficiently prompt LLMs to parse raw multi-variable CSV files and map calculations directly into functional code logic.
2. **CDN-Free UI Assets:** Mastered building responsive, interactive data visualizations (bar charts, doughnut charts, and animated gauges) using pure, inline SVG graphics and native CSS/JS.
3. **The Power of TCO (Total Cost of Ownership):** Realized that a cheaper fuel price at the pump doesn't always translate to actual savings when consumption rates and maintenance overheads fluctuate.

---

## 📂 Deliverables Link
* [Interactive Dashboard HTML File](./vehicle_dashboard.html)