# Day 28: Hospital Admission Readiness Simulator 🏥

## 📌 Project Overview
The **Hospital Admission Readiness Simulator** is an interactive healthcare workflow tool designed to simulate real-time emergency room (ER) triaging and inpatient bed allocation. It helps healthcare operations managers visualize hospital capacity, optimize patient flow, and evaluate operational readiness based on changing acuity levels and staffing constraints.

---

## 🚀 Key Features
*   **Dynamic Patient Triage:** Implements an Emergency Severity Index (ESI) scoring system (Levels 1 to 5) to categorize patient urgency.
*   **Live Bed Inventory Management:** Real-time tracking of Intensive Care Unit (ICU), General Ward, and Emergency Department (ED) beds.
*   **Operational Readiness Alerts:** Visual warnings when bed occupancy exceeds safety thresholds (e.g., >85% capacity) or staffing ratios drop.
*   **Simulated Intake Workflow:** Interactive processing queue that admits, transfers, or discharges patients dynamically.

---

## 🛠️ Technology Stack
*   **Language:** Python 3.x
*   **UI Framework:** Streamlit (or Command-Line Interface)
*   **Data Handling:** Pandas / Datetime

---

## 💻 Code Structure & Implementation
The core logic evaluates admission readiness using a weighted criteria matrix:
1.  **Acuity Match:** Ensures high-ESI patients are routed to critical care units immediately.
2.  **Resource Constraints:** Factors in nurse-to-patient ratios prior to accepting transfers.

### Core Simulation Logic (Preview)
```python
def check_admission_readiness(patient_acuity, available_beds, staff_count):
    # Threshold check for hospital safety
    required_ratio = 1 if patient_acuity == "Critical" else 4
    if available_beds > 0 and staff_count >= required_ratio:
        return {"status": "Ready", "action": "Proceed with Admission"}
    else:
        return {"status": "Delayed", "action": "Route to Alternative Facility / Surge Protocol"}