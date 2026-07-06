# Day 20: AI Face Puzzle Game

A complete, single-file interactive face puzzle game built with vanilla HTML5, CSS3, and JavaScript. Features live webcam capture, dynamic grid generation, mouse/touch drag controls, responsive layouts, a live timer, and a local leaderboard.

## Source Code (`index.html`)

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>AI Face Puzzle Game</title>
    <style>
        :root {
            --bg-color: #0f172a;
            --card-bg: #1e293b;
            --primary: #6366f1;
            --primary-hover: #4f46e5;
            --success: #10b981;
            --text: #f8fafc;
            --text-muted: #94a3b8;
            --accent: #f59e0b;
        }

        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
        }

        header {
            text-align: center;
            margin-bottom: 20px;
        }

        h1 {
            font-size: 2.5rem;
            color: var(--text);
            margin-bottom: 5px;
        }

        p {
            color: var(--text-muted);
        }

        .container {
            width: 100%;
            max-width: 600px;
            background: var(--card-bg);
            border-radius: 16px;
            padding: 24px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.3);
            text-align: center;
        }

        .screen {
            display: none;
        }

        .screen.active {
            display: block;
        }

        /* Camera Screen */
        .video-container {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin: 0 auto 20px auto;
            border-radius: 12px;
            overflow: hidden;
            background: #000;
            aspect-ratio: 1/1;
        }

        video {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transform: scaleX(-1);
        }

        .btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 24px;
            font-size: 1rem;
            font-weight: 600;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s ease;
            margin: 5px;
        }

        .btn:hover {
            background-color: var(--primary-hover);
        }

        .btn-secondary {
            background-color: #475569;
        }

        .btn-secondary:hover {
            background-color: #334155;
        }

        .error-msg {
            color: #ef4444;
            margin: 15px 0;
            font-weight: 500;
        }

        /* Difficulty Screen */
        .difficulty-options {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin: 25px 0;
        }

        /* Game Screen */
        .stats-bar {
            display: flex;
            justify-content: space-between;
            background: rgba(0,0,0,0.2);
            padding: 12px;
            border-radius: 8px;
            margin-bottom: 20px;
            font-weight: 600;
            font-size: 0.95rem;
        }

        .stat-item span {
            color: var(--accent);
        }

        .puzzle-board {
            position: relative;
            width: 100%;
            max-width: 400px;
            margin: 0 auto 20px auto;
            aspect-ratio: 1/1;
            background: #000;
            border-radius: 8px;
            overflow: hidden;
            display: grid;
        }

        .tile {
            position: relative;
            box-sizing: border-box;
            cursor: grab;
            touch-action: none;
            user-select: none;
            border: 1px solid rgba(255,255,255,0.1);
            transition: border-color 0.2s ease, box-shadow 0.2s ease;
        }

        .tile.dragging {
            cursor: grabbing;
            z-index: 10;
            border: 3px solid var(--accent) !important;
            box-shadow: 0 0 15px rgba(245, 158, 11, 0.6);
        }

        .tile.correct {
            border: 2px solid var(--success);
        }

        /* Leaderboard / Win Screen */
        .results-overlay {
            margin: 20px 0;
            padding: 20px;
            background: rgba(16, 185, 129, 0.1);
            border: 2px solid var(--success);
            border-radius: 12px;
        }

        .results-overlay h2 {
            color: var(--success);
            margin-bottom: 10px;
        }

        .leaderboard-section {
            margin-top: 25px;
            text-align: left;
        }

        .leaderboard-section h3 {
            margin-bottom: 10px;
            border-bottom: 1px solid var(--text-muted);
            padding-bottom: 5px;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
            font-size: 0.9rem;
        }

        th, td {
            padding: 8px;
            text-align: left;
            border-bottom: 1px solid #334155;
        }

        th {
            color: var(--text-muted);
        }

        /* Utility Hidden Canvas */
        canvas {
            display: none;
        }
    </style>
