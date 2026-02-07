mock-test-website/
│
├── index.html
├── style.css
└── script.js<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>Mock Test Platform</title>
  <link rel="stylesheet" href="style.css" />
</head>
<body>

  <header>
    <h1>Mock Learning</h1>
    <p>Practice • Improve • Succeed</p>
  </header>

  <main id="app">
    <div id="start-screen">
      <h2>Maths Mock Test</h2>
      <p>10 questions • 10 minutes</p>
      <button onclick="startTest()">Start Test</button>
    </div>

    <div id="test-screen" class="hidden">
      <div class="top-bar">
        <span id="timer">10:00</span>
        <span id="progress"></span>
      </div>

      <h2 id="question"></h2>

      <div id="options"></div>

      <button id="next-btn" onclick="nextQuestion()">Next</button>
    </div>

    <div id="result-screen" class="hidden">
      <h2>Your Result</h2>
      <p id="score"></p>
      <button onclick="location.reload()">Try Again</button>
    </div>
  </main>

  <script src="script.js"></script>
</body>
</html>body {
  font-family: "Segoe UI", sans-serif;
  background: #f4f7fb;
  margin: 0;
}

header {
  background: #4f46e5;
  color: white;
  padding: 20px;
  text-align: center;
}

main {
  max-width: 600px;
  margin: 30px auto;
  background: white;
  padding: 25px;
  border-radius: 12px;
  box-shadow: 0 10px 30px rgba(0,0,0,0.1);
}

.hidden {
  display: none;
}

button {
  background: #4f46e5;
  color: white;
  border: none;
  padding: 12px 20px;
  border-radius: 8px;
  cursor: pointer;
  font-size: 16px;
}

button:hover {
  background: #4338ca;
}

#options button {
  display: block;
  width: 100%;
  margin: 10px 0;
  background: #eef2ff;
  color: #1e1b4b;
}

#options button.selected {
  background: #c7d2fe;
}

.top-bar {
  display: flex;
  justify-content: space-between;
  margin-bottom: 15px;
  font-weight: bold;
}
const questions = [
  {
    question: "What is 12 × 8?",
    options: ["96", "84", "88", "108"],
    answer: 0
  },
  {
    question: "What is 45 ÷ 5?",
    options: ["7", "8", "9", "10"],
    answer: 2
  },
  {
    question: "What is 15% of 200?",
    options: ["20", "25", "30", "35"],
    answer: 2
  }
];

let currentQuestion = 0;
let score = 0;
let time = 600;
let timerInterval;

function startTest() {
  document.getElementById("start-screen").classList.add("hidden");
  document.getElementById("test-screen").classList.remove("hidden");
  loadQuestion();
  startTimer();
}

function loadQuestion() {
  const q = questions[currentQuestion];
  document.getElementById("question").innerText = q.question;
  document.getElementById("progress").innerText =
    `Question ${currentQuestion + 1} of ${questions.length}`;

  const optionsDiv = document.getElementById("options");
  optionsDiv.innerHTML = "";

  q.options.forEach((opt, index) => {
    const btn = document.createElement("button");
    btn.innerText = opt;
    btn.onclick = () => selectAnswer(btn, index);
    optionsDiv.appendChild(btn);
  });
}

let selectedAnswer = null;

function selectAnswer(button, index) {
  selectedAnswer = index;
  document.querySelectorAll("#options button").forEach(btn =>
    btn.classList.remove("selected")
  );
  button.classList.add("selected");
}

function nextQuestion() {
  if (selectedAnswer === null) return;

  if (selectedAnswer === questions[currentQuestion].answer) {
    score++;
  }

  selectedAnswer = null;
  currentQuestion++;

  if (currentQuestion < questions.length) {
    loadQuestion();
  } else {
    endTest();
  }
}

function startTimer() {
  timerInterval = setInterval(() => {
    time--;
    const minutes = Math.floor(time / 60);
    const seconds = time % 60;
    document.getElementById("timer").innerText =
      `${minutes}:${seconds.toString().padStart(2, "0")}`;

    if (time <= 0) endTest();
  }, 1000);
}

function endTest() {
  clearInterval(timerInterval);
  document.getElementById("test-screen").classList.add("hidden");
  document.getElementById("result-screen").classList.remove("hidden");
  document.getElementById("score").innerText =
    `You scored ${score} out of ${questions.length}`;
}
