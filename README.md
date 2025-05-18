<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>SmartFlash OMR+ Fixed Layout</title>
  <link href="https://fonts.googleapis.com/css2?family=Poppins:wght@400;600&display=swap" rel="stylesheet" />
  <style>
    * {
      box-sizing: border-box;
    }
    body {
      font-family: 'Poppins', sans-serif;
      background: linear-gradient(to right, #e3f2fd, #e1bee7);
      margin: 0;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      min-height: 100vh;
    }
    .card-container {
      width: 360px;
      perspective: 1200px;
      /* Prevent container from shrinking too small */
      min-height: 320px;
    }
    .flashcard {
      position: relative;
      width: 100%;
      transition: transform 0.8s;
      transform-style: preserve-3d;
      cursor: default;
    }
    .flipped {
      transform: rotateY(180deg);
    }
    .face {
      position: relative;
      width: 100%;
      padding: 20px;
      background: white;
      border-radius: 20px;
      box-shadow: 0 10px 25px rgba(0,0,0,0.15);
      backface-visibility: hidden;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      /* Make sure text wraps and stays inside */
      overflow-wrap: break-word;
      word-break: break-word;
    }
    .back {
      background: #dcedc8;
      transform: rotateY(180deg);
      position: relative;
    }
    h2 {
      margin: 0 0 15px;
      color: #333;
      font-weight: 600;
      font-size: 1.4rem;
      max-width: 100%;
      text-align: center;
      overflow-wrap: break-word;
      word-break: break-word;
      white-space: normal;
    }
    .options {
      width: 100%;
    }
    .option {
      background: #f1f1f1;
      padding: 12px;
      margin: 8px 0;
      border-radius: 12px;
      cursor: pointer;
      transition: all 0.3s ease;
      text-align: center;
      user-select: none;
      /* Keep options inside card */
      overflow-wrap: break-word;
      word-break: break-word;
      white-space: normal;
      max-width: 100%;
    }
    .option:hover {
      background: #c5cae9;
    }
    .option.correct {
      background-color: #a5d6a7;
    }
    .option.incorrect {
      background-color: #ef9a9a;
    }
    .option.disabled {
      pointer-events: none;
    }
    button {
      margin-top: 20px;
      padding: 10px 20px;
      font-weight: bold;
      border: none;
      background: #7e57c2;
      color: #fff;
      border-radius: 10px;
      cursor: pointer;
      transition: background 0.3s ease;
      user-select: none;
      display: block;
      width: 100%;
    }
    button:hover {
      background: #5e35b1;
    }
  </style>
</head>
<body>
  <div class="card-container">
    <div class="flashcard" id="flashcard">
      <div class="face front">
        <h2 id="question">Loading...</h2>
        <div class="options" id="options"></div>
      </div>
      <div class="face back">
        <h2 id="answer">Answer here</h2>
      </div>
    </div>
    <button onclick="nextCard()">Next</button>
  </div>

  <script>
    const data = [
      {
        question: "What is the boiling point of water in degrees Celsius at standard atmospheric pressure?",
        options: ["90°C", "100°C", "110°C", "120°C"],
        correct: "100°C"
      },
      { question: "Capital of Japan?", options: ["Seoul", "Beijing", "Tokyo", "Bangkok"], correct: "Tokyo" },
      {
        question: "HTML stands for?",
        options: [
          "Hyper Type Multi Language",
          "HyperText Markup Language",
          "HighText Machine Language",
          "None"
        ],
        correct: "HyperText Markup Language"
      },
      {
        question: "Planet known as the Red Planet, famous for its reddish appearance caused by iron oxide on its surface?",
        options: ["Venus", "Saturn", "Mars", "Jupiter"],
        correct: "Mars"
      },
      { question: "CSS is used for?", options: ["Logic", "Content", "Design", "Database"], correct: "Design" },
      {
        question: "Which JavaScript method is used to select an element by its ID?",
        options: ["getElementByClass", "getElementById", "querySelectorAll", "getElementsByTagName"],
        correct: "getElementById"
      },
      {
        question: "What does OMR stand for in examination systems?",
        options: [
          "Optical Mark Recognition",
          "Online Marking Resource",
          "Official Math Review",
          "Output Module Recorder"
        ],
        correct: "Optical Mark Recognition"
      },
      {
        question: "Which gas is most abundant in the Earth's atmosphere?",
        options: ["Oxygen", "Nitrogen", "Carbon Dioxide", "Argon"],
        correct: "Nitrogen"
      },
      {
        question: "The chemical symbol 'Na' stands for which element?",
        options: ["Sodium", "Nitrogen", "Neon", "Nickel"],
        correct: "Sodium"
      },
      {
        question: "Who wrote the novel 'To Kill a Mockingbird', a classic of modern American literature?",
        options: ["Harper Lee", "J.K. Rowling", "Mark Twain", "Ernest Hemingway"],
        correct: "Harper Lee"
      }
    ];

    let current = 0;
    const flashcard = document.getElementById('flashcard');
    const questionEl = document.getElementById('question');
    const optionsEl = document.getElementById('options');
    const answerEl = document.getElementById('answer');

    function loadCard() {
      flashcard.classList.remove('flipped');
      const q = data[current];
      questionEl.textContent = q.question;
      answerEl.textContent = "Answer: " + q.correct;
      optionsEl.innerHTML = '';

      q.options.forEach(opt => {
        const btn = document.createElement('div');
        btn.className = 'option';
        btn.textContent = opt;
        btn.onclick = () => checkAnswer(btn, opt, q.correct);
        optionsEl.appendChild(btn);
      });
    }

    function checkAnswer(btn, selected, correct) {
      const all = document.querySelectorAll('.option');
      all.forEach(opt => {
        opt.classList.add('disabled');
        if (opt.textContent === correct) opt.classList.add('correct');
        else if (opt.textContent === selected) opt.classList.add('incorrect');
      });
      setTimeout(() => {
        flashcard.classList.add('flipped');
      }, 700);
    }

    function nextCard() {
      current = (current + 1) % data.length;
      loadCard();
    }

    loadCard();
  </script>
</body>
</html>
