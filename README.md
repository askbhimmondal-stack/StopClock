

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Minimal Stopwatch</title>
    <style>
        body {
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
            background: #d1c9b8;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, sans-serif;
        }
       
        .stopwatch {
            background: #e7e1d3;
            border-radius: 16px;
            padding: 30px;
            box-shadow: 0 8px 30px rgba(0,0,0,0.08);
            text-align: center;
            width: 340px;
            max-width: 90vw;
            position: relative;
        }
       
        .logo {
            width: 100%;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
       
        .logo img {
            max-width: 100%;
            height: auto;
            border-radius: 8px;
        }
       
        .display {
            font-size: 48px;
            font-weight: 300;
            margin: 20px 0;
            color: #333;
            font-variant-numeric: tabular-nums;
            letter-spacing: 2px;
        }
       
        .controls {
            display: flex;
            justify-content: center;
            gap: 8px;
            margin-bottom: 20px;
        }
       
        button {
            padding: 10px 16px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 14px;
            transition: all 0.2s;
            font-weight: 500;
        }
       
        #startBtn {
            background: #4CAF50;
            color: white;
        }
       
        #stopBtn {
            background: #f44336;
            color: white;
        }
       
        #resetBtn {
            background: #757575;
            color: white;
        }
       
        #lapBtn {
            background: #2196F3;
            color: white;
        }
       
        button:hover {
            opacity: 0.9;
            transform: translateY(-2px);
        }
       
        button:active {
            transform: translateY(0);
        }
       
        button:disabled {
            opacity: 0.5;
            cursor: not-allowed;
            transform: none;
        }
       
        .laps {
            max-height: 200px;
            overflow-y: auto;
            margin-top: 20px;
            text-align: left;
            border-top: 1px solid #d1c9b8;
            padding-top: 15px;
        }
       
        .lap-item {
            padding: 8px 0;
            border-bottom: 1px solid #d1c9b8;
            font-size: 14px;
            display: flex;
            justify-content: space-between;
        }
       
        .lap-number {
            font-weight: 500;
            color: #666;
        }
       
        .lap-time {
            font-variant-numeric: tabular-nums;
        }
       
        .laps-title {
            font-size: 14px;
            color: #888;
            margin-bottom: 10px;
            text-align: center;
        }
       
        .footer {
            margin-top: 20px;
            padding-top: 15px;
            border-top: 1px solid #d1c9b8;
            font-size: 14px;
            color: #888;
        }
    </style>
</head>
<body>
    <div class="stopwatch">
        <div class="logo">
            <img src="https://i.ibb.co/B2KPHct0/image.png" alt="Stopwatch Logo">
        </div>
       
        <div class="display" id="display">00:00.00</div>
       
        <div class="controls">
            <button id="startBtn">Start</button>
            <button id="stopBtn" disabled>Stop</button>
            <button id="resetBtn">Reset</button>
            <button id="lapBtn" disabled>Lap</button>
        </div>
       
        <div class="laps">
            <div class="laps-title">LAP TIMES</div>
            <div id="lapTimes"></div>
        </div>
       
        <div class="footer">Made in ❤️ for Pooja</div>
    </div>

    <script>
        let startTime;
        let elapsedTime = 0;
        let timerInterval;
        let isRunning = false;
        let lapCount = 0;

        const display = document.getElementById('display');
        const startBtn = document.getElementById('startBtn');
        const stopBtn = document.getElementById('stopBtn');
        const resetBtn = document.getElementById('resetBtn');
        const lapBtn = document.getElementById('lapBtn');
        const lapTimes = document.getElementById('lapTimes');

        function formatTime(ms) {
            let minutes = Math.floor(ms / 60000);
            let seconds = Math.floor((ms % 60000) / 1000);
            let milliseconds = Math.floor((ms % 1000) / 10);
           
            return (
                String(minutes).padStart(2, '0') + ':' +
                String(seconds).padStart(2, '0') + '.' +
                String(milliseconds).padStart(2, '0')
            );
        }

        function updateDisplay() {
            display.textContent = formatTime(elapsedTime);
        }

        function start() {
            if (!isRunning) {
                startTime = Date.now() - elapsedTime;
                timerInterval = setInterval(function() {
                    elapsedTime = Date.now() - startTime;
                    updateDisplay();
                }, 10);
                isRunning = true;
               
                startBtn.disabled = true;
                stopBtn.disabled = false;
                lapBtn.disabled = false;
            }
        }

        function stop() {
            if (isRunning) {
                clearInterval(timerInterval);
                isRunning = false;
               
                startBtn.disabled = false;
                stopBtn.disabled = true;
            }
        }

        function reset() {
            clearInterval(timerInterval);
            isRunning = false;
            elapsedTime = 0;
            lapCount = 0;
            updateDisplay();
            lapTimes.innerHTML = '';
           
            startBtn.disabled = false;
            stopBtn.disabled = true;
            lapBtn.disabled = true;
        }

        function lap() {
            if (isRunning) {
                lapCount++;
                const lapTime = formatTime(elapsedTime);
                const lapItem = document.createElement('div');
                lapItem.className = 'lap-item';
                lapItem.innerHTML = `<span class="lap-number">Lap ${lapCount}</span><span class="lap-time">${lapTime}</span>`;
                lapTimes.prepend(lapItem);
            }
        }

        startBtn.addEventListener('click', start);
        stopBtn.addEventListener('click', stop);
        resetBtn.addEventListener('click', reset);
        lapBtn.addEventListener('click', lap);
    </script>
</body>
</html> 
