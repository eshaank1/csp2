---
toc: true
comments: false
layout: post
title: binary palindromes
type: tangibles
courses: { compsci: {week: 0} }
---

<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Palindrome Checker</title>
</head>
<body>
    <h2>Binary Palindrome Checker</h2>
    <label for="binaryInput">Enter Binary Number:</label>
    <input type="text" id="binaryInput" placeholder="Enter binary number">
    <button onclick="checkPalindrome()">Check Palindrome</button>
    <p id="result"></p>

    <script>
        function checkPalindrome() {
            const binaryInput = document.getElementById("binaryInput").value;

            if (!/^[01]+$/.test(binaryInput)) {
                alert("Please enter a valid binary number (0s and 1s only).");
                return;
            }

            const reversedBinary = binaryInput.split('').reverse().join('');
            const isPalindrome = binaryInput === reversedBinary;

            const resultElement = document.getElementById("result");
            resultElement.textContent = isPalindrome
                ? "Yes, it's a palindrome!"
                : "No, it's not a palindrome.";
        }
    </script>
</body>
</html>
