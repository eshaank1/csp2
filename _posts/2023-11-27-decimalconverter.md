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
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 40px;
        }
    </style>
</head>
<body>
    <h2>Binary to Decimal Converter</h2>

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
            }
        }
    </script>
</body>
</html>

