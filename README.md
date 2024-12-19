<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Snake Game</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            margin: 0;
            padding: 0;
            background-color: #f0f0f0;
        }

        h1 {
            margin: 20px 0;
            color: #333;
        }

        canvas {
            border: 2px solid #333;
            background-color: #fff;
            margin: auto;
            display: block;
        }
    </style>
</head>
<body>
    <h1>Snake Game</h1>
    <canvas id="gameCanvas" width="400" height="400"></canvas>
    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // 게임 설정
        const gridSize = 20; // 그리드 크기
        let snake = [{ x: 200, y: 200 }]; // 뱀 초기 위치
        let direction = { x: 0, y: 0 }; // 초기 방향
        let food = { x: 100, y: 100 }; // 음식 초기 위치
        let score = 0;

        // 음식 위치 랜덤 생성
        function generateFood() {
            food = {
                x: Math.floor((Math.random() * canvas.width) / gridSize) * gridSize,
                y: Math.floor((Math.random() * canvas.height) / gridSize) * gridSize,
            };
        }

        // 키 입력 처리
        document.addEventListener('keydown', (e) => {
            switch (e.key) {
                case 'ArrowUp':
                    if (direction.y === 0) direction = { x: 0, y: -gridSize };
                    break;
                case 'ArrowDown':
                    if (direction.y === 0) direction = { x: 0, y: gridSize };
                    break;
                case 'ArrowLeft':
                    if (direction.x === 0) direction = { x: -gridSize, y: 0 };
                    break;
                case 'ArrowRight':
                    if (direction.x === 0) direction = { x: gridSize, y: 0 };
                    break;
            }
        });

        // 게임 업데이트
        function update() {
            const head = {
                x: snake[0].x + direction.x,
                y: snake[0].y + direction.y,
            };

            // 벽 충돌 체크
            if (
                head.x < 0 ||
                head.y < 0 ||
                head.x >= canvas.width ||
                head.y >= canvas.height
            ) {
                alert(`Game Over! Your Score: ${score}`);
                document.location.reload();
            }

            // 자기 자신 충돌 체크
            for (let segment of snake) {
                if (head.x === segment.x && head.y === segment.y) {
                    alert(`Game Over! Your Score: ${score}`);
                    document.location.reload();
                }
            }

            snake.unshift(head);

            // 음식 먹기
            if (head.x === food.x && head.y === food.y) {
                score++;
                generateFood();
            } else {
                snake.pop(); // 뱀 길이 유지
            }
        }

        // 화면 그리기
        function draw() {
            ctx.clearRect(0, 0, canvas.width, canvas.height);

            // 음식 그리기
            ctx.fillStyle = 'red';
            ctx.fillRect(food.x, food.y, gridSize, gridSize);

            // 뱀 그리기
            ctx.fillStyle = 'green';
            for (let segment of snake) {
                ctx.fillRect(segment.x, segment.y, gridSize, gridSize);
            }
        }

        // 게임 루프
        function gameLoop() {
            update();
            draw();
            setTimeout(gameLoop, 100); // 속도 조절 (ms)
        }

        generateFood();
        gameLoop();
    </script>
</body>
</html>
