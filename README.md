# new-kuiz

<!DOCTYPE html>
<html lang="kk">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>HCI & UX Master Quiz</title>
    <script src="https://cdn.jsdelivr.net/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <script src="https://cdn.tailwindcss.com"></script>
</head>
<body class="bg-slate-900 text-slate-100 min-h-screen flex flex-col items-center justify-center p-4">

    <div class="w-full max-w-3xl bg-slate-800 rounded-2xl shadow-2xl p-6 md:p-8 border border-slate-700">
        <!-- Бас бөлім -->
        <div class="flex justify-between items-center mb-6 pb-4 border-b border-slate-700">
            <h1 class="text-2xl font-bold bg-gradient-to-r from-blue-400 to-indigo-400 bg-clip-text text-transparent">HCI & UX Quiz</h1>
            <div id="timer" class="text-sm font-semibold bg-slate-700 px-3 py-1.5 rounded-lg text-blue-300">⏱️ 00:00</div>
        </div>

        <!-- Сұрақтар аймағы -->
        <div id="quiz-container">
            <div class="flex justify-between text-sm text-slate-400 mb-2">
                <span id="question-progress">Сұрақ 1 / 75</span>
                <span id="score-counter">Ұпай: 0</span>
            </div>
            
            <div class="w-full bg-slate-700 h-2 rounded-full mb-6 overflow-hidden">
                <div id="progress-bar" class="bg-blue-500 h-full transition-all duration-300" style="width: 0%"></div>
            </div>

            <h2 id="question-text" class="text-xl font-semibold mb-6 text-slate-100">Сұрақ жүктелуде...</h2>

            <div id="options-container" class="space-y-3">
                <!-- Жауап нұсқалары осында шығады -->
            </div>
        </div>

        <!-- Қорытынды бет (Жасырын) -->
        <div id="result-container" class="hidden text-center py-8">
            <h2 class="text-3xl font-bold mb-4 text-emerald-400">Тест аяқталды! 🎉</h2>
            <div class="bg-slate-700/50 p-6 rounded-xl max-w-md mx-auto mb-6 space-y-3 border border-slate-600">
                <p id="final-score" class="text-xl text-slate-200 font-semibold">Итоговый балл: </p>
                <p id="correct-count" class="text-emerald-400 font-medium">✅ Дұрыс жауаптар: 0</p>
                <p id="incorrect-count" class="text-rose-400 font-medium">❌ Қате жауаптар: 0</p>
                <p id="accuracy-rate" class="text-blue-300 font-medium">📊 Дәлдік: 0%</p>
            </div>
            <button onclick="restartQuiz()" class="bg-blue-600 hover:bg-blue-700 text-white font-semibold px-6 py-3 rounded-xl transition-all shadow-lg">Қайта бастау</button>
        </div>
    </div>

    <script>
        const quizData = [
            {
                question: "What is included in a research plan?",
                options: ["Only competitive analysis", "Surveys, usability tests, focus groups or interviews, and website metrics analysis", "Only interviews", "Only a budget"],
                correct: 1
            },
            {
                question: "What are examples of user experience techniques that can contribute to competitive analysis?",
                options: ["Recruiting participants, interviews and observations, focus groups, usability tests, and surveys", "Only redesigns", "Only advertising and sales", "Only website metrics"],
                correct: 0
            },
            {
                question: "What should a product be easy to do?",
                options: ["Easy to compare", "Easy to learn and use", "Easy to redesign", "Easy to advertise"],
                correct: 1
            },
            {
                question: "What is a key success factor for website usability?",
                options: ["Making the choices clear and mindless", "Increasing the number of choices", "Adding more words", "Using dense paragraphs"],
                correct: 0
            },
            {
                question: "What percentage of the total project budget do most organizations spend on user design activities?",
                options: ["5%", "50%", "10%", "25%"],
                correct: 2
            },
            {
                question: "What does removing extra content make more prominent?",
                options: ["Useful content", "Competitors", "Support tickets", "Legal action"],
                correct: 0
            },
            {
                question: "What is key to maximizing usability ROI?",
                options: ["Early and continuous user research", "Increasing ad spend", "Writing more code", "Using complex layouts"],
                correct: 0
            },
            {
                question: "What is the Second Law of Usability called?",
                options: ["Research Structure", "Get Rid of Half the Words", "Competitive Analysis", "Mindless Choices"],
                correct: 1
            },
            {
                question: "What should be done if the competitor is not clear?",
                options: ["Stop the research", "Perform internet searches", "Remove the product", "Ignore the competitor"],
                correct: 1
            },
            {
                question: "What should a product be for the user?",
                options: ["Expensive and competitive", "Complex and detailed", "Long and informative", "Satisfying and productive"],
                correct: 3
            },
            {
                question: "What does removing extra content make the page?",
                options: ["More complicated", "More expensive", "Shorter", "Longer"],
                correct: 2
            },
            {
                question: "What should designers focus on when editing content?",
                options: ["Legal action", "Project schedules", "Competitor prices", "Essential information"],
                correct: 3
            },
            {
                question: "What does getting rid of extra content reduce?",
                options: ["Sales", "The clutter on a page", "User research", "Customer satisfaction"],
                correct: 1
            },
            {
                question: "What do unnecessary words create?",
                options: ["Customer loyalty", "Clarity", "Clutter", "Revenue"],
                correct: 2
            },
            {
                question: "What does a swimlane diagram mainly focus on?",
                options: ["One process and who does what in it", "All company finances", "Only software interfaces", "Only user emotions"],
                correct: 0
            },
            {
                question: "Which area focuses on thoughts, feelings, needs, and pains of key users?",
                options: ["Implementation", "Objectives", "Persona(s)", "Channels"],
                correct: 2
            },
            {
                question: "A User Experience Journey Map helps a team understand a use case through whose eyes?",
                options: ["The user", "The developer", "The investor", "The manager"],
                correct: 0
            },
            {
                question: "What does the 'See' section include?",
                options: ["Environment, family/friends, and market offers", "Only application code", "Only financial data", "Only employee roles"],
                correct: 0
            },
            {
                question: "What type of problem can journey mapping help avoid?",
                options: ["Password resets", "Assumption misalignments", "Internet connection failures", "Compiler errors"],
                correct: 1
            },
            {
                question: "What are swimlane diagrams sometimes called after Rummler and Brache?",
                options: ["Journey tables", "Persona diagrams", "Rummler-Brache diagrams", "Empathy charts"],
                correct: 2
            },
            {
                question: "What knowledge is key to conducting a User Experience Journey Map exercise?",
                options: ["Knowledge of programming languages only", "Knowledge of accounting", "Knowledge about the use case and the user", "Knowledge of network protocols"],
                correct: 2
            },
            {
                question: "What information is used to create personas according to the lecture?",
                options: ["Source code analysis", "Only sales reports", "User and stakeholder interviews", "Server logs only"],
                correct: 2
            },
            {
                question: "Who highlighted swimlane diagrams in the 1990 book Improving Processes?",
                options: ["Steve Jobs and Bill Gates", "Geary Rummler and Alan Brache", "Alan Turing and John von Neumann", "Don Norman and Jakob Nielsen"],
                correct: 1
            },
            {
                question: "What is a swimlane diagram?",
                options: ["A type of process flow diagram showing who does what at a high level", "A database table", "A website color palette", "A customer satisfaction survey"],
                correct: 0
            },
            {
                question: "What are swimlane diagrams called in Microsoft Office Visio according to the lecture?",
                options: ["Entity diagrams", "User journey diagrams", "Cross-functional diagrams", "Interaction maps"],
                correct: 2
            },
            {
                question: "What does 'The Person' provide in a Journey Map?",
                options: ["A network diagram", "A list of programming errors", "A point of view for users or people", "A financial forecast"],
                correct: 2
            },
            {
                question: "What can Opportunities help a team identify?",
                options: ["How to install an operating system", "How to rename files", "Which CPU to purchase", "Where the experience can be improved"],
                correct: 3
            },
            {
                question: "What kinds of processes were swimlane diagrams designed to depict?",
                options: ["Multi-person or cross-functional processes", "Only financial transactions", "Only individual tasks", "Only automated processes"],
                correct: 0
            },
            {
                question: "Which category describes what encourages a persona to act?",
                options: ["Source code", "Database schema", "Network settings", "Motivations"],
                correct: 3
            },
            {
                question: "In a customer/user journey map, what should be specified at the top?",
                options: ["Specific user, scenario, and goals", "Only employee names", "Only company revenue", "Only software version"],
                correct: 0
            },
            {
                question: "What is a Journey Map according to the quick definition?",
                options: ["A UX visualization document", "A financial report", "A database model", "A programming algorithm"],
                correct: 0
            },
            {
                question: "What should be drawn to create the process flow in a swimlane diagram?",
                options: ["Only user emotions", "Only business goals", "Only personas", "Processes and decisions"],
                correct: 3
            },
            {
                question: "What can be mapped throughout different journey stages?",
                options: ["Only financial costs", "Only device specifications", "Actions, mindset, and emotions", "Only programming languages"],
                correct: 2
            },
            {
                question: "Which area asks how accomplishments will be measured?",
                options: ["Metrics & Opportunities", "Persona(s)", "Stakeholders", "The Experience"],
                correct: 0
            },
            {
                question: "What does the process of creating a journey map encourage within a team?",
                options: ["Budget reduction only", "Hardware replacement", "Internal conversation", "Code compilation"],
                correct: 2
            },
            {
                question: "Which area of a journey map can show possible improvements?",
                options: ["IP addresses", "File names", "Opportunities", "Passwords"],
                correct: 2
            },
            {
                question: "What do separate lanes represent in a swimlane diagram?",
                options: ["Different file types", "Different passwords", "Unique participant roles in the process", "Different website colors"],
                correct: 2
            },
            {
                question: "In an empathy-oriented user description, what belongs to 'Think and Feel'?",
                options: ["Server capacity", "Screen dimensions", "Programming language", "Worries and aspirations"],
                correct: 3
            },
            {
                question: "What are 'Pains' in the user context?",
                options: ["Company profits", "Fears, frustrations, obstacles, and challenges", "Technical specifications", "Completed goals only"],
                correct: 1
            },
            {
                question: "What do Journey Phases describe?",
                options: ["High-level stages of the user's journey", "Only technical risks", "Only project costs", "Only user demographics"],
                correct: 0
            },
            {
                question: "What can be identified as a result of journey mapping and persona activities?",
                options: ["The number of developers required", "The cheapest server", "The most important functionality an audience needs", "The fastest programming language"],
                correct: 2
            },
            {
                question: "Which persona template area can include Instagram and newspapers?",
                options: ["Process decisions", "Project budget", "Technology errors", "Information channels"],
                correct: 3
            },
            {
                question: "What does a journey map help define clearly?",
                options: ["Pain points", "Database keys", "IP addresses", "Programming syntax"],
                correct: 0
            },
            {
                question: "Which persona template area can include mobile and tablet?",
                options: ["Main frustrations", "Internal ownership", "Technology use", "Business goals"],
                correct: 2
            },
            {
                question: "What is Step 3 in conducting a persona interview?",
                options: ["Understand Your Interview Goals", "Create a Scenario", "Prepare for Your Interview", "Select a Prototype"],
                correct: 2
            },
            {
                question: "What should early interview questions help establish?",
                options: ["A database schema", "A final design", "Trust", "A programming framework"],
                correct: 2
            },
            {
                question: "Which environmental element should a scenario consider?",
                options: ["Only screen resolution", "Only file size", "Only programming language", "Environment's conditions"],
                correct: 3
            },
            {
                question: "How should an interview preferably end?",
                options: ["With an open-ended question", "With a usability score", "With a technical specification", "With a programming task"],
                correct: 0
            },
            {
                question: "Which factor can limit or affect a persona in a scenario?",
                options: ["Code comments", "Constraints", "Compiler settings", "Database indexes"],
                correct: 1
            },
            {
                question: "What do scenarios help teams understand?",
                options: ["Only company finances", "Only hardware specifications", "Only programming syntax", "Practical needs and behaviors of users"],
                correct: 3
            },
            {
                question: "What can you do if it is difficult to find potential users?",
                options: ["Find people who can role-play or speak for a potential user", "Skip user research", "Interview only developers", "Cancel the research"],
                correct: 0
            },
            {
                question: "What is Step 1 in conducting a persona interview?",
                options: ["Prepare for Your Interview", "Plan Your Interview", "Create a Use Case", "Understand Your Interview Goals"],
                correct: 3
            },
            {
                question: "How can user stories help developers?",
                options: ["They help developers understand users", "They replace system requirements", "They remove the need for users", "They automatically generate all code"],
                correct: 0
            },
            {
                question: "Which factor may interrupt or affect the user in a scenario?",
                options: ["IP addresses", "Source files", "Class names", "Distractions"],
                correct: 3
            },
            {
                question: "Compared with a user story, a use case usually follows:",
                options: ["Only one short sentence", "A larger and richer template", "A visual style guide", "No structure at all"],
                correct: 1
            },
            {
                question: "What should be done before conducting an interview?",
                options: ["Plan and practice the interview", "Only prepare technical terms", "Avoid planning questions", "Create the final interface"],
                correct: 0
            },
            {
                question: "Which additional stakeholders may be considered for interviews?",
                options: ["Only competitors", "Only investors", "Only programmers", "Administrators and managers"],
                correct: 3
            },
            {
                question: "How should question complexity generally progress?",
                options: ["All questions should be equally complex", "From simple to more complex", "From complex to simple only", "Only complex questions should be used"],
                correct: 1
            },
            {
                question: "What should you learn about users' current activities during an interview?",
                options: ["How developers write code", "How the database is structured", "How the company advertises", "How they currently accomplish the task"],
                correct: 3
            },
            {
                question: "Why do scenarios create a 'rich picture' of events?",
                options: ["To help teams understand user interactions in context", "To focus only on technical code", "To remove user behavior from design", "To reduce all stories to one sentence"],
                correct: 0
            },
            {
                question: "How can user stories affect testing?",
                options: ["They can make testing more realistic", "They make testing unrelated to users", "They prevent testing", "They only test hardware"],
                correct: 0
            },
            {
                question: "What does a use case describe in HCI?",
                options: ["Only a persona's biography", "A step-by-step interaction between an actor and a system to achieve a goal", "Only interface colors", "Only a business budget"],
                correct: 1
            },
            {
                question: "What should a scenario describe about progress toward the goal?",
                options: ["Only the final screen", "Steps from beginning to end", "Only technical errors", "Only the first step"],
                correct: 1
            },
            {
                question: "Who should be interviewed when possible?",
                options: ["Only developers", "Potential users", "Only designers", "Only company owners"],
                correct: 1
            },
            {
                question: "What should designers do after creating scenarios?",
                options: ["Delete the persona", "Stop all user research", "Avoid revisions", "Iterate"],
                correct: 3
            },
            {
                question: "How should the interviewer demonstrate interest?",
                options: ["By interrupting frequently", "By answering every question for the user", "By showing interest in what the interviewee says and why", "By avoiding follow-up discussion"],
                correct: 2
            },
            {
                question: "Which information should interview goals help you discover?",
                options: ["The background of the people being interviewed", "The programming language of the system", "The color of the final interface", "The price of computer hardware"],
                correct: 0
            },
            {
                question: "What is a user scenario?",
                options: ["A hardware installation guide", "A database backup", "A detailed description of a realistic situation involving a user", "A list of programming commands"],
                correct: 2
            },
            {
                question: "Can a scenario describe failure to achieve a goal?",
                options: ["Yes, it can describe failure as well as success", "Only if there is no persona", "No, scenarios can only describe success", "Only after the system is released"],
                correct: 0
            },
            {
                question: "What time-related information should an opening statement provide?",
                options: ["The time needed to program the system", "An estimate of how long the interview will last", "The date of product release", "The user's age"],
                correct: 1
            },
            {
                question: "How does a more diverse target user population affect interview planning?",
                options: ["Fewer people are needed", "No interviews are needed", "Only managers should be interviewed", "More people may need to be interviewed"],
                correct: 3
            },
            {
                question: "What mix of questions is recommended for interviews?",
                options: ["Only open-ended questions", "Only closed-ended questions", "Closed-ended and open-ended questions", "Only yes/no questions"],
                correct: 2
            },
            {
                question: "In the 'Add a Volunteer' use case, who is the actor?",
                options: ["System administrator from another company", "Software developer", "New volunteer", "WFI staff member"],
                correct: 3
            },
            {
                question: "What type of information should early interview questions obtain?",
                options: ["Server logs", "Financial passwords", "Background information", "Source code"],
                correct: 2
            }
        ];

        let currentIdx = 0;
        let score = 0;
        let correctCount = 0;
        let incorrectCount = 0;
        let seconds = 0;
        let timerInterval;

        const questionText = document.getElementById("question-text");
        const optionsContainer = document.getElementById("options-container");
        const questionProgress = document.getElementById("question-progress");
        const scoreCounter = document.getElementById("score-counter");
        const progressBar = document.getElementById("progress-bar");
        const quizContainer = document.getElementById("quiz-container");
        const resultContainer = document.getElementById("result-container");
        const finalScore = document.getElementById("final-score");
        const correctCountElem = document.getElementById("correct-count");
        const incorrectCountElem = document.getElementById("incorrect-count");
        const accuracyRateElem = document.getElementById("accuracy-rate");
        const timerElement = document.getElementById("timer");

        function startTimer() {
            timerInterval = setInterval(() => {
                seconds++;
                let m = Math.floor(seconds / 60).toString().padStart(2, '0');
                let s = (seconds % 60).toString().padStart(2, '0');
                timerElement.innerText = `⏱️ ${m}:${s}`;
            }, 1000);
        }

        function loadQuestion() {
            if (currentIdx >= quizData.length) {
                endQuiz();
                return;
            }

            let q = quizData[currentIdx];
            questionText.innerText = q.question;
            questionProgress.innerText = `Сұрақ ${currentIdx + 1} / ${quizData.length}`;
            scoreCounter.innerText = `Ұпай: ${score}`;
            progressBar.style.width = `${((currentIdx) / quizData.length) * 100}%`;

            optionsContainer.innerHTML = "";
            q.options.forEach((opt, idx) => {
                let btn = document.createElement("button");
                btn.className = "w-full text-left p-4 rounded-xl bg-slate-700 hover:bg-slate-600 border border-slate-600 transition-all font-medium flex justify-between items-center";
                
                let spanText = document.createElement("span");
                spanText.innerText = opt;
                btn.appendChild(spanText);

                btn.onclick = () => selectAnswer(idx, btn);
                optionsContainer.appendChild(btn);
            });
        }

        function selectAnswer(selectedIdx, btn) {
            let q = quizData[currentIdx];
            let allBtns = optionsContainer.children;

            // Жауап берілген соң барлық түймелерді бұғаттау және дұрыс жауапты көрсету
            Array.from(allBtns).forEach((b, idx) => {
                b.disabled = true;
                if (idx === q.correct) {
                    b.classList.remove("bg-slate-700", "border-slate-600");
                    b.classList.add("bg-emerald-600/90", "border-emerald-500", "text-white");
                    let badge = document.createElement("span");
                    badge.className = "text-xs bg-emerald-800 text-white px-2 py-1 rounded ml-2 font-bold";
                    badge.innerText = "Дұрыс";
                    b.appendChild(badge);
                }
            });

            if (selectedIdx === q.correct) {
                score += 10;
                correctCount++;
            } else {
                btn.classList.remove("bg-slate-700", "border-slate-600");
                btn.classList.add("bg-rose-600/90", "border-rose-500", "text-white");
                let badge = document.createElement("span");
                badge.className = "text-xs bg-rose-800 text-white px-2 py-1 rounded ml-2 font-bold";
                badge.innerText = "Сіздікі";
                btn.appendChild(badge);
                incorrectCount++;
            }

            setTimeout(() => {
                currentIdx++;
                loadQuestion();
            }, 1500);
        }

        function endQuiz() {
            clearInterval(timerInterval);
            quizContainer.classList.add("hidden");
            resultContainer.classList.remove("hidden");
            
            let total = quizData.length;
            let accuracy = Math.round((correctCount / total) * 100);

            finalScore.innerText = `Жиналған ұпай: ${score} / ${total * 10}`;
            correctCountElem.innerText = `✅ Дұрыс жауаптар: ${correctCount}`;
            incorrectCountElem.innerText = `❌ Қате жауаптар: ${incorrectCount}`;
            accuracyRateElem.innerText = `📊 Дәлдік көрсеткіші: ${accuracy}%`;
            
            confetti({
                particleCount: 120,
                spread: 70,
                origin: { y: 0.6 }
            });
        }

        function restartQuiz() {
            currentIdx = 0;
            score = 0;
            correctCount = 0;
            incorrectCount = 0;
            seconds = 0;
            quizContainer.classList.remove("hidden");
            resultContainer.classList.add("hidden");
            startTimer();
            loadQuestion();
        }

        startTimer();
        loadQuestion();
    </script>
</body>
</html>
