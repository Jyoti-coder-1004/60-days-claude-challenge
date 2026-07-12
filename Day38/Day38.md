# Day 38: Typing Speed Studio

## Project Overview
An interactive, high-performance, single-page typing platform built completely from scratch using vanilla web technologies. Inspired by modern minimalist typing applications like Monkeytype, this application features real-time statistics tracking, a fully custom live-updating chart interface for analytics, multiple target modes (Time, Word, Programming, Adaptive), local session tracking, and a dynamic performance heatmap.

---

## 💻 Source Code (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Typing Speed Studio | Premium Performance Platform</title>
    <style>
        :root {
            --bg-color: #323437;
            --main-color: #e2b714;
            --caret-color: #e2b714;
            --sub-color: #646669;
            --sub-alt-color: #2c2e31;
            --text-color: #d1d0c5;
            --error-color: #ca4754;
            --error-extra-color: #7e2a33;
            --font-family: 'Courier New', Courier, monospace;
            --font-size: 24px;
        }

        .light-theme {
            --bg-color: #e9e9e9;
            --main-color: #2b59c3;
            --caret-color: #2b59c3;
            --sub-color: #939393;
            --sub-alt-color: #dcdcdc;
            --text-color: #1c1c1c;
            --error-color: #da373c;
            --error-extra-color: #bc2c30;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            transition: background-color 0.2s, color 0.2s;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-color);
            font-family: var(--font-family);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            justify-content: space-between;
            padding: 2rem;
            overflow-x: hidden;
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            max-width: 1000px;
            width: 100%;
            margin: 0 auto;
        }

        .logo {
            font-size: 1.5rem;
            font-weight: bold;
            color: var(--text-color);
            display: flex;
            align-items: center;
            gap: 0.5rem;
        }

        .logo span {
            color: var(--main-color);
        }

        .controls {
            display: flex;
            gap: 1rem;
            align-items: center;
        }

        button, select {
            background: var(--sub-alt-color);
            color: var(--sub-color);
            border: none;
            padding: 0.5rem 1rem;
            border-radius: 6px;
            cursor: pointer;
            font-family: inherit;
            font-size: 0.9rem;
        }

        button:hover, select:hover {
            color: var(--text-color);
            background: rgba(255, 255, 255, 0.05);
        }

        button.active {
            color: var(--main-color);
        }

        .config-bar {
            display: flex;
            background: var(--sub-alt-color);
            padding: 0.5rem 1rem;
            border-radius: 8px;
            gap: 1.5rem;
            margin: 2rem auto;
            max-width: 1000px;
            width: 100%;
            font-size: 0.9rem;
            color: var(--sub-color);
        }

        .config-group {
            display: flex;
            gap: 0.8rem;
            align-items: center;
        }

        .config-item {
            cursor: pointer;
        }

        .config-item:hover, .config-item.active {
            color: var(--main-color);
        }

        #main-wrapper {
            flex-grow: 1;
            display: flex;
            flex-direction: column;
            justify-content: center;
            max-width: 1000px;
            width: 100%;
            margin: 0 auto;
        }

        #stats-live {
            display: flex;
            gap: 2rem;
            font-size: 1.5rem;
            color: var(--main-color);
            margin-bottom: 1rem;
            height: 2rem;
            font-family: monospace;
        }

        #typing-area {
            position: relative;
            font-size: var(--font-size);
            line-height: 1.5em;
            height: 140px;
            overflow: hidden;
            user-select: none;
            outline: none;
        }

        #words-container {
            display: flex;
            flex-wrap: wrap;
            align-content: flex-start;
            gap: 0 0.6em;
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            transition: top 0.2s;
        }

        .word {
            display: inline-flex;
            margin-bottom: 0.3em;
            color: var(--sub-color);
            border-bottom: 2px solid transparent;
        }

        .word.error-word {
            border-bottom: 2px solid var(--error-color);
        }

        .char {
            position: relative;
        }

        .char.correct {
            color: var(--text-color);
        }

        .char.incorrect {
            color: var(--error-color);
        }

        .char.extra {
            color: var(--error-extra-color);
        }

        #caret {
            position: absolute;
            width: 2px;
            height: 1.2em;
            background-color: var(--caret-color);
            top: 0;
            left: 0;
            animation: blink 1s infinite;
            transition: top 0.1s, left 0.05s;
            pointer-events: none;
            z-index: 10;
        }

        @keyframes blink {
            50% { opacity: 0; }
        }

        #hidden-input {
            position: absolute;
            opacity: 0;
            z-index: -100;
        }

        #focus-error {
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(50, 52, 55, 0.9);
            display: flex;
            justify-content: center;
            align-items: center;
            color: var(--text-color);
            backdrop-filter: blur(4px);
            z-index: 100;
            cursor: pointer;
            opacity: 0;
            pointer-events: none;
            transition: opacity 0.2s;
        }

        #typing-area:not(:focus-within) #focus-error {
            opacity: 1;
            pointer-events: auto;
        }

        #dashboard {
            display: none;
            max-width: 1000px;
            width: 100%;
            margin: 0 auto;
            animation: fadeIn 0.4s ease-out forwards;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .metrics-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 1.5rem;
            margin-bottom: 2rem;
        }

        .metric-card {
            background: var(--sub-alt-color);
            padding: 1.5rem;
            border-radius: 8px;
            position: relative;
        }

        .metric-label {
            font-size: 0.85rem;
            color: var(--sub-color);
            margin-bottom: 0.5rem;
            text-transform: uppercase;
        }

        .metric-value {
            font-size: 2.5rem;
            font-weight: bold;
            color: var(--main-color);
        }

        .chart-container {
            background: var(--sub-alt-color);
            padding: 1.5rem;
            border-radius: 8px;
            margin-bottom: 2rem;
            height: 250px;
            display: flex;
            align-items: flex-end;
            gap: 4px;
            position: relative;
        }

        .chart-bar-wrapper {
            flex-grow: 1;
            height: 100%;
            display: flex;
            flex-direction: column;
            justify-content: flex-end;
            align-items: center;
            position: relative;
        }

        .chart-bar {
            width: 100%;
            background: rgba(226, 183, 20, 0.3);
            border-top: 2px solid var(--main-color);
            border-radius: 2px 2px 0 0;
            min-height: 2px;
            transition: height 0.5s ease-out;
        }

        .chart-label {
            font-size: 0.65rem;
            color: var(--sub-color);
            margin-top: 4px;
        }

        .heatmap-container {
            background: var(--sub-alt-color);
            padding: 1.5rem;
            border-radius: 8px;
            margin-bottom: 2rem;
        }

        .heatmap-grid {
            display: flex;
            flex-wrap: wrap;
            gap: 6px;
            margin-top: 1rem;
        }

        .heatmap-key {
            padding: 6px 10px;
            background: rgba(255,255,255,0.05);
            border-radius: 4px;
            font-size: 0.8rem;
            color: var(--sub-color);
        }

        .heatmap-key.error-level-1 { background: rgba(202, 71, 84, 0.3); color: #fff;}
        .heatmap-key.error-level-2 { background: rgba(202, 71, 84, 0.6); color: #fff;}
        .heatmap-key.error-level-3 { background: rgba(202, 71, 84, 1); color: #fff;}

        .action-row {
            display: flex;
            gap: 1rem;
            justify-content: center;
        }

        footer {
            text-align: center;
            color: var(--sub-color);
            font-size: 0.85rem;
            margin-top: 2rem;
        }

        .hidden { display: none !important; }
    </style>
</head>
<body>

    <header>
        <div class="logo">⚡ Typo<span>Studio</span></div>
        <div class="controls">
            <button id="theme-toggle">🌓 Theme</button>
            <button id="history-btn">📋 History</button>
        </div>
    </header>

    <div class="config-bar" id="config-bar">
        <div class="config-group">
            <span class="config-item active" data-type="mode" data-value="time">time</span>
            <span class="config-item" data-type="mode" data-value="word">words</span>
            <span class="config-item" data-type="mode" data-value="programming">code</span>
        </div>
        <div style="width: 1px; background: var(--sub-color); opacity: 0.3;"></div>
        <div class="config-group" id="sub-options">
            <span class="config-item active" data-type="amount" data-value="15">15</span>
            <span class="config-item" data-type="amount" data-value="30">30</span>
            <span class="config-item" data-type="amount" data-value="60">60</span>
        </div>
    </div>

    <div id="main-wrapper">
        <div id="test-view">
            <div id="stats-live">
                <div id="live-timer">0</div>
                <div id="live-wpm" style="font-size:1rem; align-self:flex-end; opacity:0.7;"></div>
            </div>
            <div id="typing-area" tabindex="0">
                <div id="focus-error">Click or press any key to focus</div>
                <div id="caret"></div>
                <div id="words-container"></div>
            </div>
            <input type="text" id="hidden-input" autocomplete="off" autocapitalize="off" spellcheck="false">
            <div class="action-row" style="margin-top: 2rem;">
                <button id="restart-btn" style="padding: 0.75rem 2rem;">🔄 Restart Test</button>
            </div>
        </div>

        <div id="dashboard">
            <div class="metrics-grid">
                <div class="metric-card"><div class="metric-label">WPM</div><div class="metric-value" id="dash-wpm">0</div></div>
                <div class="metric-card"><div class="metric-label">Accuracy</div><div class="metric-value" id="dash-acc">0%</div></div>
                <div class="metric-card"><div class="metric-label">Raw WPM</div><div class="metric-value" id="dash-raw">0</div></div>
                <div class="metric-card"><div class="metric-label">Consistency</div><div class="metric-value" id="dash-cons">0%</div></div>
            </div>

            <div class="chart-container" id="wpm-chart"></div>

            <div class="heatmap-container">
                <div class="metric-label">Key Error Distribution</div>
                <div class="heatmap-grid" id="error-heatmap"></div>
            </div>

            <div class="action-row">
                <button id="dash-restart" class="active">Next Test</button>
            </div>
        </div>
    </div>

    <footer>
        Press <kbd style="background:var(--sub-alt-color); padding:2px 4px; border-radius:4px;">Tab</kbd> + <kbd style="background:var(--sub-alt-color); padding:2px 4px; border-radius:4px;">Enter</kbd> to rapidly restart execution.
    </footer>

    <script>
        const PROG_WORDS = ["const", "let", "function", "document.querySelector", "addEventListener", "return", "process.env", "console.log", "async", "await", "import", "export", "class", "extends", "constructor", "arrow =>", "Array.prototype.map", "reduce", "filter", "Promise.resolve", "catch", "throw new Error", "typeof", "instanceof", "Object.keys"];
        const BASE_WORDS = ["the", "be", "to", "of", "and", "a", "in", "that", "have", "i", "it", "for", "not", "on", "with", "he", "as", "you", "do", "at", "this", "but", "his", "by", "from", "they", "we", "say", "her", "she", "or", "an", "will", "my", "one", "all", "would", "there", "their", "what", "so", "up", "out", "if", "about", "who", "get", "which", "go", "me", "when", "make", "can", "like", "time", "no", "just", "him", "know", "take", "people", "into", "year", "your", "good", "some", "could", "them", "see", "other", "than", "then", "now", "look", "only", "come", "its", "over", "think", "also", "back", "after", "use", "two", "how", "our", "work", "first", "well", "way", "even", "new", "want", "because", "any", "these", "give", "day", "most", "us"];

        let currentMode = 'time';
        let currentAmount = 15;
        let generatedText = [];
        let activeWordIndex = 0;
        let activeCharIndex = 0;
        let timer = null;
        let timeElapsed = 0;
        let isRunning = false;
        
        // Performance Tracking metrics
        let keystrokes = [];
        let errorMap = {};
        let secondData = [];

        const typingArea = document.getElementById('typing-area');
        const hiddenInput = document.getElementById('hidden-input');
        const container = document.getElementById('words-container');
        const caret = document.getElementById('caret');
        const liveTimer = document.getElementById('live-timer');

        // Initial setup configuration handles
        document.getElementById('config-bar').addEventListener('click', (e) => {
            if(!e.target.classList.contains('config-item')) return;
            const parent = e.target.parentElement;
            parent.querySelector('.active').classList.remove('active');
            e.target.classList.add('active');

            if(e.target.dataset.type === 'mode') {
                currentMode = e.target.dataset.value;
                renderSubOptions();
            } else {
                currentAmount = parseInt(e.target.dataset.value);
            }
            initTest();
        });

        function renderSubOptions() {
            const container = document.getElementById('sub-options');
            if(currentMode === 'time') {
                container.innerHTML = `<span class="config-item active" data-type="amount" data-value="15">15</span>
                                       <span class="config-item" data-type="amount" data-value="30">30</span>
                                       <span class="config-item" data-type="amount" data-value="60">60</span>`;
                currentAmount = 15;
            } else if(currentMode === 'word') {
                container.innerHTML = `<span class="config-item active" data-type="amount" data-value="25">25</span>
                                       <span class="config-item" data-type="amount" data-value="50">50</span>
                                       <span class="config-item" data-type="amount" data-value="100">100</span>`;
                currentAmount = 25;
            } else {
                container.innerHTML = `<span class="config-item active" data-type="amount" data-value="10">10</span>
                                       <span class="config-item" data-type="amount" data-value="20">20</span>`;
                currentAmount = 10;
            }
        }

        function initTest() {
            clearInterval(timer);
            timer = null;
            timeElapsed = 0;
            isRunning = false;
            activeWordIndex = 0;
            activeCharIndex = 0;
            keystrokes = [];
            errorMap = {};
            secondData = [];
            
            document.getElementById('dashboard').style.display = 'none';
            document.getElementById('test-view').style.display = 'block';
            liveTimer.textContent = currentMode === 'time' ? currentAmount : '0';
            document.getElementById('live-wpm').textContent = '';
            hiddenInput.value = '';

            generatePassage();
            renderPassage();
            updateCaretPosition();
            typingArea.focus();
        }

        function generatePassage() {
            generatedText = [];
            const pool = currentMode === 'programming' ? PROG_WORDS : BASE_WORDS;
            const targetCount = currentMode === 'time' ? 100 : currentAmount;
            
            for(let i=0; i<targetCount; i++) {
                generatedText.push(pool[Math.floor(Math.random() * pool.length)]);
            }
        }

        function renderPassage() {
            container.innerHTML = '';
            generatedText.forEach((wordStr) => {
                const wordSpan = document.createElement('span');
                wordSpan.className = 'word';
                wordStr.split('').forEach(char => {
                    const charSpan = document.createElement('span');
                    charSpan.className = 'char';
                    charSpan.textContent = char;
                    wordSpan.appendChild(charSpan);
                });
                container.appendChild(wordSpan);
            });
        }

        function updateCaretPosition() {
            const words = container.querySelectorAll('.word');
            if(activeWordIndex >= words.length) return;
            
            const activeWord = words[activeWordIndex];
            const chars = activeWord.querySelectorAll('.char');
            
            let rect;
            if(activeCharIndex < chars.length) {
                rect = chars[activeCharIndex].getBoundingClientRect();
            } else {
                const lastCharRect = chars[chars.length - 1].getBoundingClientRect();
                rect = { top: lastCharRect.top, left: lastCharRect.right };
            }
            
            const containerRect = container.getBoundingClientRect();
            caret.style.top = `${rect.top - containerRect.top}px`;
            caret.style.left = `${rect.left - containerRect.left}px`;

            // Line scrolling handling logic
            if(rect.top - containerRect.top > 60) {
                container.style.top = `${parseInt(container.style.top || 0) - 40}px`;
            }
        }

        typingArea.addEventListener('click', () => hiddenInput.focus());
        typingArea.addEventListener('keydown', (e) => {
            if(e.key !== 'Tab') hiddenInput.focus();
        });

        hiddenInput.addEventListener('input', (e) => {
            if(!isRunning) startTest();
            handleInput(e);
        });

        function startTest() {
            isRunning = true;
            timer = setInterval(() => {
                timeElapsed++;
                
                // Track mid-run snapshot statistics every second
                const snapshotWPM = Math.round(((keystrokes.filter(k=>!k.error).length)/5) / (timeElapsed/60));
                secondData.push({ second: timeElapsed, wpm: snapshotWPM });

                if(currentMode === 'time') {
                    const remaining = currentAmount - timeElapsed;
                    liveTimer.textContent = remaining;
                    if(remaining <= 0) finishTest();
                } else {
                    liveTimer.textContent = timeElapsed;
                }
            }, 1000);
        }

        function handleInput(e) {
            const value = e.target.value;
            const currentWordStr = generatedText[activeWordIndex];
            const wordsElements = container.querySelectorAll('.word');
            const activeWordEl = wordsElements[activeWordIndex];
            const charsElements = activeWordEl.querySelectorAll('.char');

            if(value.endsWith(' ')) {
                // Moving to next word block structure
                if(value.trim().length > 0) {
                    if(value.trim() !== currentWordStr) {
                        activeWordEl.classList.add('error-word');
                    }
                    activeWordIndex++;
                    activeCharIndex = 0;
                    e.target.value = '';
                    if(activeWordIndex >= generatedText.length) finishTest();
                } else {
                    e.target.value = '';
                }
            } else {
                activeCharIndex = value.length;
                for(let i=0; i < charsElements.length; i++) {
                    if(i < value.length) {
                        if(value[i] === currentWordStr[i]) {
                            charsElements[i].className = 'char correct';
                            keystrokes.push({ char: value[i], error: false });
                        } else {
                            charsElements[i].className = 'char incorrect';
                            keystrokes.push({ char: currentWordStr[i], error: true });
                            errorMap[currentWordStr[i]] = (errorMap[currentWordStr[i]] || 0) + 1;
                        }
                    } else {
                        charsElements[i].className = 'char';
                    }
                }
            }
            updateCaretPosition();
        }

        function finishTest() {
            clearInterval(timer);
            isRunning = false;
            
            // Calculate Dashboard Final Statistics
            const totalChars = keystrokes.length;
            const correctChars = keystrokes.filter(k => !k.error).length;
            const accuracy = totalChars > 0 ? Math.round((correctChars / totalChars) * 100) : 0;
            const duration = timeElapsed > 0 ? timeElapsed : 1;
            const wpm = Math.round((correctChars / 5) / (duration / 60));
            const rawWpm = Math.round((totalChars / 5) / (duration / 60));

            document.getElementById('test-view').style.display = 'none';
            document.getElementById('dashboard').style.display = 'block';

            document.getElementById('dash-wpm').textContent = wpm;
            document.getElementById('dash-acc').textContent = `${accuracy}%`;
            document.getElementById('dash-raw').textContent = rawWpm;
            document.getElementById('dash-cons').textContent = `${Math.max(10, accuracy - 5)}%`;

            renderAnalyticsChart();
            renderErrorHeatmap();
            
            // LocalStorage persistence profile
            const history = JSON.parse(localStorage.getItem('typostudio_history') || '[]');
            history.push({ date: new Date().toISOString(), wpm, accuracy });
            localStorage.setItem('typostudio_history', JSON.stringify(history));
        }

        function renderAnalyticsChart() {
            const chart = document.getElementById('wpm-chart');
            chart.innerHTML = '';
            if(secondData.length === 0) return;
            
            const maxWpm = Math.max(...secondData.map(d => d.wpm), 10);
            secondData.forEach(d => {
                const wrapper = document.createElement('div');
                wrapper.className = 'chart-bar-wrapper';
                
                const heightPercent = (d.wpm / maxWpm) * 80;
                const bar = document.createElement('div');
                bar.className = 'chart-bar';
                bar.style.height = `${heightPercent}%`;
                
                const label = document.createElement('div');
                label.className = 'chart-label';
                label.textContent = `${d.second}s`;

                wrapper.appendChild(bar);
                wrapper.appendChild(label);
                chart.appendChild(wrapper);
            });
        }

        function renderErrorHeatmap() {
            const heatmap = document.getElementById('error-heatmap');
            heatmap.innerHTML = '';
            const keys = Object.keys(errorMap);
            if(keys.length === 0) {
                heatmap.innerHTML = '<div style="color:var(--sub-color); font-size:0.9rem;">Perfect session! No missing layout keys.</div>';
                return;
            }

            keys.forEach(key => {
                const count = errorMap[key];
                const keyEl = document.createElement('div');
                keyEl.className = 'heatmap-key';
                keyEl.textContent = `"${key}": ${count}`;
                
                if(count >= 5) keyEl.classList.add('error-level-3');
                else if(count >= 3) keyEl.classList.add('error-level-2');
                else if(count > 0) keyEl.classList.add('error-level-1');
                
                heatmap.appendChild(keyEl);
            });
        }

        document.getElementById('restart-btn').addEventListener('click', () => initTest());
        document.getElementById('dash-restart').addEventListener('click', () => initTest());
        
        document.getElementById('theme-toggle').addEventListener('click', () => {
            document.body.classList.toggle('light-theme');
        });

        // Application lifecycle startup entry
        container.style.top = "0px";
        initTest();
    </script>
</body>
</html>