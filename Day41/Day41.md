# Day 41: Interactive Learning Studio — JavaScript Asynchronous Architecture

This document contains the complete layout, architectural system prompt rules, and the full single-file production source code for the Day 41 curriculum application.

---

## 1. Complete Application Source Code (`index.html`)

Save this code block as an `index.html` file to launch the premium, dark-mode, zero-dependency Interactive Learning Studio.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Mastering JS Asynchronous Architecture // Learning Studio</title>
    <style>
        :root {
            --bg-main: #0b0f19;
            --bg-card: #131a2c;
            --bg-input: #1e293b;
            --text-main: #f8fafc;
            --text-muted: #94a3b8;
            --accent: #3b82f6;
            --accent-gradient: linear-gradient(135deg, #3b82f6 0%, #8b5cf6 100%);
            --success: #10b981;
            --warning: #f59e0b;
            --danger: #ef4444;
            --border: #2d3748;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        body {
            background-color: var(--bg-main);
            color: var(--text-main);
            min-height: 100vh;
            padding: 2rem;
            display: flex;
            flex-direction: column;
            gap: 2rem;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            border-bottom: 1px solid var(--border);
            padding-bottom: 1.5rem;
        }

        h1 {
            font-size: 1.75rem;
            font-weight: 800;
            background: var(--accent-gradient);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            letter-spacing: -0.03em;
        }

        .studio-container {
            display: grid;
            grid-template-columns: 280px 1fr;
            gap: 2rem;
        }

        @media (max-width: 900px) {
            .studio-container {
                grid-template-columns: 1fr;
            }
        }

        /* Sidebar Navigation */
        .sidebar {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 1.5rem;
            display: flex;
            flex-direction: column;
            gap: 1rem;
            height: fit-content;
        }

        .nav-item {
            padding: 0.75rem 1rem;
            border-radius: 8px;
            cursor: pointer;
            font-weight: 600;
            color: var(--text-muted);
            transition: all 0.2s;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }

        .nav-item.active {
            background: var(--accent-gradient);
            color: white;
        }

        .nav-item.locked {
            cursor: not-allowed;
            opacity: 0.5;
        }

        .nav-badge {
            font-size: 0.75rem;
            padding: 0.2rem 0.5rem;
            border-radius: 999px;
            background: rgba(0,0,0,0.2);
            color: #fff;
        }

        /* Content Panel */
        .content-panel {
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 16px;
            padding: 2.5rem;
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.3);
        }

        .module-section {
            display: none;
            flex-direction: column;
            gap: 1.5rem;
        }

        .module-section.active {
            display: flex;
        }

        p {
            line-height: 1.7;
            color: #cbd5e1;
            font-size: 1.05rem;
        }

        .diagram {
            background: var(--bg-input);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 1.5rem;
            display: flex;
            justify-content: space-around;
            align-items: center;
            flex-wrap: wrap;
            gap: 1rem;
            margin: 1rem 0;
        }

        .diagram-box {
            padding: 1rem;
            border-radius: 8px;
            background: var(--bg-card);
            border: 2px solid var(--border);
            min-width: 120px;
            text-align: center;
            font-weight: bold;
        }

        .diagram-box.highlight {
            border-color: var(--accent);
            box-shadow: 0 0 15px rgba(59, 130, 246, 0.4);
        }

        code {
            font-family: 'Courier New', Courier, monospace;
            background: rgba(255, 255, 255, 0.1);
            padding: 0.2rem 0.4rem;
            border-radius: 4px;
            color: #f472b6;
        }

        pre {
            background: var(--bg-main);
            padding: 1.25rem;
            border-radius: 8px;
            border: 1px solid var(--border);
            overflow-x: auto;
            margin: 1rem 0;
        }

        pre code {
            background: transparent;
            color: #38bdf8;
            padding: 0;
        }

        /* Quiz UI */
        .quiz-container {
            background: var(--bg-input);
            border: 1px solid var(--border);
            border-radius: 12px;
            padding: 2rem;
            margin-top: 2rem;
        }

        .quiz-title {
            font-size: 1.25rem;
            font-weight: 700;
            margin-bottom: 1rem;
            color: var(--text-main);
        }

        .option-btn {
            display: block;
            width: 100%;
            text-align: left;
            padding: 1rem;
            background: var(--bg-card);
            border: 1px solid var(--border);
            border-radius: 8px;
            color: var(--text-main);
            margin-bottom: 0.75rem;
            cursor: pointer;
            transition: all 0.2s;
            font-size: 1rem;
        }

        .option-btn:hover:not(:disabled) {
            border-color: var(--accent);
            background: rgba(59, 130, 246, 0.05);
        }

        .option-btn.correct {
            background: rgba(16, 185, 129, 0.2) !important;
            border-color: var(--success) !important;
        }

        .option-btn.wrong {
            background: rgba(239, 68, 68, 0.2) !important;
            border-color: var(--danger) !important;
        }

        .feedback-box {
            padding: 1rem;
            border-radius: 8px;
            margin-top: 1rem;
            font-weight: 500;
            display: none;
        }

        .btn-primary {
            background: var(--accent-gradient);
            color: white;
            border: none;
            border-radius: 8px;
            padding: 0.75rem 1.5rem;
            font-weight: 700;
            cursor: pointer;
            margin-top: 1rem;
        }

        .btn-primary:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }
    </style>
