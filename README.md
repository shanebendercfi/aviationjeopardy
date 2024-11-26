<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Jeopardy-style Game</title>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f8f8f8;
            text-align: center;
        }
        h1 {
            background-color: #004f91;
            color: white;
            padding: 20px;
            font-size: 2.5em;
            margin: 0;
        }
        .game-board {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            gap: 10px;
            max-width: 1200px;
            margin: 30px auto;
        }
        .category {
            background-color: #004f91;
            color: white;
            font-weight: bold;
            padding: 15px;
            font-size: 1.5em;
        }
        .question-cell {
            background-color: #f4f4f4;
            border: 3px solid #004f91;
            font-size: 1.5em;
            padding: 20px;
            cursor: pointer;
            transition: background-color 0.3s;
        }
        .question-cell:hover {
            background-color: #d1e3f0;
        }
        .question-cell.selected {
            background-color: #d1e3f0;
        }
        .question-cell:active {
            background-color: #004f91;
            color: white;
        }
        .answer-box {
            font-size: 1.2em;
            background-color: #ffff99;
            padding: 20px;
            border-radius: 10px;
            display: none;
            margin-top: 10px;
        }
    </style>
</head>
<body>

    <h1>Airplane Flying Handbook Jeopardy</h1>

    <!-- Audio Background -->
    <audio id="bg-music" loop>
        <source src="https://www.soundjay.com/button/beep-07.wav" type="audio/wav">
    </audio>

    <div class="game-board">
        <!-- Category Titles -->
        <div class="category">General Information</div>
        <div class="category">Four Forces</div>
        <div class="category">Aircraft Axes</div>
        <div class="category">Flight Performance</div>
        <div class="category">Safety and Coordination</div>

        <!-- Question Cells (clickable) -->
        <div class="question-cell" onclick="showAnswer('general1')">$100</div>
        <div class="question-cell" onclick="showAnswer('forces1')">$100</div>
        <div class="question-cell" onclick="showAnswer('axes1')">$100</div>
        <div class="question-cell" onclick="showAnswer('performance1')">$100</div>
        <div class="question-cell" onclick="showAnswer('safety1')">$100</div>

        <div class="question-cell" onclick="showAnswer('general2')">$200</div>
        <div class="question-cell" onclick="showAnswer('forces2')">$200</div>
        <div class="question-cell" onclick="showAnswer('axes2')">$200</div>
        <div class="question-cell" onclick="showAnswer('performance2')">$200</div>
        <div class="question-cell" onclick="showAnswer('safety2')">$200</div>

        <div class="question-cell" onclick="showAnswer('general3')">$300</div>
        <div class="question-cell" onclick="showAnswer('forces3')">$300</div>
        <div class="question-cell" onclick="showAnswer('axes3')">$300</div>
        <div class="question-cell" onclick="showAnswer('performance3')">$300</div>
        <div class="question-cell" onclick="showAnswer('safety3')">$300</div>

        <div class="question-cell" onclick="showAnswer('general4')">$400</div>
        <div class="question-cell" onclick="showAnswer('forces4')">$400</div>
        <div class="question-cell" onclick="showAnswer('axes4')">$400</div>
        <div class="question-cell" onclick="showAnswer('performance4')">$400</div>
        <div class="question-cell" onclick="showAnswer('safety4')">$400</div>

        <div class="question-cell" onclick="showAnswer('general5')">$500</div>
        <div class="question-cell" onclick="showAnswer('forces5')">$500</div>
        <div class="question-cell" onclick="showAnswer('axes5')">$500</div>
        <div class="question-cell" onclick="showAnswer('performance5')">$500</div>
        <div class="question-cell" onclick="showAnswer('safety5')">$500</div>
    </div>

    <div id="answer-box" class="answer-box"></div>

    <script>
        // List of answers
        var answers = {
            'general1': "The primary purpose of the *Airplane Flying Handbook* is to provide information on basic flight principles and techniques.",
            'general2': "The FAA sets standards for flight training and certification of pilots.",
            'general3': "The four primary forces are Lift, Weight, Thrust, and Drag.",
            'general4': "Proper coordination ensures the aircraft responds predictably, reducing risks like uncoordinated turns or stalls.",
            'general5': "The *Airplane Flying Handbook* is organized in chapters covering flight principles, techniques, and emergency procedures.",
            
            'forces1': "Lift counteracts the weight of the airplane.",
            'forces2': "Drag resists the aircraft's motion through the air.",
            'forces3': "Thrust is the forward force produced by the engine(s).",
            'forces4': "When lift is greater than weight, the aircraft climbs.",
            'forces5': "The four primary forces are Lift, Weight, Thrust, and Drag.",
            
            'axes1': "The longitudinal axis runs from nose to tail.",
            'axes2': "The lateral axis runs from wingtip to wingtip.",
            'axes3': "Roll happens around the longitudinal axis.",
            'axes4': "The vertical axis runs vertically through the aircraft's center.",
            'axes5': "Yaw happens around the vertical axis.",
            
            'performance1': "Weight, altitude, weather conditions, and aircraft configuration affect performance.",
            'performance2': "Higher altitude leads to thinner air, affecting engine performance and lift.",
            'performance3': "Weight and balance affect the aircraft’s handling and performance.",
            'performance4': "Weather conditions like wind, temperature, and air density can impact flight performance.",
            'performance5': "Configuration elements like flaps, landing gear, and trim can impact performance.",
            
            'safety1': "Preflight inspections ensure the aircraft is safe to operate.",
            'safety2': "Situational awareness helps pilots anticipate potential issues before they arise.",
            'safety3': "During maneuvers, pilots must be aware of the aircraft’s response and ensure it remains stable.",
            'safety4': "Emergency procedures provide pilots with a clear course of action during unexpected events.",
            'safety5': "Uncoordinated flight occurs when the aircraft’s control surfaces are not properly aligned with flight direction."
        };

        // Show the answer when a question is selected
        function showAnswer(questionId) {
            var answerBox = document.getElementById("answer-box");
            answerBox.style.display = "block";
            answerBox.textContent = answers[questionId];

            // Highlight the selected question
            var selectedQuestion = document.querySelector(".question-cell.selected");
            if (selectedQuestion) {
                selectedQuestion.classList.remove("selected");
            }
            var currentQuestion = document.querySelector(`.question-cell[onclick="showAnswer('${questionId}')"]`);
            currentQuestion.classList.add("selected");

            // Play background music
            var music = document.getElementById("bg-music");
            music.play();
        }

        // Play background music when the page loads
        window.onload = function() {
            var music = document.getElementById("bg-music");
            music.play();
        };
    </script>

</body>
</html>
