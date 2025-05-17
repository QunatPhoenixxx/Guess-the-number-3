<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Ultimate Guess the Number</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <style>
    :root {
      --main-color: #4caf50;
      --dark-bg: #1c1c1c;
      --light-text: #f5f5f5;
      --accent: #ffc107;
    }

    body {
      margin: 0;
      font-family: 'Segoe UI', sans-serif;
      background-color: var(--dark-bg);
      color: var(--light-text);
      display: flex;
      flex-direction: column;
      align-items: center;
      padding: 2rem;
    }

    h1 {
      font-size: 2.5rem;
      color: var(--accent);
      margin-bottom: 0.5rem;
    }

    .game-container {
      background-color: #2c2c2c;
      padding: 2rem;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0,0,0,0.5);
      max-width: 400px;
      width: 100%;
      text-align: center;
    }

    input, select, button {
      padding: 0.7rem;
      font-size: 1rem;
      margin-top: 1rem;
      width: 100%;
      border-radius: 8px;
      border: none;
      box-sizing: border-box;
    }

    button {
      background-color: var(--main-color);
      color: white;
      cursor: pointer;
      font-weight: bold;
      transition: background 0.2s;
    }

    button:hover {
      background-color: #45a049;
    }

    .message {
      margin-top: 1.5rem;
      font-size: 1.2rem;
      min-height: 1.5rem;
    }

    .score {
      margin-top: 1rem;
      font-size: 1rem;
      color: var(--accent);
    }

    @media (max-width: 500px) {
      h1 {
        font-size: 2rem;
      }
      .game-container {
        padding: 1rem;
      }
    }
  </style>
</head>
<body>

  <h1>Ultimate Guess the Number</h1>

  <div class="game-container">
    <label for="difficulty">Choose Difficulty:</label>
    <select id="difficulty">
      <option value="easy">Easy (1-10)</option>
      <option value="medium" selected>Medium (1-50)</option>
      <option value="hard">Hard (1-100)</option>
    </select>

    <input type="number" id="guessInput" placeholder="Enter your guess..." />
    <button onclick="submitGuess()">Guess</button>
    <div class="message" id="message"></div>
    <div class="score" id="scoreDisplay">Score: 0</div>
    <button onclick="startGame()">Restart Game</button>
  </div>

  <!-- Sound Effects -->
  <audio id="sound-correct" src="https://assets.mixkit.co/sfx/preview/mixkit-winning-notification-2018.mp3"></audio>
  <audio id="sound-wrong" src="https://assets.mixkit.co/sfx/preview/mixkit-wrong-answer-bass-buzzer-948.mp3"></audio>
  <audio id="sound-click" src="https://assets.mixkit.co/sfx/preview/mixkit-modern-technology-select-3124.mp3"></audio>

  <script>
    let targetNumber = 0;
    let maxNumber = 50;
    let score = 0;
    let tries = 0;

    const messageDiv = document.getElementById("message");
    const scoreDisplay = document.getElementById("scoreDisplay");
    const correctSound = document.getElementById("sound-correct");
    const wrongSound = document.getElementById("sound-wrong");
    const clickSound = document.getElementById("sound-click");

    function startGame() {
      const difficulty = document.getElementById("difficulty").value;
      switch (difficulty) {
        case "easy":
          maxNumber = 10;
          break;
        case "medium":
          maxNumber = 50;
          break;
        case "hard":
          maxNumber = 100;
          break;
      }

      targetNumber = Math.floor(Math.random() * maxNumber) + 1;
      tries = 0;
      messageDiv.textContent = "Game restarted. Try your luck!";
      document.getElementById("guessInput").value = "";
      scoreDisplay.textContent = `Score: ${score}`;
      clickSound.play();
    }

    function submitGuess() {
      const guess = parseInt(document.getElementById("guessInput").value);
      if (isNaN(guess) || guess < 1 || guess > maxNumber) {
        messageDiv.textContent = `Enter a number between 1 and ${maxNumber}.`;
        wrongSound.play();
        return;
      }

      tries++;

      if (guess === targetNumber) {
        messageDiv.textContent = `Correct! You got it in ${tries} ${tries === 1 ? "try" : "tries"}!`;
        score += Math.max(10 - tries, 1);
        scoreDisplay.textContent = `Score: ${score}`;
        correctSound.play();
        targetNumber = Math.floor(Math.random() * maxNumber) + 1;
        tries = 0;
      } else if (guess < targetNumber) {
        messageDiv.textContent = "Too low! Try again.";
        wrongSound.play();
      } else {
        messageDiv.textContent = "Too high! Try again.";
        wrongSound.play();
      }
    }

    startGame(); // Init
  </script>

</body>
</html>
