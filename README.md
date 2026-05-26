<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minimal Study Flow</title>
    <style>
        :root {
            --teal-dark: #00796b;
            --teal-medium: #4db6ac;
            --teal-light: #e0f2f1;
            --text: #2c3e50;
        }

        body {
            font-family: 'Inter', -apple-system, sans-serif;
            background-color: var(--teal-light);
            color: var(--text);
            margin: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
        }

        .container {
            background: white;
            padding: 40px;
            border-radius: 24px;
            box-shadow: 0 10px 25px rgba(0, 121, 107, 0.05);
            width: 90%;
            max-width: 500px;
            text-align: center;
        }

        h1 {
            font-size: 1.5rem; /* Smaller title */
            font-weight: 500;
            margin-bottom: 30px;
            letter-spacing: -0.5px;
        }

        /* The Grid for Square Buttons */
        .method-grid {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 20px;
        }

        .square-card {
            background: #ffffff;
            border: 2px solid var(--teal-light);
            border-radius: 20px;
            aspect-ratio: 1 / 1; /* Makes it a perfect square */
            display: flex;
            flex-direction: column;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            transition: all 0.3s ease;
            padding: 10px;
        }

        .square-card:hover {
            border-color: var(--teal-medium);
            transform: translateY(-5px);
            background-color: #fafdfd;
        }

        .icon-preview {
            font-size: 2rem;
            margin-bottom: 10px;
        }

        .card-label {
            font-weight: 600;
            font-size: 0.9rem;
            color: var(--teal-dark);
        }

        /* Timer Section */
        #study-section { display: none; }

        #timer-display {
            font-size: 4rem;
            font-weight: 200;
            color: var(--teal-dark);
            margin: 30px 0;
        }

        .control-btn {
            background: var(--teal-medium);
            color: white;
            border: none;
            padding: 12px 30px;
            border-radius: 50px;
            font-size: 1rem;
            cursor: pointer;
            margin: 5px;
        }

        .back-link {
            display: block;
            margin-top: 20px;
            color: #95a5a6;
            text-decoration: none;
            font-size: 0.8rem;
            cursor: pointer;
        }
    </style>
</head>
<body>

    <div class="container">
        <div id="menu">
            <h1>Select Method</h1>
            <div class="method-grid">
                <div class="square-card" onclick="startMethod('Pomodoro', 25)">
                    <div class="icon-preview">🍅</div>
                    <div class="card-label">Pomodoro</div>
                </div>
                <div class="square-card" onclick="startMethod('Quick Burst', 10)">
                    <div class="icon-preview">⚡</div>
                    <div class="card-label">Quick Burst</div>
                </div>
            </div>
        </div>

        <div id="study-section">
            <h2 id="method-title" style="font-size: 1.2rem; color: var(--teal-dark);">Method</h2>
            <p id="method-desc" style="font-size: 0.9rem; color: #7f8c8d;"></p>
            
            <div id="timer-display">25:00</div>
            
            <button class="control-btn" onclick="toggleTimer()" id="start-btn">Start</button>
            <span class="back-link" onclick="reset()">← Change Method</span>
        </div>
    </div>

    <script>
        let timeLeft;
        let timerId = null;
        let isRunning = false;

        function startMethod(name, mins) {
            document.getElementById('menu').style.display = 'none';
            document.getElementById('study-section').style.display = 'block';
            document.getElementById('method-title').innerText = name;
            
            if(name === 'Pomodoro') {
                document.getElementById('method-desc').innerText = "Focus deeply for 25 minutes.";
            } else {
                document.getElementById('method-desc').innerText = "Rapid focus for a short task.";
            }

            timeLeft = mins * 60;
            updateDisplay();
        }

        function updateDisplay() {
            let minutes = Math.floor(timeLeft / 60);
            let seconds = timeLeft % 60;
            document.getElementById('timer-display').innerText = 
                `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
        }

        function toggleTimer() {
            if (isRunning) {
                clearInterval(timerId);
                document.getElementById('start-btn').innerText = "Resume";
            } else {
                timerId = setInterval(() => {
                    timeLeft--;
                    updateDisplay();
                    if (timeLeft <= 0) {
                        clearInterval(timerId);
                        alert("Time's up! Great work.");
                        reset();
                    }
                }, 1000);
                document.getElementById('start-btn').innerText = "Pause";
            }
            isRunning = !isRunning;
        }

        function reset() {
            clearInterval(timerId);
            isRunning = false;
            document.getElementById('menu').style.display = 'block';
            document.getElementById('study-section').style.display = 'none';
            document.getElementById('start-btn').innerText = "Start";
        }
    </script>
</body>
</html>