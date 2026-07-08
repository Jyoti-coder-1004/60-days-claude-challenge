# Day 36: Cognitive Pattern Explorer

## Project Overview
The **Cognitive Pattern Explorer** is a psychology-inspired, single-file offline web app designed to help individuals explore their thinking tendencies and decision-making styles through interactive scenarios. Built purely with vanilla web technologies, the application avoids clinical diagnosis, focusing instead on reflective self-discovery across five primary profiles: Analytical Thinker, Emotional Intuitive, Overthinking Loop Style, Action-First Decision Maker, and Balanced Reflective Thinker.

## Key Features
- **Dual Mood Atmosphere:** Choose between a Calm and Stress mode to alter the visual landscape and observe how environmental tension impacts your choices.
- **Chapter-Based Progression:** 
  - *Chapter 1 (Discovery):* Contextual scenario-based multiple-choice exploration.
  - *Chapter 2 (Priorities):* Interactive drag-and-drop card sorting to align values.
  - *Chapter 3 (Timeline Mapping):* Draggable timeline placement to map decision sequences under pressure.
- **Dynamic Reflection Journal:** Aggregates selections to output an insightful, percentage-based breakdown of your dominant cognitive patterns alongside personalized advice.
- **Offline & Responsive:** Built completely using standard HTML5, CSS3 transitions, and vanilla JavaScript with native touch-fallback support.

## Tech Stack
- **Frontend:** Semantic HTML5, CSS3 Custom Properties (Variables), and Flexbox/Grid layouts.
- **Logic:** Vanilla JavaScript (ES6+) for state tracking, native Drag-and-Drop API handling, and scoring evaluation.
- **Accessibility:** Keyboard navigable inputs, high-contrast focus indicators, and strict alignment with `prefers-reduced-motion` media queries.

## Lessons Learned
- Implementing reliable touch fallbacks for native HTML5 drag-and-drop interfaces without resorting to external framework abstraction layers.
- Structuring adaptive scoring mechanisms using pure JavaScript object configurations to map multi-layered user pathways cleanly.
- Balancing game-like interactive mechanics with calm, mindful UX architecture to foster genuine self-reflection.