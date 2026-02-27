<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Vocabulary Game Pro</title>
<style>
body {
    font-family: Arial;
    text-align: center;
    background: linear-gradient(to right, #4facfe, #00f2fe);
    color: white;
}

.game-box {
    background: white;
    color: black;
    width: 400px;
    margin: 50px auto;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 20px rgba(0,0,0,0.3);
}

input {
    padding: 10px;
    width: 80%;
    font-size: 16px;
}

button {
    padding: 10px 15px;
    margin-top: 10px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 5px;
    border: none;
    background-color: #4facfe;
    color: white;
}

button:hover {
    background-color: #00c6ff;
}

#timer {
    font-size: 20px;
    color: red;
}

#leaderboard {
    margin-top: 20px;
    text-align: left;
}
</style>
</head>
<body>

<h1>🎮 Vocabulary Master Game</h1>

<div class="game-box">
    <div id="timer">⏳ Time: 10</div>
    <h3 id="question"></h3>
    <input type="text" id="answer" placeholder="Type English word...">
    <br>
    <button onclick="checkAnswer()">Submit</button>
    <p id="result"></p>
    <p>🏆 Score: <span id="score">0</span></p>
    <button onclick="restartGame()">🔄 Restart</button>

    <div id="leaderboard">
        <h4>🏅 High Scores:</h4>
        <ol id="highScores"></ol>
    </div>
</div>

<script>
const vocabulary = [
    { en: "Reading books", vi: "Đọc sách" },
    { en: "Watching TV", vi: "Xem truyền hình" },
    { en: "Listening to music", vi: "Nghe nhạc" },
    { en: "Playing video games", vi: "Chơi trò chơi điện tử" },
    { en: "Surfing the internet", vi: "Lướt mạng" },
    { en: "Cooking", vi: "Nấu ăn" },
    { en: "Baking", vi: "Làm bánh" },
    { en: "Drawing", vi: "Vẽ tranh" },
    { en: "Gardening", vi: "Làm vườn" },
    { en: "Knitting", vi: "Đan lát" }
];

let currentWord;
let score = 0;
let timeLeft = 10;
let timerInterval;

const correctSound = new Audio("https://www.soundjay.com/buttons/sounds/button-4.mp3");
const wrongSound = new Audio("https://www.soundjay.com/buttons/sounds/button-10.mp3");

function newQuestion() {
    const randomIndex = Math.floor(Math.random() * vocabulary.length);
    currentWord = vocabulary[randomIndex];
    document.getElementById("question").innerText = "Nghĩa: " + currentWord.vi;
    document.getElementById("answer").value = "";
}

function checkAnswer() {
    const userAnswer = document.getElementById("answer").value.trim();
    if (userAnswer.toLowerCase() === currentWord.en.toLowerCase()) {
        score++;
        correctSound.play();
        document.getElementById("result").innerText = "✅ Correct!";
    } else {
        wrongSound.play();
        document.getElementById("result").innerText = "❌ Wrong! Correct: " + currentWord.en;
    }
    document.getElementById("score").innerText = score;
    newQuestion();
}

function startTimer() {
    timerInterval = setInterval(() => {
        timeLeft--;
        document.getElementById("timer").innerText = "⏳ Time: " + timeLeft;
        if (timeLeft <= 0) {
            clearInterval(timerInterval);
            saveHighScore();
            alert("⏰ Time's up! Your score: " + score);
        }
    }, 1000);
}

function restartGame() {
    score = 0;
    timeLeft = 10;
    document.getElementById("score").innerText = score;
    document.getElementById("result").innerText = "";
    clearInterval(timerInterval);
    startTimer();
    newQuestion();
}

function saveHighScore() {
    let scores = JSON.parse(localStorage.getItem("highScores")) || [];
    scores.push(score);
    scores.sort((a,b) => b-a);
    scores = scores.slice(0,5);
    localStorage.setItem("highScores", JSON.stringify(scores));
    displayHighScores();
}

function displayHighScores() {
    let scores = JSON.parse(localStorage.getItem("highScores")) || [];
    let list = document.getElementById("highScores");
    list.innerHTML = "";
    scores.forEach(s => {
        let li = document.createElement("li");
        li.textContent = s;
        list.appendChild(li);
    });
}

displayHighScores();
startTimer();
newQuestion();
</script>

</body>
</html>
