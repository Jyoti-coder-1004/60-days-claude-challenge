# Day 30: Supply Chain Optimization Builder

## 🚀 Project Overview
For Day 30 of the 60-Day Coding Challenge, I built an interactive **Supply Chain Builder & Optimizer** simulation. The application is designed to teach users the foundational mechanics, trade-offs, and critical decisions involved in modern global logistics management. 

The application generates a randomized enterprise profile and challenges the user to architect an optimized supply chain network balancing cost, delivery speed, operational risk, customer satisfaction, and environmental sustainability.

---

## 🛠️ Tech Stack & Architecture
* **Frontend Framework:** React 18 (via CDN)
* **Compiler:** Babel JSX (Standalone via CDN)
* **Styling:** Vanilla CSS3 with semantic modern CSS Variables
* **Design Paradigm:** Premium dark-themed enterprise dashboard interface
* **Deployment Structure:** Single-file standalone HTML (`index.html`), fully operational offline with zero external image/asset dependencies.

---

## 📋 Core Features
1. **Dynamic Company Parameter Generator:** Randomizes target industry, core product constraints, baseline localized demand, and regional market corridors upon each instantiation.
2. **Step-by-Step Architecture Guide:** Walks the player through 5 core structural layout nodes:
   * **Sourcing Strategy:** Single vs. Diversified Multi-Sourcing.
   * **Production Footprint:** Distributed Local vs. Centralized Offshore Factories.
   * **Distribution Strategy:** JIT Hubs vs. Bulk Regional Warehouses.
   * **Logistics Modes:** Road, Rail, Ocean, or Expedited Air Freight.
   * **Buffer Inventory Caps:** Low (Lean/JIT), Balanced, or Deep Safety Stock.
3. **Contextual Trade-Off Panels:** Explains the industrial *why* behind every mechanism before a user locks in a path, providing a comprehensive learning loop for beginners.
4. **Live Dynamic Metric Gauges:** Instantly computes system impacts on Core Cost, Pipeline Speed, Fragility Risk, Satisfaction Sentiment, and Green Sustainability.
5. **Post-Mortem Executive Dashboard:** Evaluates overall architectural health, rendering a performance score (0–100), customized operational strengths, critical system vulnerabilities, and high-priority optimization recommendations.

---

## 💡 Key Learnings & Strategic Insights
* **The Fragility of Lean:** Pursuing a pure Just-In-Time (JIT) model with single-source vendors drastically improves short-term cost structures but creates an extremely brittle network vulnerable to minor macroeconomic friction (e.g., transit bottlenecks, natural disruptions).
* **Multi-Echelon Balancing Acts:** Supply chain efficiency is fundamentally an exercise in navigating conflicting trade-offs. Expediting shipping speeds via air cargo directly sacrifices margin and sustainability metrics; expanding local factory footprints minimizes geopolitical risk but introduces heavy capital investment hurdles.
* **Resilience as an Asset:** Transitioning from "cheapest cost" sourcing toward structured redundancy (multi-sourcing, strategic regional safety buffers) protects long-term customer equity and insulates an enterprise from systemic market shocks.

---

## 📸 Simulation Screenshots
*(Place your playthrough dashboard screenshots inside the `Day30/` directory and update the paths below)*

![Dashboard Welcome Page](./screenshot_welcome.png)
![Active Optimization Pipeline](./screenshot_metrics.png)
![Final Performance Assessment](./screenshot_results.png)