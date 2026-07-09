# Day 26: Prior Authorization Workflow Simulator 🏥⚙️

## 🎯 Overview
Today, I built a complete, production-quality **Prior Authorization Workflow Simulator** using Claude. This gamified, drag-and-drop web application models real-world healthcare administration processes, showing how patients, medical providers, and insurance payers interact during medical necessity reviews.

## 🚀 Features Implemented
- **Three-Lane Workflow Grid:** Clean structural layout separating the **Patient**, **Provider**, and **Payer** operational lanes.
- **Interactive Drag-and-Drop:** Seamless client-side card manipulation moving patient cases across different sequential review stages.
- **Dynamic Patient Scenarios:** Built-in simulation case profiles covering an elective surgery, an MRI scan, a specialty medication requirement, and an acute inpatient admission.
- **Document Assembler:** Interactive sub-steps allowing users to collect clinical charts, provider notes, and specific medical documentation before submission.
- **Payer Review Engine:** Replicates industry outcomes including Approval, Pend (requesting more data), Denial, and Peer-to-Peer administrative appeals.
- **Metrics Trackers:** Real-time counters showing a Days Elapsed clock, a procedural Efficiency Score, and a live tracking banner across the top.
- **Modern UI & UX:** Clean blue healthcare-themed interface featuring inline educational tooltips after every operational step and a success celebration animation upon final approval.

## 📸 Screenshots & Proof
*Place your workflow simulation and final scorecard screenshots here*
- ![Simulation Board UI](screenshots/simulation_ui.png)
- ![Workflow Completed Dashboard](screenshots/workflow_summary.png)

## 💡 Key Learnings
1. **Healthcare Operations:** Gained a deep understanding of the structural checkpoints, friction points, and document requirements involved in US healthcare utilization management.
2. **Drag-and-Drop APIs:** Implemented native HTML5 Drag and Drop event handlers seamlessly within a self-contained single-file state framework.
3. **Gamified Educational Architecture:** Experienced how translating a dry, highly bureaucratic business workflow into an interactive simulator makes complex compliance protocols significantly easier to master.