---
toc: true
comments: false
layout: post
title: decimal converter
type: tangibles
courses: { compsci: {week: 0} }
---


<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary-Decimal Converter</title>
    <style>
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: #f4f4f4;
            color: #333;
            text-align: center;
            margin: 20px;
        }

        h2 {
            color: #4285f4;
        }

        label {
            display: block;
            margin: 10px 0;
            color: #555;
        }

        input {
            padding: 8px;
            font-size: 16px;
            border: 1px solid #ccc;
            border-radius: 4px;
            margin-bottom: 10px;
        }

        button {
            padding: 10px;
            font-size: 16px;
            background-color: #4285f4;
            color: #fff;
            border: none;
            border-radius: 4px;
            cursor: pointer;
        }

        button:hover {
            background-color: #357ae8;
        }

        p {
            font-size: 18px;
            margin: 10px 0;
        }

        #binaryResult {
            color: #0f9d58;
        }

        #decimalResult {
            color: #db4437;
        }
    </style>
</head>
<body>
    <h2>Binary-Decimal Converter</h2>

    <label for="decimalInput">Enter Decimal Number:</label>
    <input type="number" id="decimalInput" placeholder="Enter decimal number">
    <button onclick="convertDecimalToBinary()">Convert to Binary</button>
    <p id="binaryResult"></p>

    <label for="binaryInput">Enter Binary Number:</label>
    <input type="text" id="binaryInput" placeholder="Enter binary number">
    <button onclick="convertBinaryToDecimal()">Convert to Decimal</button>
    <p id="decimalResult"></p>

    <script>
        function convertDecimalToBinary() {
            const decimalInput = document.getElementById('decimalInput').value;
            const binaryResult = document.getElementById('binaryResult');

            if (!isNaN(decimalInput)) {
                const binaryValue = (decimalInput >>> 0).toString(2);
                binaryResult.textContent = `Binary: ${binaryValue}`;
            } else {
                binaryResult.textContent = 'Please enter a valid decimal number.';
                binaryResult.style.color = '#db4437';
            }
        }

        function convertBinaryToDecimal() {
            const binaryInput = document.getElementById('binaryInput').value;
            const decimalResult = document.getElementById('decimalResult');

            if (/^[01]+$/.test(binaryInput)) {
                const decimalValue = parseInt(binaryInput, 2);
                decimalResult.textContent = `Decimal: ${decimalValue}`;
            } else {
                decimalResult.textContent = 'Please enter a valid binary number.';
                decimalResult.style.color = '#db4437';
            }
        }
    </script>
</body>
</html>
