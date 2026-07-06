# Day 9: Building & Enhancing an AI Nutrition Analytics App (NutriScope)

## 📌 Project Overview
Today's challenge focused on the core philosophy of professional AI application development: **Iterative Engineering**. Instead of prompting an AI to build a massive, complex application in a single go, I learned to build a functional Minimum Viable Product (MVP) first, and then progressively layer on advanced features. 

Using this approach, I developed **NutriScope**—a premium, dark-themed, single-file HTML nutrition analytics dashboard.

---

## 🚀 Step 1: The MVP (Prompt 1)
The goal was to establish a solid foundation with core functional tracking.

### Core Features:
* **User Profiling:** Inputs for age, gender, height, weight, activity level, and dietary preferences (Vegetarian, Non-Vegetarian, Eggetarian).
* **Food Logging:** Dynamic, editable table to add/remove entries.
* **Built-in Database:** Loaded with 20 staple common foods (Rice, Roti, Dal, Paneer, etc.).
* **Analytics Dashboard:** Tracks 10 essential macro and micronutrients (Calories, Protein, Carbs, Fat, Fiber, Iron, Calcium, Vitamins C, D, and B12) paired with dynamic `Chart.js` visualizations.

### MVP Screenshots
![NutriScope MVP Dashboard](screenshots/mvp_dashboard.png) **

---

## ⚡ Step 2: The Enhanced Version (Prompt 2)
With a stable MVP working perfectly, I used a second structured prompt to drastically scale the application's capabilities.

### Advanced Upgrades:
* **Expanded Database:** Increased the internal database from 20 to 60 common foods.
* **Data Portability:** Added a CSV Upload feature to import external food logs.
* **Smart Planning:** Integrated a 2-day meal planner and advanced risk analysis algorithms.
* **Professional Polish:** Embedded an educational disclaimer, official nutrition sources, and upgraded data visualization charts.

### Enhanced Version Screenshots
![NutriScope Enhanced Dashboard](screenshots/enhanced_dashboard.png) **

---

## 📊 MVP vs. Enhanced Comparison

| Feature | MVP Version | Enhanced Version |
| :--- | :--- | :--- |
| **Food Database** | 20 Basic Items | 60 Extensive Items |
| **Data Input** | Manual Logging Only | Manual Logging + CSV File Upload |
| **Core Logic** | Basic target calculations | Advanced Risk Analysis & Swaps |
| **Meal Planning** | Real-time daily tracking | Full 2-Day Meal Planner |
| **UX/UI Depth** | Clean SaaS Dark Theme | Upgraded charts, clear source citations & disclaimers |

---

## 💡 Key Learnings
1. **The MVP Trap:** Prompting an AI to build a 10-feature app right away usually leads to broken code, missing scripts, or hallucinated UI elements. Starting small guarantees a clean codebase.
2. **Context Preservation:** By building sequentially, the AI cleanly inherited the established CSS styling and JavaScript architecture from the MVP, expanding it flawlessly without breaking existing logic.
3. **SaaS UI Design:** Using single-file setups with Tailwind CSS or raw modern CSS custom variables makes creating beautiful, production-ready dark themes incredibly fast.

---

## 🔗 Project Files
* [View MVP HTML Code](NutriScope_MVP.html)
* [View Enhanced HTML Code](NutriScope_Enhanced.html)