</head>
<body>

    <header>
        <div>
            <h1>Interactive Learning Studio</h1>
            <p style="color: var(--text-muted); font-size: 0.9rem; margin-top: 0.25rem;">Topic: JavaScript Asynchronous Engine & Architecture</p>
        </div>
        <div style="text-align: right;">
            <div id="global-progress" style="font-weight: 700; font-size: 1.1rem; color: var(--success);">Progress: 0%</div>
        </div>
    </header>

    <main class="studio-container">
        <!-- Syllabus Sidebar Map -->
        <nav class="sidebar" id="sidebarNav">
            <div class="nav-item active" onclick="switchTab('intro')">Introduction</div>
            <div class="nav-item locked" id="nav-m1" onclick="switchTab('m1')">Module 1: Call Stack <span class="nav-badge" id="b1">Locked</span></div>
            <div class="nav-item locked" id="nav-m2" onclick="switchTab('m2')">Module 2: Web APIs <span class="nav-badge" id="b2">Locked</span></div>
            <div class="nav-item locked" id="nav-m3" onclick="switchTab('m3')">Module 3: Task Queues <span class="nav-badge" id="b3">Locked</span></div>
            <div class="nav-item locked" id="nav-m4" onclick="switchTab('m4')">Module 4: The Event Loop <span class="nav-badge" id="b4">Locked</span></div>
            <div class="nav-item locked" id="nav-conclusion" onclick="switchTab('conclusion')">Mastery Review <span class="nav-badge" id="b5">Locked</span></div>
        </nav>

        <!-- Core Instruction Interface -->
        <section class="content-panel">
            
            <!-- Introduction Section -->
            <div id="sec-intro" class="module-section active">
                <h2>Welcome to Asynchronous Core Mastery</h2>
                <p style="margin-top: 1rem;">JavaScript is inherently single-threaded, yet handles complex networking, rendering tasks, and animations flawlessly. This studio dissects the underlying runtime engine mechanics behind this behavior.</p>
                
                <div style="background: var(--bg-input); padding: 1.5rem; border-radius: 12px; margin: 1.5rem 0; border: 1px solid var(--border);">
                    <h4 style="margin-bottom: 0.5rem;">🎯 Learning Parameters:</h4>
                    <ul style="margin-left: 1.5rem; color: var(--text-muted); line-height: 1.6;">
                        <li><strong>Estimated Time:</strong> 25 Minutes</li>
                        <li><strong>Prerequisite:</strong> Execution contexts and variable scope understanding.</li>
                        <li><strong>Target Outcome:</strong> Zero guesswork execution-order prediction of Promise cascades.</li>
                    </ul>
                </div>

                <button class="btn-primary" onclick="unlockModule('m1')">Initialize Studio & Start Module 1</button>
            </div>

            <!-- Module 1 -->
            <div id="sec-m1" class="module-section">
                <h2>Module 1: The Execution Call Stack</h2>
                <p>The Call Stack follows LIFO (Last In, First Out) processing rules. Every function invocation creates an execution context frame pushed straight to the stack summit.</p>
                
                <div class="diagram">
                    <div class="diagram-box">Global Context</div>
                    <div class="diagram-box">OuterFunc()</div>
                    <div class="diagram-box highlight">InnerFunc() [Active]</div>
                </div>

                <pre><code>function multiply(a, b) { return a * b; }
function square(n) { return multiply(n, n); }
square(4);</code></pre>

                <div class="quiz-container" id="quiz-m1">
                    <div class="quiz-title">Checkpoint Question: What happens if code allocation exceeds stack depth limitations