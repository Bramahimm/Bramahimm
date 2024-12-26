<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Bounce Game</title>
    <style>
        body {
            margin: 0;
            padding: 0;
            background: linear-gradient(120deg, #1a1a2e, #16213e, #0f3460);
            color: #ffffff;
            font-family: Arial, sans-serif;
            text-align: center;
            overflow: hidden;
        }

        canvas {
            display: block;
            margin: 20px auto;
            border: 2px solid white;
            background-color: #000;
        }

        h1 {
            color: #00f7ff;
            text-shadow: 0 0 5px #00f7ff, 0 0 10px #00f7ff;
        }
    </style>
</head>

<body>
    <h1>Bounce Game</h1>
    <canvas id="gameKanvas" width="800" height="600"></canvas>
    <script>
        const canvas = document.getElementById('gameKanvas');
        const konteks = canvas.getContext('2d');

        let bola = { x: canvas.width / 2, y: canvas.height / 2, radius: 10, dx: 3, dy: -3 };
        let pedal = { width: 100, height: 10, x: canvas.width / 2 - 50, y: canvas.height - 20, speed: 7, dx: 0 };
        let isGameOver = false;

        function drawBola() {
            konteks.beginPath();
            konteks.arc(bola.x, bola.y, bola.radius, 0, Math.PI * 2);
            konteks.fillStyle = "#00f7ff";
            konteks.fill();
            konteks.closePath();
        }

        function drawPedal() {
            konteks.fillStyle = "#00f7ff";
            konteks.fillRect(pedal.x, pedal.y, pedal.width, pedal.height);
        }

        function movePedal() {
            pedal.x += pedal.dx;
            if (pedal.x < 0) pedal.x = 0;
            if (pedal.x + pedal.width > canvas.width) pedal.x = canvas.width - pedal.width;
        }

        function moveBola() {
            bola.x += bola.dx;
            bola.y += bola.dy;

            if (bola.x - bola.radius < 0 || bola.x + bola.radius > canvas.width) {
                bola.dx *= -1;
            }
            if (bola.y - bola.radius < 0) {
                bola.dy *= -1;
            }

            if (
                bola.x > pedal.x &&
                bola.x < pedal.x + pedal.width &&
                bola.y + bola.radius > pedal.y
            ) {
                bola.dy *= -1;
            }

            if (bola.y - bola.radius > canvas.height) {
                isGameOver = true;
            }
        }

        function drawGameOver() {
            konteks.fillStyle = "red";
            konteks.font = "30px Arial";
            konteks.textAlign = "center";
            konteks.fillText("Permainan Selesai!", canvas.width / 2, canvas.height / 2);
        }

        function draw() {
            konteks.clearRect(0, 0, canvas.width, canvas.height);
            drawBola();
            drawPedal();
        }

        function update() {
            if (isGameOver) {
                drawGameOver();
                return;
            }

            movePedal();
            moveBola();
            draw();

            requestAnimationFrame(update);
        }

        document.addEventListener("keydown", (e) => {
            if (e.key === "ArrowLeft") pedal.dx = -pedal.speed;
            if (e.key === "ArrowRight") pedal.dx = pedal.speed;
        });

        document.addEventListener("keyup", (e) => {
            if (e.key === "ArrowLeft" || e.key === "ArrowRight") pedal.dx = 0;
        });

        update();

    </script>
</body>

</html>
