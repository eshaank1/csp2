<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Clock</title>
    <style>
        body {
            display: flex;
            align-items: center;
            justify-content: center;
            flex-direction: column;
            height: 100vh;
            margin: 0;
            font-family: 'Arial', sans-serif;
            background-color: #282c34;
            color: #61dafb;
        }

        #currentTime {
            font-size: 24px;
            margin-bottom: 20px;
            background-color: #61dafb;
            padding: 10px;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(97, 218, 251, 0.5);
        }

        .binary-clock {
            display: grid;
            grid-template-rows: repeat(3, 60px);
            gap: 20px;
        }

        .binary-row {
            display: flex;
            gap: 10px;
        }

        .binary-digit {
            width: 40px;
            height: 60px;
            border: 2px solid #61dafb;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            background-color: #282c34;
            border-radius: 5px;
            box-shadow: 0 0 10px rgba(97, 218, 251, 0.5);
        }
    </style>
</head>
<body>
    <div id="currentTime"></div>
    <div class="binary-clock" id="binaryClock">
        <div class="binary-row" id="hourRow"></div>
        <div class="binary-row" id="minuteRow"></div>
        <div class="binary-row" id="secondRow"></div>
    </div>

    <script>
        function updateBinaryClock() {
            const hourRow = document.getElementById('hourRow');
            const minuteRow = document.getElementById('minuteRow');
            const secondRow = document.getElementById('secondRow');
            const currentTime = document.getElementById('currentTime');
            const time = new Date();

            const hours = time.getHours().toString(2).padStart(6, '0');
            const minutes = time.getMinutes().toString(2).padStart(6, '0');
            const seconds = time.getSeconds().toString(2).padStart(6, '0');

            updateBinaryRow(hourRow, hours);
            updateBinaryRow(minuteRow, minutes);
            updateBinaryRow(secondRow, seconds);

            currentTime.textContent = `Current Time: ${time.toLocaleTimeString()}`;
        }

        function updateBinaryRow(rowElement, binaryValue) {
            rowElement.innerHTML = '';

            for (let i = 0; i < binaryValue.length; i++) {
                const binaryDigit = document.createElement('div');
                binaryDigit.classList.add('binary-digit');
                binaryDigit.textContent = binaryValue[i];
                rowElement.appendChild(binaryDigit);
            }
        }

        // Update the clock every second
        setInterval(updateBinaryClock, 1000);

        // Initial update
        updateBinaryClock();
    </script>
</body>
</html>
