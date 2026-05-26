<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Study Flow</title>
    <style>
        :root {
            --primary: #6c5ce7;
            --bg: #f9f9fb;
            --text: #2d3436;
        }
        body { font-family: 'Segoe UI', sans-serif; background: var(--bg); color: var(--text); text-align: center; padding: 50px; }
        
        .container { max-width: 600px; margin: auto; background: white; padding: 30px; border-radius: 15px; shadow: 0 10px 30px rgba(0,0,0,0.1); }
        
        .menu-btn { background: var(--primary); color: white; border: none; padding: 15px 25px; margin: 10px; border-radius: 8px; cursor: pointer; transition: 0.3s; }
        .menu-btn:hover { transform: translateY(-3px); opacity: 0.9; }

        #study-section { display: none; margin-top: 30px; }
        
        #timer-display { font-size: 3rem; font-weight: bold; margin: 20px 0; color: var(--primary); }
        
        .hidden { display: none; }
    </style>
</head>
<body>

    <div class="container">
        <h1>Choose Your Study Method</h1>
        <div id="menu">
            <button class="menu-btn" onclick="startMethod('Pomodoro', 25)">Pomodoro (25 min)</button>
            <button class="menu-btn" onclick="startMethod('Quick Burst', 10)">Quick Burst (10 min)</button>
        </div>

        <div id="study-section">
            <h2 id="method-title">Method Name</h2>
            <p id="method-desc">Description goes here...</p>
            
            <div id="timer-display">25:00</div>
            
            <button class="menu-btn" onclick="toggleTimer()" id="start-btn">Start Timer</button>
            <button class="menu-btn" style="background:#b2bec3" onclick="reset()">Back to Menu</button>
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
                document.getElementById('method-desc').innerText = "Focus for 25 minutes, then take a 5-minute break.";
            } else {
                document.getElementById('method-desc').innerText = "A short 10-minute sprint for quick tasks.";
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
                        alert("Time for a break!");
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
            document.getElementById('start-btn').innerText = "Start Timer";
        }
    </script>
</body>
</html>