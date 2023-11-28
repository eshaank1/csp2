<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Binary Quiz</title>
    <style>
        /* Your CSS styles remain unchanged */
    </style>
</head>
<body>
    <h2>Binary Quiz</h2>

    <label for="binaryInput">Convert the following decimal number to binary:</label>
    <p id="quizQuestion"></p>
    <input type="text" id="binaryInput" placeholder="Enter binary number">
    <button onclick="checkAnswer()">Check Answer</button>
    <p id="quizResult"></p>
    <p id="score">Score: 0</p>

    <script>
        let score = 0; // Initialize score

        function generateRandomNumber() {
            return Math.floor(Math.random() * 100) + 1;
        }

        function setupQuiz() {
            const quizQuestion = document.getElementById('quizQuestion');
            const binaryInput = document.getElementById('binaryInput');
            const quizResult = document.getElementById('quizResult');

            const randomNumber = generateRandomNumber();

            quizQuestion.textContent = "Convert the following decimal number to binary: " + randomNumber;
        }

        function checkAnswer() {
            const userAnswer = document.getElementById('binaryInput').value;
            const randomNumber = parseInt(document.getElementById('quizQuestion').textContent.split(": ")[1]);
            const correctAnswer = (randomNumber).toString(2); // Convert random number to binary

            if (userAnswer === correctAnswer) {
                quizResult.textContent = "Correct!";
                score++;
            } else {
                quizResult.textContent = "Incorrect. The correct answer is " + correctAnswer;
            }

            // Update the score display
            document.getElementById('score').textContent = "Score: " + score;

            // Set up the next quiz question
            setupQuiz();
        }

        setupQuiz(); // Initial quiz setup
    </script>
</body>
</html>
