# Day 27: Prior Authorization Story Simulator

## Project Overview
An interactive text-based simulation tool designed to teach healthcare workflows and the complexities of the Prior Authorization (PA) process. Users navigate realistic scenarios from the perspective of a healthcare provider, dealing with insurance companies, medical necessity criteria, and patient care decisions.

## Features
- **Dynamic Branching Storylines:** Multiple narrative paths based on user decisions (e.g., submitting urgent requests, appealing denials, or choosing alternative treatments).
- **State & Score Tracking:** Real-time tracking of critical healthcare metrics:
  - **Patient Health:** Impacted by delays or medication changes.
  - **Administrative Burden:** Time and effort spent on paperwork.
  - **Approval Probability:** Likelihood of insurance coverage based on guidelines met.
- **Realistic Healthcare Scenarios:** Includes clinical cases like prior authorization for a specialty biologic or an advanced imaging scan (MRI).

## Tech Stack
- **Language:** Python / JavaScript (Node.js)
- **Framework/Interface:** Command Line Interface (CLI) / Streamlit / HTML-CSS
- **Core Engine:** Interactive LLM API (Claude) / Rule-based state machine

## How to Run
1. Clone the repository.
2. Install dependencies: `pip install -r requirements.txt` (or appropriate command).
3. Set your API keys in a `.env` file if required.
4. Run the simulator: `python simulator.py`

## Key Takeaways
- Learned how strict documentation criteria (e.g., trying step-therapy before expensive drugs) dictate real-world healthcare insurance approvals.
- Implemented state tracking in interactive storytelling to make choices impactful.