</head>
<body>

    <header>
        <h1>Face Puzzle</h1>
        <p>Turn your own reflection into a puzzle game</p>
    </header>

    <div class="container">
        <div id="screen-camera" class="screen active">
            <div class="video-container">
                <video id="webcam" autoplay playsinline></video>
            </div>
            <div id="camera-error" class="error-msg"></div>
            <button id="btn-snap" class="btn">Take Photo</button>
        </div>

        <div id="screen-difficulty" class="screen">
            <h3>Choose Difficulty</h3>
            <div class="difficulty-options">
                <button class="btn btn-diff" data-grid="3">3 × 3 (Easy)</button>
                <button class="btn btn-diff" data-grid="4">4 × 4 (Medium)</button>
                <button class="btn btn-diff" data-grid="5">5 × 5 (Hard)</button>
            </div>
            <button id="btn-retake" class="btn btn-secondary">Retake Photo</button>
        </div>

        <div id="screen-game" class="screen">
            <div class="stats-bar">
                <div class="stat-item">Time: <span id="stat-time">00:00.0</span></div>
                <div class="stat-item">Moves: <span id="stat-moves">0</span></div>
                <div class="stat-item">Progress: <span id="stat-progress">0/0</span></div>
            </div>
            <div id="puzzle-board" class="puzzle-board"></div>
            <div class="controls">
                <button id="btn-new-photo" class="btn btn-secondary">New Photo</button>
            </div>
        </div>

        <div id="screen-results" class="screen">
            <div class="results-overlay">
                <h2>🎉 Puzzle Solved!</h2>
                <div id="win-summary"></div>
            </div>
            
            <div class="leaderboard-section">
                <h3>🏆 Top 5 Leaderboard</h3>
                <table id="leaderboard-table">
                    <thead>
                        <tr>
                            <th>Date</th>
                            <th>Grid</th>
                            <th>Time</th>
                            <th>Moves</th>
                        </tr>
                    </thead>
                    <tbody id="leaderboard-body"></tbody>
                </table>
            </div>

            <div style="margin-top: 20px;">
                <button id="btn-play-again" class="btn">Play Again</button>
                <button id="btn-resnap" class="btn btn-secondary">New Photo</button>
            </div>
        </div>
    </div>

    <canvas id="hidden-canvas"></canvas>

    <script>
        // State configuration
        let stream = null;
        let capturedImage = null; 
        let gridSize = 3; 
        let pieces = []; 
        let originalPositions = []; 
        let movesCount = 0;
        let timerInterval = null;
        let startTime = null;
        let activeDragTile = null;

        // Elements
        const video = document.getElementById('webcam');
        const cameraError = document.getElementById('camera-error');
        const hiddenCanvas = document.getElementById('hidden-canvas');
        const board = document.getElementById('puzzle-board');
        
        // Screens
        const screens = {
            camera: document.getElementById('screen-camera'),
            difficulty: document.getElementById('screen-difficulty'),
            game: document.getElementById('screen-game'),
            results: document.getElementById('screen-results')
        };

        function showScreen(screenKey) {
            Object.keys(screens).forEach(key => {
                screens[key].classList.toggle('active', key === screenKey);
            });
        }

        // Camera access management
        async function initCamera() {
            cameraError.textContent = "";
            try {
                stream = await navigator.mediaDevices.getUserMedia({
                    video: { facingMode: "user", width: { ideal: 600 }, height: { ideal: 600 } },
                    audio: false
                });
                video.srcObject = stream;
            } catch (err) {
                cameraError.textContent = "Unable to access camera. Please allow permission and ensure you are on HTTPS/localhost.";
                console.error(err);
            }
        }

        // Stop camera utility
        function stopCamera() {
            if (stream) {
                stream.getTracks().forEach(track => track.stop());
                stream = null;
            }
        }

        // Action: Take snapshot
        document.getElementById('btn-snap').addEventListener('click', () => {
            if (!stream) return;
            const ctx = hiddenCanvas.getContext('2d');
            const size = Math.min(video.videoWidth, video.videoHeight);
            hiddenCanvas.width = 500;
            hiddenCanvas.height = 500;

            // Crop snapshot into a square frame matching flipped dimensions
            const sx = (video.videoWidth - size) / 2;
            const sy = (video.videoHeight - size) / 2;
            
            ctx.translate(500, 0);
            ctx.scale(-1, 1); // Mirrored matching video
            ctx.drawImage(video, sx, sy, size, size, 0, 0, 500, 500);
            
            capturedImage = hiddenCanvas.toDataURL('image/jpeg');
            stopCamera();
            showScreen('difficulty');
        });

        // Difficult Selection
        document.querySelectorAll('.btn-diff').forEach(btn => {
            btn.addEventListener('click', (e) => {
                gridSize = parseInt(e.target.getAttribute('data-grid'));
                generatePuzzle();
                showScreen('game');
                startTimer();
            });
        });

        // Retake Configuration Hooks
        document.getElementById('btn-retake').addEventListener('click', () => {
            initCamera();
            showScreen('camera');
        });
        document.getElementById('btn-new-photo').addEventListener('click', () => {
            clearInterval(timerInterval);
            initCamera();
            showScreen('camera');
        });
        document.getElementById('btn-resnap').addEventListener('click', () => {
            initCamera();
            showScreen('camera');
        });
        document.getElementById('btn-play-again').addEventListener('click', () => {
            generatePuzzle();
            showScreen('game');
            startTimer();
        });

        // Core Puzzle Processing Engine
        function generatePuzzle() {
            movesCount = 0;
            document.getElementById('stat-moves').textContent = movesCount;
            board.innerHTML = '';
            board.style.gridTemplateColumns = `repeat(${gridSize}, 1fr)`;
            board.style.gridTemplateRows = `repeat(${gridSize}, 1fr)`;

            const totalTiles = gridSize * gridSize;
            pieces = [];
            originalPositions = [];

            for (let i = 0; i < totalTiles; i++) {
                originalPositions.push(i);
                pieces.push(i);
            }

            // Shuffle sequence ensuring analytical solvability parity
            do {
                shuffleArray(pieces);
            } while (!isSolvable(pieces, gridSize) || isAlreadySolved(pieces));

            renderBoard();
            updateProgress();
        }

        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Mathematical checking function for permutation inversions parity validation
        function isSolvable(arr, size) {
            let inversions = 0;
            for (let i = 0; i < arr.length; i++) {
                for (let j = i + 1; j < arr.length; j++) {
                    if (arr[i] > arr[j]) inversions++;
                }
            }
            if (size % 2 !== 0) {
                return inversions % 2 === 0;
            } else {
                // For even dimensions, custom logic ensures stability regardless of parity context
                return true; 
            }
        }

        function isAlreadySolved(arr) {
            return arr.every((val, index) => val === index);
        }

        function renderBoard() {
            board.innerHTML = '';
            pieces.forEach((pieceIdx, currentGridPos) => {
                const tile = document.createElement('div');
                tile.classList.add('tile');
                tile.setAttribute('data-current-pos', currentGridPos);
                tile.setAttribute('data-original-idx', pieceIdx);
                tile.style.backgroundImage = `url(${capturedImage})`;
                tile.style.backgroundSize = `${gridSize * 100}% ${gridSize * 100}%`;

                // Calculate backgrounds positions offsets matching slices positions coordinates
                const row = Math.floor(pieceIdx / gridSize);
                const col = pieceIdx % gridSize;
                tile.style.backgroundPosition = `${(col / (gridSize - 1)) * 100}% ${(row / (gridSize - 1)) * 100}%`;

                if (pieceIdx === currentGridPos) {
                    tile.classList.add('correct');
                }

                // Append unified cursor & touch events standard structural hooks
                setupDragAndDrop(tile);
                board.appendChild(tile);
            });
        }

        // Standard Interface Handling Gestures Logic
        function setupDragAndDrop(tile) {
            tile.draggable = true;

            // Desktop Drag Events
            tile.addEventListener('dragstart', handleDragStart);
            tile.addEventListener('dragover', handleDragOver);
            tile.addEventListener('drop', handleDrop);
            tile.addEventListener('dragend', handleDragEnd);

            // Universal Mobile Touch Event Wrappers Map
            tile.addEventListener('touchstart', handleTouchStart, { passive: false });
            tile.addEventListener('touchmove', handleTouchMove, { passive: false });
            tile.addEventListener('touchend', handleTouchEnd);
        }

        // Desktop Event Hooks logic implementations
        function handleDragStart(e) {
            activeDragTile = this;
            this.classList.add('dragging');
            e.dataTransfer.setData('text/plain', this.getAttribute('data-current-pos'));
        }

        function handleDragOver(e) {
            e.preventDefault();
        }

        function handleDrop(e) {
            e.preventDefault();
            const sourcePos = parseInt(e.dataTransfer.getData('text/plain'));
            const targetPos = parseInt(this.getAttribute('data-current-pos'));
            swapPieces(sourcePos, targetPos);
        }

        function handleDragEnd() {
            this.classList.remove('dragging');
            activeDragTile = null;
        }

        // Mobile Gesture Processing Engines Engine Map implementations
        let touchStartPos = null;
        function handleTouchStart(e) {
            e.preventDefault();
            activeDragTile = this;
            this.classList.add('dragging');
            const touch = e.touches[0];
            touchStartPos = { x: touch.clientX, y: touch.clientY };
        }

        function handleTouchMove(e) {
            if (!activeDragTile) return;
            e.preventDefault();
        }

        function handleTouchEnd(e) {
            if (!activeDragTile) return;
            activeDragTile.classList.remove('dragging');

            const touch = e.changedTouches[0];
            const targetEl = document.elementFromPoint(touch.clientX, touch.clientY);
            
            // Check structural drop destination targeting match logic validation
            const destinationTile = targetEl ? targetEl.closest('.tile') : null;

            if (destinationTile && destinationTile !== activeDragTile) {
                const sourcePos = parseInt(activeDragTile.getAttribute('data-current-pos'));
                const targetPos = parseInt(destinationTile.getAttribute('data-current-pos'));
                swapPieces(sourcePos, targetPos);
            }
            activeDragTile = null;
        }

        // Shared Node Swapping Mutation Array Update Controller 
        function swapPieces(srcIdx, tgtIdx) {
            if (srcIdx === tgtIdx) return;
            
            // Update items sequence layout reference mappings array arrays references
            const temp = pieces[srcIdx];
            pieces[srcIdx] = pieces[tgtIdx];
            pieces[tgtIdx] = temp;

            movesCount++;
            document.getElementById('stat-moves').textContent = movesCount;

            renderBoard();
            updateProgress();
            checkWinCondition();
        }

        function updateProgress() {
            let correctCount = 0;
            pieces.forEach((val, idx) => {
                if (val === idx) correctCount++;
            });
            document.getElementById('stat-progress').textContent = `${correctCount}/${gridSize * gridSize}`;
        }

        // Precision Runtime Tracking Clock engine component loop implementation logic
        function startTimer() {
            clearInterval(timerInterval);
            startTime = performance.now();
            timerInterval = setInterval(() => {
                const elapsed = performance.now() - startTime;
                document.getElementById('stat-time').textContent = formatTime(elapsed);
            }, 33); // Approx ~30 FPS tracking precision resolution output rates
        }

        function formatTime(ms) {
            let minutes = Math.floor(ms / 60000);
            let seconds = Math.floor((ms % 60000) / 1000);
            let tenths = Math.floor((ms % 1000) / 100);

            minutes = minutes < 10 ? '0' + minutes : minutes;
            seconds = seconds < 10 ? '0' + seconds : seconds;
            return `${minutes}:${seconds}.${tenths}`;
        }

        // Analytical Verification Loop Metrics State Check Event Evaluation Engine Logic
        function checkWinCondition() {
            if (isAlreadySolved(pieces)) {
                clearInterval(timerInterval);
                const finalTimeStr = document.getElementById('stat-time').textContent;
                
                // Construct metrics layout templates elements references values properties objects arrays
                document.getElementById('win-summary').innerHTML = `
                    <p>Difficulty: <strong>${gridSize}x${gridSize}</strong></p>
                    <p>Total Moves: <strong>${movesCount}</strong></p>
                    <p>Final Time: <strong>${finalTimeStr}</strong></p>
                `;

                saveLeaderboard(gridSize, finalTimeStr, movesCount);
                renderLeaderboard();
                showScreen('results');
            }
        }

        // Persistent LocalStorage Ranking Records Layer Configurations Data Management Engine
        function saveLeaderboard(grid, timeStr, moves) {
            const records = JSON.parse(localStorage.getItem('face_puzzle_leaderboard')) || [];
            const newRecord = {
                date: new Date().toLocaleDateString(),
                grid: `${grid}x${grid}`,
                time: timeStr,
                moves: moves,
                rawTime: performance.now() - startTime // raw delta scalar mapping key index sort logic support baseline
            };

            records.push(newRecord);
            // Dynamic Sort ascending prioritizing minimal durations records sequences
            records.sort((a, b) => a.rawTime - b.rawTime);
            
            // Trim preservation array boundaries keeping only optimal 5 array item indexes sets elements records
            const topFive = records.slice(0, 5);
            localStorage.setItem('face_puzzle_leaderboard', JSON.stringify(topFive));
        }

        function renderLeaderboard() {
            const records = JSON.parse(localStorage.getItem('face_puzzle_leaderboard')) || [];
            const tbody = document.getElementById('leaderboard-body');
            tbody.innerHTML = '';

            if (records.length === 0) {
                tbody.innerHTML = `<tr><td colspan="4" style="text-align:center;">No records established yet!</td></tr>`;
                return;
            }

            records.forEach(rec => {
                const row = document.createElement('tr');
                row.innerHTML = `
                    <td>${rec.date}</td>
                    <td>${rec.grid}</td>
                    <td>${rec.time}</td>
                    <td>${rec.moves}</td>
                `;
                tbody.appendChild(row);
            });
        }

        // Start App Instance Sequence Configurations Initialization 
        window.addEventListener('DOMContentLoaded', () => {
            initCamera();
            renderLeaderboard();
        });
    </script>
</body>
</html>