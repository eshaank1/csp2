<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Quiz</title>
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

        #quizResult {
            color: #0f9d58;
        }
    </style>
</head>
<body>
    <h2>Binary Quiz</h2>

    <label for="binaryInput">Convert the following decimal number to binary:</label>
    <p id="quizQuestion"></p>
    <input type="text" id="binaryInput" placeholder="Enter binary number">
    <button onclick="checkAnswer()">Check Answer</button>
    <p id="quizResult"></p>

    <script>
        // Function to generate a random number between 1 and 100
        function generateRandomNumber() {
            return Math.floor(Math.random() * 100) + 1;
        }

        // Function to set up a new quiz question
        function setupQuiz() {
            const quizQuestion = document.getElementById('quizQuestion');
            const binaryInput = document.getElementById('binaryInput');
            const quizResult = document.getElementById('quizResult');

            // Generate a random number
            const randomNumber = generateRandomNumber();

            // Display the question
            quizQuestion.textContent = "Convert the following decimal number to binary: " + randomNumber;
        }

        // Function to check the answer
        function checkAnswer() {
            // Add your answer-checking logic here
            // Update quizResult.textContent with the result
        }

        // Set up the initial quiz
        setupQuiz();
    </script>
</body>
</html>
