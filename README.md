# survey

<!DOCTYPE html>
<html>
<head>
    <title>Dynamic Survey with Skip Logic</title>
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <script>
        let questions = [
            { id: 1, text: "What is your favorite color?", options: ["Red", "Blue", "Green"], next: 2 },
            { id: 2, text: "Do you have a pet?", options: ["Yes", "No"], next: { Yes: 3, No: 4 } },
            { id: 3, text: "What kind of pet do you have?", options: ["Dog", "Cat", "Other"], next: 5 },
            { id: 4, text: "Why don't you have a pet?", options: ["Allergy", "No time", "Other"], next: 5 },
            { id: 5, text: "How did you hear about us?", options: ["Friends", "Internet", "Other"], next: null }
        ];

        let currentQuestionIndex = 0;
        let answers = [];

        function showQuestion(index) {
            let question = questions[index];
            let questionContainer = document.getElementById("questionContainer");
            let optionsContainer = document.getElementById("optionsContainer");

            questionContainer.innerHTML = question.text;
            optionsContainer.innerHTML = "";

            question.options.forEach(option => {
                let button = document.createElement("button");
                button.innerHTML = option;
                button.onclick = () => {
                    answers.push({ question: question.text, answer: option });
                    let nextIndex = question.next;

                    if (typeof nextIndex === 'object') {
                        nextIndex = nextIndex[option];
                    }

                    if (nextIndex !== null) {
                        showQuestion(nextIndex - 1);
                    } else {
                        showSummary();
                    }
                };
                optionsContainer.appendChild(button);
            });
        }

        function showSummary() {
            let summaryContainer = document.getElementById("summaryContainer");
            summaryContainer.innerHTML = "<h2>Summary</h2>";
            answers.forEach(answer => {
                summaryContainer.innerHTML += `<p>Q: ${answer.question} <br> A: ${answer.answer}</p>`;
            });
        }

        window.onload = function() {
            showQuestion(currentQuestionIndex);
        };
    </script>
</head>
<body>
    <div id="questionContainer"></div>
    <div id="optionsContainer"></div>
    <div id="summaryContainer"></div>
</body>
</html>