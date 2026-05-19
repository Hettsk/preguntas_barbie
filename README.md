<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Barbie Interactive Dreamhouse</title>
    <style>
        :root {
            --barbie-pink: #ff69b4;
            --light-pink: #ffc0cb;
            --deep-pink: #e0218a;
            --white: #ffffff;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background-color: var(--light-pink);
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            margin: 0;
        }

        #game-container {
            background-color: var(--white);
            padding: 30px;
            border-radius: 20px;
            box-shadow: 0 10px 25px rgba(0,0,0,0.1);
            border: 5px solid var(--barbie-pink);
            width: 90%;
            max-width: 500px;
            text-align: center;
        }

        h1 {
            color: var(--deep-pink);
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .question {
            font-size: 1.2rem;
            margin-bottom: 20px;
            color: #333;
        }

        .options-grid {
            display: grid;
            gap: 10px;
        }

        button {
            background-color: var(--barbie-pink);
            color: white;
            border: none;
            padding: 12px;
            border-radius: 10px;
            cursor: pointer;
            font-size: 1rem;
            transition: transform 0.2s, background-color 0.2s;
        }

        button:hover {
            background-color: var(--deep-pink);
            transform: scale(1.02);
        }

        #feedback {
            margin-top: 20px;
            font-weight: bold;
            min-height: 24px;
        }

        .success { color: #28a745; }
        .error { color: #dc3545; }

        .hidden { display: none; }
    </style>
</head>
<body>

<div id="game-container">
    <h1>Dreamhouse Quiz</h1>
    <div id="quiz-content">
        <p id="question-text" class="question"></p>
        <div id="options" class="options-grid"></div>
        <p id="feedback"></p>
    </div>
    <div id="final-screen" class="hidden">
        <h2>¡Increíble!</h2>
        <p>Has completado el tour por la casa de Barbie.</p>
        <button onclick="resetGame()">Jugar de nuevo</button>
    </div>
</div>

<script>
    const questions = [
        {
            q: "¿En qué año fue creada Barbie?",
            options: ["2000", "1877", "1959"],
            correct: "1959"
        },
        {
            q: "¿Quién la creó?",
            options: ["Steve Jobs", "Ole Kirk Christiansen", "Ruth Handler"],
            correct: "Ruth Handler"
        },
        {
            q: "¿Cuáles son algunas de las profesiones de Barbie?",
            options: ["Astronauta", "Presidente", "Doctora y Veterinaria", "Todas las anteriores"],
            correct: "Todas las anteriores"
        }
    ];

    let currentQuestionIndex = 0;

    function loadQuestion() {
        const feedback = document.getElementById('feedback');
        feedback.textContent = "";
        
        if (currentQuestionIndex >= questions.length) {
            showFinalScreen();
            return;
        }

        const currentQ = questions[currentQuestionIndex];
        document.getElementById('question-text').textContent = currentQ.q;
        
        const optionsContainer = document.getElementById('options');
        optionsContainer.innerHTML = '';

        currentQ.options.forEach(option => {
            const btn = document.createElement('button');
            btn.textContent = option;
            btn.onclick = () => checkAnswer(option);
            optionsContainer.appendChild(btn);
        });
    }

    function checkAnswer(selected) {
        const feedback = document.getElementById('feedback');
        const correct = questions[currentQuestionIndex].correct;

        if (selected === correct) {
            feedback.textContent = "¡Correcto! ✨";
            feedback.className = "success";
            setTimeout(() => {
                currentQuestionIndex++;
                loadQuestion();
            }, 1500);
        } else {
            feedback.textContent = "Inténtalo de nuevo 🎀";
            feedback.className = "error";
        }
    }

    function showFinalScreen() {
        document.getElementById('quiz-content').classList.add('hidden');
        document.getElementById('final-screen').classList.remove('hidden');
    }

    function resetGame() {
        currentQuestionIndex = 0;
        document.getElementById('quiz-content').classList.remove('hidden');
        document.getElementById('final-screen').classList.add('hidden');
        loadQuestion();
    }

    // Iniciar el juego
    loadQuestion();
</script>

</body>
</html>
