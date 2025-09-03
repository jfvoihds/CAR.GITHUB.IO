<!DOCTYPE html>
<html lang="ar">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>لعبة عربيات</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background-color: #eee;
        }
        #gameArea {
            position: relative;
            width: 100%;
            height: 500px;
            background-color: #333;
            overflow: hidden;
        }
        #car {
            position: absolute;
            bottom: 20px;
            left: 50%;
            width: 50px;
            height: 100px;
            background-color: red;
            border-radius: 10px;
        }
        .obstacle {
            position: absolute;
            width: 50px;
            height: 100px;
            background-color: green;
            border-radius: 10px;
        }
    </style>
</head>
<body>

<div id="gameArea">
    <div id="car"></div>
</div>

<script>
    // تعيين بعض المتغيرات
    let gameArea = document.getElementById("gameArea");
    let car = document.getElementById("car");
    let carPosition = { left: 150, bottom: 20 }; // موضع السيارة
    let obstacles = [];
    let isGameOver = false;
    
    // تحريك السيارة باستخدام الأسهم
    document.addEventListener("keydown", function(event) {
        if (isGameOver) return;

        if (event.key === "ArrowLeft") {
            carPosition.left = Math.max(0, carPosition.left - 10);
        } else if (event.key === "ArrowRight") {
            carPosition.left = Math.min(gameArea.offsetWidth - car.offsetWidth, carPosition.left + 10);
        }

        car.style.left = carPosition.left + "px";
    });

    // توليد العقبات
    function createObstacle() {
        let obstacle = document.createElement("div");
        obstacle.classList.add("obstacle");
        let randomLeft = Math.floor(Math.random() * (gameArea.offsetWidth - 50));
        obstacle.style.left = randomLeft + "px";
        obstacle.style.bottom = "0px";
        gameArea.appendChild(obstacle);
        obstacles.push(obstacle);
    }

    // تحريك العقبات
    function moveObstacles() {
        if (isGameOver) return;

        for (let i = 0; i < obstacles.length; i++) {
            let obstacle = obstacles[i];
            let currentBottom = parseInt(obstacle.style.bottom);
            if (currentBottom > gameArea.offsetHeight) {
                obstacle.remove();
                obstacles.splice(i, 1);
                i--;
            } else {
                obstacle.style.bottom = currentBottom + 5 + "px";
            }
        }

        checkCollisions();
    }

    // التحقق من الاصطدامات
    function checkCollisions() {
        for (let i = 0; i < obstacles.length; i++) {
            let obstacle = obstacles[i];
            let carRect = car.getBoundingClientRect();
            let obstacleRect = obstacle.getBoundingClientRect();
            
            if (carRect.left < obstacleRect.left + obstacleRect.widt
