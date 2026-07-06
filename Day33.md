# Day 33: Media Literacy with Claude - Build a Media Integrity Analyzer

## 📋 Project Overview
The **Media Integrity Analyzer** is an interactive, single-file HTML educational application designed to teach media literacy through discovery rather than memorization. It guides users through evaluating misleading headlines, recognizing emotional manipulation, and understanding how content bias impacts perception.

## 🚀 Features Built
*   **Theme Customization:** Allows users to choose an editorial color palette (including a premium Claude Orange dark theme).
*   **Challenge 1 (Headline Detective):** Evaluates user interaction with fictional news stories, highlighting mismatches between sensationalist headlines and underlying facts.
*   **Challenge 2 (Emotion Detector):** Pinpoints emotionally charged language used in social media updates to uncover hidden manipulation techniques.
*   **Live Metrics Tracking:** Displays dynamic updates for *Headline Accuracy*, *Source Reliability*, *Emotional Manipulation*, and *Audience Targeting*.
*   **Media Integrity Dashboard:** Generates an overall competency score, key red flags, and three practical daily habits for healthier media consumption.

---

## 📸 Application Screenshots

### 1. Theme Selection & Introduction
![Theme Selection and Intro](screenshots/intro_theme.png)

### 2. Headline Detective Challenge
![Headline Detective](screenshots/headline_challenge.png)

### 3. Emotion Detector Challenge
![Emotion Detector](screenshots/emotion_challenge.png)

### 4. Final Media Integrity Dashboard
![Final Dashboard](screenshots/final_dashboard.png)

---

## 💡 Key Learnings

### 1. The Power of Guided Discovery
Teaching media literacy is far more effective when users uncover biases themselves. By asking *"Would you click this?"* and letting them flag emotional triggers before revealing the breakdown, the application shifts from a boring lecture into an eye-opening experiment.

### 2. Spotting Emotional Triggers
Content designed to go viral frequently exploits high-arousal emotions like anger, fear, or validation. Isolating specific loaded phrases and converting them into a neutral rewrite clearly demonstrates how facts get buried beneath sensationalist framing.

### 3. Creating Component-Free Single-File Apps
Building a highly interactive dashboard using strictly vanilla CSS and JavaScript (without relying on external frameworks like Tailwind or React) reinforced clean state management practices. Everything operates offline instantly through simple event handling and CSS variable switching.

---

## 🛠️ How to Run the App
1. Clone this repository or download the `media_integrity_analyzer.html` file from this folder.
2. Double-click the file to open it directly in any modern web browser.
3. Select your favorite theme and step through the challenges!