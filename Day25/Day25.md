# Day 25: AI Shark Tank Simulator 🦈💼

## 🎯 Overview
Today, I built a complete, production-quality **AI Shark Tank Simulator** using Claude. The application allows users to pitch their startup concepts to four distinct AI judges, each evaluating the business through a unique professional lens. 

## 🚀 Features Implemented
- **User Idea Input:** Form to capture Startup Name, Problem Statement, Solution, Revenue Model, Target Audience, and Funding Ask.
- **4 AI Judges (Sharks):**
  - 🦈 **Venture Capitalist:** Focuses heavily on market size and scalability.
  - 🦈 **Founder:** Critiques the execution plan and operational realities.
  - 🦈 **Customer:** Evaluates actual usefulness and user value proposition.
  - 🦈 **Angel Investor:** Prioritizes early profitability and margins.
- **Dynamic Pitch Round:** Interactive Q&A where judges ask specific questions based on the input data, allowing user responses.
- **Scorecard System:** Provides a visual breakdown (out of 100) across Market Potential, Innovation, Business Model, Execution, and Investment Worthiness.
- **Investment Decisions:** Outputs real-time outcomes (Invest, Reject, Acquire, or Come Back Later) alongside suggested valuations, funding amounts, and deep reasoning.
- **UI & Experience:** Dark theme layout featuring animated cards, success confetti on funding wins, PDF pitch report downloads, and a leaderboard.

## 📸 Screenshots & Proof
*Place your simulation and final scorecard screenshots here*
- ![Simulation UI](screenshots/simulation_ui.png)
- ![Investment Scorecard](screenshots/scorecard_results.png)

## 💡 Key Learnings
1. **Investor Frameworks:** Gained a clearer understanding of how different archetypes evaluate risk, traction, and scaling potential.
2. **Interactive State Management:** Learned how to build multi-stage user workflows (Input -> Pitch -> Q&A -> Results) inside a single, self-contained HTML file.
3. **Prompt Engineering for Dynamic UI:** Experienced how setting clear personality constraints on LLMs translates into fun, realistic game mechanics.