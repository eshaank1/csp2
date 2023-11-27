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
            height: 100vh;
            margin: 0;
            font-family: 'Arial', sans-serif;
        }

        .binary-clock {
            display: grid;
            grid-template-columns: repeat(6, 50px);
            gap: 5px;
        }

        .binary-digit {
            width: 50px;
            height: 50px;
            border: 1px solid #333;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 18px;
        }
    </style>
</head>
<body>
    <div class="binary-clock" id="binaryClock"></div>

    <script>
        function updateBinaryClock() {
            const binaryClock = document.getElementById('binaryClock');
            const time = new Date();

            const hours = time.getHours().toString(2).padStart(6, '0');
            const minutes = time.getMinutes().toString(2).padStart(6, '0');
            const seconds = time.getSeconds().toString(2).padStart(6, '0');

            const binaryTime = hours + minutes + seconds;

            binaryClock.innerHTML = '';

            for (let i = 0; i < binaryTime.length; i++) {
                const binaryDigit = document.createElement('div');
                binaryDigit.classList.add('binary-digit');
                binaryDigit.textContent = binaryTime[i];
                binaryClock.appendChild(binaryDigit);
            }
        }

        // Update the clock every second
        setInterval(updateBinaryClock, 1000);

        // Initial update
        updateBinaryClock();
    </script>
</body>
</html>