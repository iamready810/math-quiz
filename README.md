<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0"/>
  <title>7th Grade Math Quiz</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background: linear-gradient(to bottom, #f2f2f2, #e6f0ff);
      margin: 0;
      padding: 20px;
      display: flex;
      flex-direction: column;
      align-items: center;
    }

    .quiz-container {
      background: white;
      padding: 20px 30px;
      border-radius: 10px;
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
      width: 100%;
      max-width: 600px;
      position: relative;
    }

    h1 {
      text-align: center;
      margin-bottom: 10px;
      color: #333;
    }

    .question {
      font-size: 18px;
      margin: 20px 0 10px;
    }

    .options {
      list-style: none;
      padding: 0;
    }

    .options li {
      margin-bottom: 10px;
    }

    label {
      cursor: pointer;
    }

    input[type="radio"] {
      margin-right: 10px;
    }

    button {
      padding: 10px 20px;
      margin-top: 20px;
      font-size: 16px;
      background-color: #4caf50;
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
    }

    button:hover {
      background-color: #388e3c;
    }

    .result {
      text-align: center;
      font-size: 20px;
      margin-top: 20px;
    }

    .restart-btn {
      background-color: #2196f3;
    }

    .restart-btn:hover {
      background-color: #1976d2;
    }

    .progress-bar {
      height: 10px;
      width: 100%;
      background-color: #eee;
      border-radius: 5px;
      overflow: hidden;
      margin-top: 10px;
    }

    .progress {
      height: 100%;
      background-color: #4caf50;
      width: 0%;
      transition: width 0.3s;
    }

    .prev-score {
      text-align: center;
      font-size: 14px;
      color: #777;
      margin-top: 10px;
    }
  </style>
</head>
<body>
  <div class="quiz-container">
    <h1>📘 7th Grade Math Quiz</h1>
    <p><strong>Instructions:</strong> Select the best answer for each question.</p>
    <div class="progress-bar"><div id="progress" class="progress"></div></div>
    <div id="quiz"></div>
    <button id="nextBtn">Next</button>
    <div id="result" class="result"></div>
    <div id="prevScore" class="prev-score"></div>
    <button id="restartBtn" class="restart-btn" style="display: none;">Restart Quiz</button>
  </div>

  <script>
    const quizData = [
      { question: "1. What is 5/8 + 7/12?", options: ["41/48", "71/48", "47/48", "35/48"], answer: "47/48" },
      { question: "2. What is −3×(−4)+5÷(−1)?", options: ["-17", "7", "-7", "17"], answer: "-7" },
      { question: "3. If 2/3 ÷ 5/6 equals x, what is x × 3/4?", options: ["6/10", "8/15", "4/5", "9/10"], answer: "4/5" },
      { question: "4. What is 35% of 240?", options: ["72", "84", "96", "108"], answer: "84" },
      { question: "5. What is 5/8 − 7/12?", options: ["1/24", "-1/24", "11/24", "-11/24"], answer: "-11/24" },
      { question: "6. If a $120 item is discounted by 25% and then an additional 10%, what is the final price?", options: ["$81", "$90", "$85", "$80"], answer: "$81" },
      { question: "7. What is 3/4 ÷ 9/16?", options: ["4/3", "12/9", "16/9", "9/16"], answer: "4/3" },
      { question: "8. What is −2×(−3)+(−4)÷2?", options: ["-4", "4", "-2", "2"], answer: "2" },
      { question: "9. What is 7/9 + 5/6?", options: ["41/36", "47/36", "29/36", "35/36"], answer: "47/36" },
      { question: "10. If 40% of a number is 32, what is the number?", options: ["80", "64", "100", "120"], answer: "80" },
      { question: "11. What is 5/6 ÷ 2/3?", options: ["15/12", "5/4", "10/9", "6/5"], answer: "5/4" },
      { question: "12. What is 18% of 450?", options: ["72", "81", "90", "108"], answer: "81" },
      { question: "13. What is 11/15 − 7/10?", options: ["-1/30", "1/30", "-2/30", "2/30"], answer: "1/30" },
      { question: "14. If a $200 item is discounted by 15% and then an additional 20%, what is the final price?", options: ["$136", "$140", "$150", "$160"], answer: "$136" }
    ];

    let currentQuestion = 0;
    let score = 0;

    const quiz = document.getElementById("quiz");
    const nextBtn = document.getElementById("nextBtn");
    const result = document.getElementById("result");
    const progress = document.getElementById("progress");
    const restartBtn = document.getElementById("restartBtn");
    const prevScore = document.getElementById("prevScore");

    function loadQuestion() {
      const q = quizData[currentQuestion];
      quiz.innerHTML = `
        <div class="question">${q.question}</div>
        <ul class="options">
          ${q.options.map(option => `
            <li>
              <label>
                <input type="radio" name="answer" value="${option}"/>
                ${option}
              </label>
            </li>`).join("")}
        </ul>
      `;

      // Update progress bar
      progress.style.width = ((currentQuestion / quizData.length) * 100) + "%";
    }

    function getSelectedAnswer() {
      const answers = document.getElementsByName("answer");
      for (let a of answers) {
        if (a.checked) return a.value;
      }
      return null;
    }

    nextBtn.addEventListener("click", () => {
      const selected = getSelectedAnswer();
      if (!selected) {
        alert("Please select an answer.");
        return;
      }

      if (selected === quizData[currentQuestion].answer) {
        score++;
      }

      currentQuestion++;

      if (currentQuestion < quizData.length) {
        loadQuestion();
      } else {
        quiz.style.display = "none";
        nextBtn.style.display = "none";
        result.innerHTML = `✅ You got <strong>${score}</strong> out of ${quizData.length} correct!`;
        progress.style.width = "100%";
        localStorage.setItem("lastScore", score);
        restartBtn.style.display = "inline-block";
      }
    });

    restartBtn.addEventListener("click", () => {
      currentQuestion = 0;
      score = 0;
      quiz.style.display = "block";
      nextBtn.style.display = "inline-block";
      result.innerHTML = "";
      restartBtn.style.display = "none";
      loadQuestion();
    });

    function showPreviousScore() {
      const last = localStorage.getItem("lastScore");
      if (last !== null) {
        prevScore.innerText = `📝 Last time you scored ${last}/${quizData.length}`;
      }
    }

    loadQuestion();
    showPreviousScore();
  </script>
</body>
</html>
