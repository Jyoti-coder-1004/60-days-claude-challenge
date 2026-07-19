# Day 46: Build Autonomous Agent Studio

## Prompt Template
You are an expert AI systems architect, agentic workflow designer, prompt engineer, automation engineer, conversation designer, UX designer, and senior frontend developer. Your task is to guide me through designing an autonomous multi-agent system and then generate the production-ready application.

### RULES FOR THIS SESSION:
- Conduct the interview first, asking exactly ONE question at a time.
- All questions must be Multiple Choice Questions (MCQ) only, offering clear structural options, with a final "Other" text option if I want to customize.
- Wait for my choice before asking the next question.

### THE INTERVIEW WORKFLOW:
1. **Question 1:** "What kind of autonomous AI agent should we build? (Select a domain/use case)"
2. **Question 2:** [Narrow follow-up based on the chosen domain to make the workflow specific enough to automate — do not stop at just a high-level domain].
3. **Question 3:** "What is the primary success criteria for this loop? (e.g., accuracy, execution speed, quality score, safety compliance, stakeholder approval)"
4. **Question 4:** "What should be the primary stopping condition for the autonomous loop? (Select an option suited to the chosen workflow)"
5. **Question 5:** "Would you like to auto-design the architecture or customize the individual components? (If customize, ask which specific agents to include)."

---

### FINAL ARTIFACT GENERATION
After the interview is complete, build a single-page HTML application titled **"Autonomous Agent Studio"** that runs a real multi-agent orchestration pipeline live against the Claude API (`fetch` to `https://api.anthropic.com/v1/messages`, no key hardcoded, prompt user for it). The loop must handle planning, executing, evaluating, remembering, improving, and repeating until a stopping condition is met.

#### === AGENT SELECTION & ROLES ===
Choose from the following agents based on the interview results: **Planner, Executor, Evaluator, Critic, Improver, Memory Manager, Safety Monitor, Final Reviewer**. Provide a production-quality, deeply detailed system prompt embedded in the JS for each chosen agent.

#### === SYSTEM LOOP CONSTRAINTS (NON-NEGOTIABLE) ===
1. **Real Runtime Loop:** Implement the iterative optimization portion as an actual JavaScript `while` or `for` loop that calls Evaluator → Critic → Improver on each pass with a **live API call every time**. No pre-recorded sequences or static round limits. The total rounds must be a runtime result of the stop check.
2. **Literal API Responses:** Every metric, score, critique, and reasoning string shown in the UI must be the literal text extracted directly from that specific round's API response. No regex hacks or rule-based scoring placeholders.
3. **Forward State Threading:** Thread state explicitly forward. Each Improver call must ingest the prior round’s evaluation and critique. Each Evaluator call must ingest the new draft and the base rubric. Maintain a running `history` array containing `[score, critique, draft, delta]` for UI state persistence.
4. **Stop-Condition Check:** Check the following rules every round in exact order:
   - *Plateau:* Score improved less than a small delta for 2 consecutive rounds.
   - *Threshold:* Score crossed the target value configured during the interview.
   - *Hard Iteration Cap:* A safety fallback max-limit (not the expected endpoint).
   Log and visibly surface exactly which condition terminated the execution loop.

#### === UI/UX & DASHBOARD REQUIREMENTS ===
- **Workflow Visualization:** Draw the multi-agent orchestration loop as a true cyclic graph (showing the return loop from Improver back to Evaluator, with an explicit branch breaking off to the Final Reviewer when a stop condition fires). Do not display it as a linear pipeline.
- **Live State Panels:** Display the active agent badge, live network/processing status, open-ended iteration indicator (e.g., `"Round 3 — checking stop condition..."`), comprehensive activity log, intermediate outputs, running memory updates, and evaluation reports detailing round-over-round changes.
- **Architectural Documentation:** Embed an accordion explaining each agent's individual domain responsibility and how the stop-check logic strictly governs information flow across rounds.
- **Execution Summary:** Provide an end-of-run state block showing final output, agent performance summary, execution stats, architecture overview, extension ideas, and further prompts for engineering more advanced autonomous systems.

#### === BUILD CONSTRAINTS ===
- Single self-contained HTML file (including all CSS and JavaScript).
- Vanilla HTML/CSS/JS only. Strictly **no external libraries** (no React, Tailwind, D3, or FontAwesome).
- Commercial-grade polish: Responsive layout, dark mode by default (`#0f0f14`), smooth micro-interactions, explicit loading/retry states, graceful error catch blocks, and zero syntax errors.