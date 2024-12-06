
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snack Catcher Game</title>
    <style>
        canvas {
            display: block;
            margin: 0 auto;
            background: #f5f5dc;
        }
        body {
            text-align: center;
            font-family: Arial, sans-serif;
        }
    </style>
</head>
<body>
    <h1>Snack Catcher Game</h1>
    <p>Use the arrow keys to move the basket and catch the snacks!</p>
    <canvas id="gameCanvas" width="400" height="600"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // Game variables
        let basket = { x: 175, y: 550, width: 50, height: 20 };
        let snack = { x: Math.random() * 350, y: 0, radius: 10 };
        let score = 0;
        let gameOver = false;

        // Draw the basket
        function drawBasket() {
            ctx.fillStyle = '#8B4513';
            ctx.fillRect(basket.x, basket.y, basket.width, basket.height);
        }

        // Draw the snack
        function drawSnack() {
            ctx.beginPath();
            ctx.arc(snack.x, snack.y, snack.radius, 0, Math.PI * 2);
            ctx.fillStyle = '#FF4500';
            ctx.fill();
            ctx.closePath();
        }

        // Move the basket with arrow keys
        document.addEventListener('keydown', (e) => {
            if (e.key === 'ArrowLeft' && basket.x > 0) {
                basket.x -= 20;
            } else if (e.key === 'ArrowRight' && basket.x < canvas.width - basket.width) {
                basket.x += 20;
            }
        });

        // Check for collisions
        function checkCollision() {
            if (
                snack.y + snack.radius >= basket.y &&
                snack.x >= basket.x &&
                snack.x <= basket.x + basket.width
            ) {
                score++;
                resetSnack();
            } else if (snack.y > canvas.height) {
                gameOver = true;
            }
        }

        // Reset snack position
        function resetSnack() {
            snack.x = Math.random() * 350;
            snack.y = 0;
        }

        // Update game logic
        function update() {
            if (gameOver) {
                alert(`Game Over! Your Score: ${score}`);
                document.location.reload();
                return;
            }
            snack.y += 5; // Move the snack down
            checkCollision();
        }

        // Draw the game elements
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);
            drawBasket();
            drawSnack();

            // Draw score
            ctx.fillStyle = '#000';
            ctx.font = '20px Arial';
            ctx.fillText(`Score: ${score}`, 10, 30);
        }

        // Game loop
        function gameLoop() {
            update();
            draw();
            requestAnimationFrame(gameLoop);
        }

        gameLoop();
    </script>
</body>
</html>
