<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vocabulary Game: Suddenly / Evening / Otherwise...</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">

    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }

        body {
            font-family: 'Segoe UI', Tahoma, sans-serif;
            background: linear-gradient(135deg, #009688 0%, #4CAF50 100%);
            min-height: 100vh;
            padding: 20px;
            color: #2c3e50;
            display: flex;
            justify-content: center;
            align-items: flex-start;
        }

        .container {
            max-width: 1100px;
            width: 100%;
            background: #fff;
            border-radius: 18px;
            box-shadow: 0 12px 30px rgba(0,0,0,0.25);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #00695c, #009688);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 { margin-bottom: 10px; }

        .nav-buttons {
            display: flex;
            justify-content: center;
            flex-wrap: wrap;
            gap: 10px;
            background: #f8f9fa;
            padding: 20px;
        }

        .nav-btn {
            padding: 12px 24px;
            border-radius: 22px;
            font-weight: bold;
            border: none;
            background: #fff;
            color: #444;
            cursor: pointer;
            transition: all 0.2s ease;
        }

        .nav-btn.active {
            background: #009688;
            color: #fff;
            transform: translateY(-2px);
        }

        .game-section {
            display: none;
            padding: 30px;
            min-height: 400px;
        }

        .game-section.active { display: block; }

        .word-bank {
            background: #e0f2f1;
            padding: 20px;
            border: 2px solid #009688;
            border-radius: 14px;
            margin-bottom: 25px;
        }

        .word-option {
            display: inline-block;
            background: #009688;
            color: white;
            padding: 8px 14px;
            border-radius: 20px;
            font-weight: bold;
            margin: 4px;
        }

        .question {
            background: #f8f9fa;
            padding: 18px;
            border-radius: 10px;
            margin-bottom: 15px;
            border-left: 5px solid #009688;
        }

        .fill-blank {
            border: 2px solid #ccc;
            padding: 6px 10px;
            border-radius: 8px;
            min-width: 140px;
        }

        .fill-blank-pron {
            border-bottom: 2px solid #00796b;
            font-weight: bold;
            min-width: 140px;
            display: inline-block;
            padding: 3px 2px;
        }

        .check-btn, .reset-btn {
            padding: 12px 22px;
            border-radius: 22px;
            border: none;
            cursor: pointer;
            margin-top: 10px;
            font-size: 15px;
        }

        .check-btn { background: #009688; color: #fff; }
        .reset-btn { background: #F44336; color: #fff; }

        .feedback {
            margin-top: 8px;
            padding: 10px;
            border-radius: 8px;
            display: none;
            font-weight: bold;
        }
        .feedback.correct { background: #E8F5E9; color: #2E7D32; }
        .feedback.incorrect { background: #FFEBEE; color: #C62828; }

        .score {
            position: fixed;
            top: 20px;
            right: 20px;
            background: #00796b;
            color: white;
            padding: 10px 18px;
            border-radius: 25px;
            font-weight: bold;
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }

        .vocabulary-list .vocabulary-item {
            background: #f8f9fa;
            padding: 18px;
            border-radius: 10px;
            margin-bottom: 16px;
        }

        .vocabulary-item h3 { margin-bottom: 6px; }

        .vocabulary-item button {
            margin-left: 8px;
            padding: 4px 10px;
            border-radius: 15px;
            border: none;
            cursor: pointer;
            font-size: 13px;
            background: #FF9800;
            color: white;
        }

        .match-container { display: flex; gap: 20px; }
        .match-col { width: 50%; }

        .match-item {
            background: #fff;
            padding: 12px;
            margin: 6px 0;
            border-radius: 10px;
            border: 2px solid #ddd;
            cursor: pointer;
        }

        .match-item.selected {
            background: #B2DFDB;
            border-color: #009688;
        }

        .match-item.matched {
            background: #4CAF50;
            color: white;
            border-color: #2E7D32;
            cursor: default;
        }

        .pron-btn {
            padding: 6px 10px;
            background: #00796b;
            color: white;
            border-radius: 18px;
            border: none;
            cursor: pointer;
            margin-left: 6px;
            display: inline-flex;
            align-items: center;
            gap: 4px;
        }

        @media (max-width: 768px) {
            .match-container { flex-direction: column; }
            .match-col { width: 100%; }
            .score { position: static; margin-bottom: 10px; text-align: center; }
        }
    </style>
</head>

<body>
<div class="score">Score: <span id="score">0</span>/24</div>

<div class="container">

    <div class="header">
        <h1>Vocabulary Practice</h1>
        <p>Words: suddenly, evening, otherwise, compared to, add up, cheeks, patent, fully</p>
    </div>

    <!-- NAVIGATION -->
    <div class="nav-buttons">
        <button class="nav-btn active" onclick="showSection('vocabulary', event)">Vocabulary List</button>
        <button class="nav-btn" onclick="showSection('writing', event)">Fill in the Gaps</button>
        <button class="nav-btn" onclick="showSection('matching', event)">Match Definitions</button>
        <button class="nav-btn" onclick="showSection('pronunciation', event)">Fill in the Gaps (Pronunciation)</button>
    </div>

    <!-- VOCABULARY LIST -->
    <div id="vocabulary" class="game-section active">
        <h2 style="text-align:center; margin-bottom:20px;">Vocabulary List</h2>
        <div class="vocabulary-list">

            <div class="vocabulary-item">
                <h3>suddenly (all’improvviso)
                    <button onclick="speak('suddenly')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> all’improvviso</p>
                <p>Suddenly, the lights went out.</p>
            </div>

            <div class="vocabulary-item">
                <h3>evening (sera)
                    <button onclick="speak('evening')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> sera</p>
                <p>I usually relax in the evening.</p>
            </div>

            <div class="vocabulary-item">
                <h3>otherwise (altrimenti)
                    <button onclick="speak('otherwise')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> altrimenti</p>
                <p>You must hurry, otherwise you’ll miss the train.</p>
            </div>

            <div class="vocabulary-item">
                <h3>compared to (rispetto a)
                    <button onclick="speak('compared to')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> rispetto a</p>
                <p>This room is big compared to mine.</p>
            </div>

            <div class="vocabulary-item">
                <h3>add up (sommare)
                    <button onclick="speak('add up')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> sommare</p>
                <p>Can you add up these numbers?</p>
            </div>

            <div class="vocabulary-item">
                <h3>cheeks (guance)
                    <button onclick="speak('cheeks')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> guance</p>
                <p>Her cheeks turned red.</p>
            </div>

            <div class="vocabulary-item">
                <h3>patent (brevetto)
                    <button onclick="speak('patent')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> brevetto</p>
                <p>He received a patent for his invention.</p>
            </div>

            <div class="vocabulary-item">
                <h3>fully (a pieno)
                    <button onclick="speak('fully')">▶️ Hear</button>
                </h3>
                <p><strong>Italian meaning:</strong> a pieno</p>
                <p>I fully understand your point.</p>
            </div>

        </div>
    </div>

    <!-- FILL IN THE GAPS (WRITING) -->
    <div id="writing" class="game-section">
        <div class="word-bank">
            <h3>Word Bank</h3>
            <span class="word-option">suddenly</span>
            <span class="word-option">evening</span>
            <span class="word-option">otherwise</span>
            <span class="word-option">compared to</span>
            <span class="word-option">add up</span>
            <span class="word-option">cheeks</span>
            <span class="word-option">patent</span>
            <span class="word-option">fully</span>
        </div>

        <div id="writing-container"></div>
        <button class="check-btn" onclick="checkWriting()">Check Answers</button>
        <button class="reset-btn" onclick="resetWriting()">Reset</button>
    </div>

    <!-- MATCHING -->
    <div id="matching" class="game-section">
        <h2 style="text-align:center; margin-bottom:15px;">Match Definitions</h2>

        <div class="match-container">
            <div class="match-col">
                <h3>Words</h3>
                <div class="match-item" data-word="suddenly" onclick="selectMatch(this)">suddenly</div>
                <div class="match-item" data-word="evening" onclick="selectMatch(this)">evening</div>
                <div class="match-item" data-word="otherwise" onclick="selectMatch(this)">otherwise</div>
                <div class="match-item" data-word="compared to" onclick="selectMatch(this)">compared to</div>
                <div class="match-item" data-word="add up" onclick="selectMatch(this)">add up</div>
                <div class="match-item" data-word="cheeks" onclick="selectMatch(this)">cheeks</div>
                <div class="match-item" data-word="patent" onclick="selectMatch(this)">patent</div>
                <div class="match-item" data-word="fully" onclick="selectMatch(this)">fully</div>
            </div>

            <div class="match-col">
                <h3>Definitions</h3>
                <div id="definitions-container"></div>
            </div>
        </div>

        <button class="reset-btn" onclick="resetMatching()">Reset</button>
    </div>

    <!-- PRONUNCIATION GAPS -->
    <div id="pronunciation" class="game-section">
        <div class="word-bank">
            <h3>Word Bank</h3>
            <span class="word-option">suddenly</span>
            <span class="word-option">evening</span>
            <span class="word-option">otherwise</span>
            <span class="word-option">compared to</span>
            <span class="word-option">add up</span>
            <span class="word-option">cheeks</span>
            <span class="word-option">patent</span>
            <span class="word-option">fully</span>
        </div>

        <h2 style="text-align:center; margin-bottom:10px;">Fill in the Gaps (Pronunciation)</h2>
        <p style="text-align:center; margin-bottom:15px;">🎤 Say the correct word aloud.</p>

        <div id="pron-container"></div>
    </div>

</div>

<script>
    // ---------- SPEECH SYNTHESIS ----------
    function speak(word) {
        const u = new SpeechSynthesisUtterance(word);
        u.lang = "en-US";
        speechSynthesis.speak(u);
    }

    // ---------- DATA ----------
    const writingSentences = [
        { text: "The car stopped <input class='fill-blank' data-answer='suddenly' placeholder='answer'> in the middle of the road.", answer: "suddenly" },
        { text: "We usually watch TV in the <input class='fill-blank' data-answer='evening' placeholder='answer'>.", answer: "evening" },
        { text: "You must wear a helmet, <input class='fill-blank' data-answer='otherwise' placeholder='answer'> it's dangerous.", answer: "otherwise" },
        { text: "This winter is warm <input class='fill-blank' data-answer='compared to' placeholder='answer'> last year.", answer: "compared to" },
        { text: "Can you <input class='fill-blank' data-answer='add up' placeholder='answer'> all these costs before the meeting?", answer: "add up" },
        { text: "Her <input class='fill-blank' data-answer='cheeks' placeholder='answer'> turned red when she was embarrassed.", answer: "cheeks" },
        { text: "The inventor applied for a <input class='fill-blank' data-answer='patent' placeholder='answer'> for his new machine.", answer: "patent" },
        { text: "I <input class='fill-blank' data-answer='fully' placeholder='answer'> understand this document.", answer: "fully" }
    ];

    const definitions = [
        { word:"suddenly", text:"happening quickly and unexpectedly" },
        { word:"evening", text:"the time of day after late afternoon" },
        { word:"otherwise", text:"if not; or else" },
        { word:"compared to", text:"used to show differences between things" },
        { word:"add up", text:"to sum numbers" },
        { word:"cheeks", text:"the soft parts of the face below the eyes" },
        { word:"patent", text:"legal protection for an invention" },
        { word:"fully", text:"completely; entirely" }
    ];

    // NEW sentences for pronunciation (different from writing)
    const pronSentences = [
        { before:"The music stopped ___ during the party.", answer:"suddenly" },
        { before:"They go for a walk every ___.", answer:"evening" },
        { before:"Finish your homework, ___ you can't play games.", answer:"otherwise" },
        { before:"My old phone was slow ___ this one.", answer:"compared to" },
        { before:"These numbers don't ___. There must be a mistake.", answer:"add up" },
        { before:"The cold wind made his ___ very pink.", answer:"cheeks" },
        { before:"Without a ___, someone could copy your idea.", answer:"patent" },
        { before:"The hotel was ___ booked for the weekend.", answer:"fully" }
    ];

    // ---------- STATE ----------
    let writingCorrect = 0;
    let matchingCorrect = 0;
    let pronCorrect = 0;

    let activeWritingSentences = [];
    let activePronSentences = [];
    let pronFlags = [];

    let matched = [];
    let selectedWord = null;
    let selectedDef = null;

    let recognition = null;
    if ('webkitSpeechRecognition' in window) {
        recognition = new webkitSpeechRecognition();
    } else if ('SpeechRecognition' in window) {
        recognition = new SpeechRecognition();
    }
    if (recognition) {
        recognition.lang = "en-US";
        recognition.interimResults = false;
        recognition.maxAlternatives = 1;
    }

    // ---------- HELPERS ----------
    function shuffle(arr) {
        for (let i = arr.length - 1; i > 0; i--) {
            const j = Math.floor(Math.random() * (i + 1));
            [arr[i], arr[j]] = [arr[j], arr[i]];
        }
        return arr;
    }

    function updateScore() {
        document.getElementById("score").textContent = writingCorrect + matchingCorrect + pronCorrect;
    }

    // ---------- WRITING ----------
    function loadWriting() {
        const container = document.getElementById("writing-container");
        container.innerHTML = "";
        activeWritingSentences = shuffle([...writingSentences]);

        activeWritingSentences.forEach((s, i) => {
            container.innerHTML += `
                <div class="question">
                    <p>${s.text}</p>
                    <div class="feedback" id="write-feed-${i}"></div>
                </div>
            `;
        });
    }

    function checkWriting() {
        const blanks = document.querySelectorAll("#writing .fill-blank");
        let correct = 0;
        activeWritingSentences.forEach((s, i) => {
            const val = (blanks[i].value || "").toLowerCase().trim();
            const fb = document.getElementById(`write-feed-${i}`);
            if (val === s.answer) {
                fb.textContent = "✔️ Correct!";
                fb.className = "feedback correct";
                fb.style.display = "block";
                correct++;
            } else {
                fb.textContent = `❌ Incorrect. Correct answer: "${s.answer}"`;
                fb.className = "feedback incorrect";
                fb.style.display = "block";
            }
        });
        writingCorrect = correct;
        updateScore();
    }

    function resetWriting() {
        loadWriting();
        writingCorrect = 0;
        updateScore();
    }

    // ---------- MATCHING ----------
    function loadDefinitions() {
        const container = document.getElementById("definitions-container");
        container.innerHTML = "";
        shuffle([...definitions]).forEach(d => {
            container.innerHTML += `
                <div class="match-item" data-meaning="${d.word}" onclick="selectMatch(this)">
                    ${d.text}
                </div>
            `;
        });
    }

    function selectMatch(el) {
        if (el.classList.contains("matched")) return;

        if (el.dataset.word) {
            if (selectedWord) selectedWord.classList.remove("selected");
            selectedWord = el;
            el.classList.add("selected");
        } else {
            if (selectedDef) selectedDef.classList.remove("selected");
            selectedDef = el;
            el.classList.add("selected");
        }

        if (selectedWord && selectedDef) {
            if (selectedWord.dataset.word === selectedDef.dataset.meaning) {
                selectedWord.classList.add("matched");
                selectedDef.classList.add("matched");
                matched.push(selectedWord.dataset.word);
            }
            selectedWord.classList.remove("selected");
            selectedDef.classList.remove("selected");
            selectedWord = null;
            selectedDef = null;

            matchingCorrect = matched.length;
            updateScore();
        }
    }

    function resetMatching() {
        matched = [];
        matchingCorrect = 0;
        updateScore();
        document.querySelectorAll("#matching .match-item").forEach(i => i.classList.remove("selected", "matched"));
        loadDefinitions();
    }

    // ---------- PRONUNCIATION ----------
    function loadPronunciation() {
        const container = document.getElementById("pron-container");
        container.innerHTML = "";
        activePronSentences = shuffle([...pronSentences]);
        pronFlags = activePronSentences.map(() => false);
        pronCorrect = 0;
        updateScore();

        activePronSentences.forEach((s, i) => {
            container.innerHTML += `
                <div class="question">
                    <p>
                        ${s.before.replace("___", `<span id="pron-blank-${i}" class="fill-blank-pron">______</span>`)}
                        <button class="pron-btn" onclick="recordPronunciation(${i})">
                            <i class="fas fa-microphone"></i> Speak
                        </button>
                    </p>
                    <div class="feedback" id="pron-feed-${i}"></div>
                </div>
            `;
        });
    }

    function recordPronunciation(index) {
        if (!recognition) {
            alert("Speech recognition is not supported in this browser.");
            return;
        }

        const fb = document.getElementById(`pron-feed-${index}`);
        fb.textContent = "Listening...";
        fb.className = "feedback";
        fb.style.display = "block";

        recognition.start();

        recognition.onresult = function(e) {
            const spoken = e.results[0][0].transcript.toLowerCase().trim();
            const expected = activePronSentences[index].answer.toLowerCase().trim();
            const blank = document.getElementById(`pron-blank-${index}`);

            if (spoken === expected) {
                fb.textContent = `✔️ Correct! You said "${spoken}".`;
                fb.className = "feedback correct";
                blank.textContent = activePronSentences[index].answer;
                blank.style.color = "green";

                if (!pronFlags[index]) {
                    pronFlags[index] = true;
                    pronCorrect = pronFlags.filter(Boolean).length;
                    updateScore();
                }
            } else {
                fb.textContent = `❌ You said "${spoken}". Try again.`;
                fb.className = "feedback incorrect";
            }
        };

        recognition.onerror = function() {
            fb.textContent = "❌ Error during recognition. Please try again.";
            fb.className = "feedback incorrect";
        };
    }

    // ---------- NAVIGATION ----------
    function showSection(id, evt) {
        document.querySelectorAll(".game-section").forEach(s => s.classList.remove("active"));
        document.getElementById(id).classList.add("active");

        document.querySelectorAll(".nav-btn").forEach(b => b.classList.remove("active"));
        if (evt && evt.target) {
            evt.target.classList.add("active");
        }

        if (id === "writing") {
            loadWriting();
        }
        if (id === "matching") {
            resetMatching();
        }
        if (id === "pronunciation") {
            loadPronunciation();
        }
    }

    // ---------- INIT ----------
    window.onload = function() {
        loadWriting();
        loadDefinitions();
        loadPronunciation();
        updateScore();
    };
</script>

</body>
</html>
