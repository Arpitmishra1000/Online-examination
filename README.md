# Online-examination
# login page 
# index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Examination - Login</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>Login</h2>
        <form id="loginForm">
            <input type="text" id="username" placeholder="Username" required>
            <input type="password" id="password" placeholder="Password" required>
            <button type="submit">Login</button>
        </form>
    </div>

    <script src="script.js"></script>
</body>
</html>

# exam.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Online Examination</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="container">
        <h2>Exam</h2>
        <div id="timer">60:00</div>
        <form id="examForm">
            <!-- Questions will be populated here -->
            <div id="questions"></div>
            <button type="submit">Submit</button>
            <button type="button" onclick="logout()">Logout</button>
        </form>
    </div>

    <script src="exam.js"></script>
</body>
</html>

# style.css
body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f4;
    padding: 20px;
}

.container {
    max-width: 400px;
    margin: auto;
    padding: 20px;
    background: white;
    border-radius: 5px;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

h2 {
    text-align: center;
}

input {
    width: 100%;
    padding: 10px;
    margin: 10px 0;
}

button {
    width: 100%;
    padding: 10px;
    background: #007bff;
    color: white;
    border: none;
    cursor: pointer;
}

button:hover {
    background: #0056b3;
}
# script.js
document.getElementById('loginForm').addEventListener('submit', function(event) {
    event.preventDefault();
    
    const username = document.getElementById('username').value;
    const password = document.getElementById('password').value;

    // Simulate a login request (replace with actual API call)
    if (username === "user" && password === "pass") {
        window.location.href = 'exam.html'; // Redirect to exam page
    } else {
        alert('Invalid credentials!');
    }
});
# exam.js
let timeLeft = 3600; // 1 hour in seconds

const questions = [
    { question: "What is 2 + 2?", options: ["3", "4", "5"], answer: 1 },
    { question: "What is the capital of France?", options: ["Berlin", "Madrid", "Paris"], answer: 2 },
];

function loadQuestions() {
    const questionsDiv = document.getElementById("questions");
    questions.forEach((q, index) => {
        const questionHTML = `
            <div>
                <p>${q.question}</p>
                ${q.options.map((option, i) => `
                    <label>
                        <input type="radio" name="question${index}" value="${i}"> ${option}
                    </label>
                `).join('')}
            </div>
        `;
        questionsDiv.innerHTML += questionHTML;
    });
}

function startTimer() {
    const timer = setInterval(() => {
        if (timeLeft <= 0) {
            clearInterval(timer);
            alert("Time's up! Submitting your exam.");
            submitExam();
        } else {
            const minutes = Math.floor(timeLeft / 60);
            const seconds = timeLeft % 60;
            document.getElementById("timer").innerHTML = `${minutes}:${seconds < 10 ? '0' : ''}${seconds}`;
            timeLeft--;
        }
    }, 1000);
}

function submitExam() {
    const answers = Array.from(document.querySelectorAll('input[type="radio"]:checked')).map(input => input.value);
    console.log("Submitted Answers:", answers);
    alert("Exam submitted!");
}

function logout() {
    window.location.href = 'index.html'; // Redirect to login page
}

document.addEventListener('DOMContentLoaded', () => {
    loadQuestions();
    startTimer();
});
# Backened code ( server.js )
const express = require('express');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
app.use(cors());
app.use(bodyParser.json());

let users = [
    { username: 'user', password: 'pass' }
];

app.post('/login', (req, res) => {
    const { username, password } = req.body;
    const user = users.find(u => u.username === username && u.password === password);
    if (user) {
        res.status(200).send({ message: 'Login successful' });
    } else {
        res.status(401).send({ message: 'Invalid credentials' });
    }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
    console.log(`Server is running on port ${PORT}`);
});
#  THANK YOU 

