<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Countdown to February 1st</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            color: white;
            padding: 20px;
        }

        .container {
            text-align: center;
            padding: 40px;
            background: rgba(255, 255, 255, 0.1);
            backdrop-filter: blur(10px);
            border-radius: 20px;
            box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
            max-width: 800px;
            width: 100%;
        }

        .pages-per-day {
            font-size: 1.5em;
            margin-bottom: 30px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
        }

        .pages-per-day .rate {
            font-size: 2.5em;
            font-weight: bold;
            color: #ffeb3b;
            display: block;
            margin: 10px 0;
        }

        .page-display {
            font-size: 1.5em;
            margin-bottom: 30px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }

        .page-display .remaining {
            font-size: 2.5em;
            font-weight: bold;
            display: block;
            margin: 10px 0;
        }

        .countdown {
            display: flex;
            gap: 30px;
            justify-content: center;
            flex-wrap: wrap;
            margin-bottom: 50px;
        }

        .time-unit {
            background: rgba(255, 255, 255, 0.2);
            padding: 30px;
            border-radius: 15px;
            min-width: 120px;
            backdrop-filter: blur(5px);
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }

        .time-unit .number {
            font-size: 3em;
            font-weight: bold;
            display: block;
            margin-bottom: 10px;
        }

        .time-unit .label {
            font-size: 1em;
            text-transform: uppercase;
            letter-spacing: 2px;
            opacity: 0.8;
        }

        .input-section {
            margin-top: 30px;
            padding: 20px;
            background: rgba(255, 255, 255, 0.15);
            border-radius: 10px;
        }

        .input-section label {
            display: block;
            margin-bottom: 10px;
            font-size: 1.2em;
        }

        .input-group {
            display: flex;
            gap: 10px;
            justify-content: center;
            align-items: center;
            flex-wrap: wrap;
        }

        .input-section input {
            padding: 15px;
            font-size: 1.2em;
            border: none;
            border-radius: 10px;
            background: rgba(255, 255, 255, 0.9);
            color: #333;
            width: 150px;
            text-align: center;
        }

        .input-section button {
            padding: 15px 30px;
            font-size: 1.2em;
            border: none;
            border-radius: 10px;
            background: #ffeb3b;
            color: #333;
            cursor: pointer;
            font-weight: bold;
            transition: transform 0.2s, background 0.2s;
        }

        .input-section button:hover {
            background: #fdd835;
            transform: scale(1.05);
        }

        .input-section button:active {
            transform: scale(0.95);
        }

        @media (max-width: 600px) {
            h1 {
                font-size: 2em;
            }
            
            .time-unit {
                min-width: 80px;
                padding: 20px;
            }
            
            .time-unit .number {
                font-size: 2em;
            }

            .pages-per-day .rate {
                font-size: 2em;
            }

            .page-display .remaining {
                font-size: 2em;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="pages-per-day">
            <div>Pages per day to finish on time:</div>
            <span class="rate" id="pagesPerDay">0.00</span>
        </div>

        <div class="page-display">
            <div>Pages remaining:</div>
            <span class="remaining" id="pagesRemaining">120</span>
        </div>

        <div class="countdown">
            <div class="time-unit">
                <span class="number" id="days">0</span>
                <span class="label">Days</span>
            </div>
            <div class="time-unit">
                <span class="number" id="hours">0</span>
                <span class="label">Hours</span>
            </div>
            <div class="time-unit">
                <span class="number" id="minutes">0</span>
                <span class="label">Minutes</span>
            </div>
            <div class="time-unit">
                <span class="number" id="seconds">0</span>
                <span class="label">Seconds</span>
            </div>
        </div>

        <div class="input-section">
            <label for="pagesCompleted">Pages completed:</label>
            <div class="input-group">
                <input type="number" id="pagesCompleted" min="0" max="120" value="0">
                <button onclick="updatePages()">Update</button>
            </div>
        </div>
    </div>

    <script>
        const TOTAL_PAGES = 120;
        let pagesCompleted = parseInt(localStorage.getItem('pagesCompleted')) || 0;
        
        // Initialize input field with saved value
        document.getElementById('pagesCompleted').value = pagesCompleted;

        function updateCountdown() {
            const now = new Date();
            const target = new Date('2026-02-01T00:00:00');
            const diff = target - now;

            if (diff <= 0) {
                document.getElementById('days').textContent = '0';
                document.getElementById('hours').textContent = '0';
                document.getElementById('minutes').textContent = '0';
                document.getElementById('seconds').textContent = '0';
                document.getElementById('pagesPerDay').textContent = '0.00';
                return;
            }

            const days = Math.floor(diff / (1000 * 60 * 60 * 24));
            const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
            const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
            const seconds = Math.floor((diff % (1000 * 60)) / 1000);

            document.getElementById('days').textContent = days;
            document.getElementById('hours').textContent = String(hours).padStart(2, '0');
            document.getElementById('minutes').textContent = String(minutes).padStart(2, '0');
            document.getElementById('seconds').textContent = String(seconds).padStart(2, '0');

            // Calculate pages per day
            const pagesRemaining = TOTAL_PAGES - pagesCompleted;
            const pagesPerDay = days > 0 ? (pagesRemaining / days).toFixed(2) : '0.00';
            document.getElementById('pagesPerDay').textContent = pagesPerDay;
            document.getElementById('pagesRemaining').textContent = pagesRemaining;
        }

        function updatePages() {
            const input = document.getElementById('pagesCompleted');
            pagesCompleted = parseInt(input.value) || 0;
            
            // Ensure value is within bounds
            if (pagesCompleted < 0) pagesCompleted = 0;
            if (pagesCompleted > TOTAL_PAGES) pagesCompleted = TOTAL_PAGES;
            
            input.value = pagesCompleted;
            localStorage.setItem('pagesCompleted', pagesCompleted);
            updateCountdown();
        }

        // Update countdown every second
        updateCountdown();
        setInterval(updateCountdown, 1000);
    </script>
</body>
</html>
