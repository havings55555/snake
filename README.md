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

        .controls {
            position: fixed;
            right: 20px;
            bottom: 20px;
        }

        .arrow-button {
            font-size: 20px;
            padding: 10px;
            margin: 5px;
            cursor: pointer;
        }

        .start-btn {
            font-size: 20px;
            padding: 15px;
            margin-top: 20px;
            cursor: pointer;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 5px;
        }

        .start-btn:hover {
            background-color: #45a049;
        }

        #gameArea {
            display: none;
        }
    </style>
</head>
<body>
    <h1>Snake Game</h1>
    <button class="start-btn" id="startBtn">Start Game</button>

    <div id="gameArea">
        <canvas id="gameCanvas" width="400" height="400"></canvas>
        <div class="controls">
            <button class="arrow-button" id="up">↑</button><br>
            <button class="arrow-button" id="left">←</button>
            <button class="arrow-button" id="right">→</button><br>
            <button class="arrow-button" id="down">↓</button>
        </div>
    </div>

    <script>
        const canvas = document.getElementById('gameCanvas');
        const ctx = canvas.getContext('2d');

        // 게임 설정
        const gridSize = 20; // 그리드 크기
        let snake = [{ x: 200, y: 200 }]; // 뱀 초기 위치
        let direction = { x: gridSize, y: 0 }; // 초기 방향 (오른쪽으로 시작)
        let nextDirection = { x: gridSize, y: 0 }; // 다음 방향
        let food = { x: 100, y: 100 }; // 음식 초기 위치
        let score = 0;

        // 게임 영역 숨기기
        const gameArea = document.getElementById('gameArea');
        const startBtn = document.getElementById('startBtn');

        // 음식 위치 랜덤 생성
        function generateFood() {
            food = {
                x: Math.floor((Math.random() * canvas.width) / gridSize) * gridSize,
                y: Math.floor((Math.random() * canvas.height) / gridSize) * gridSize,
            };
        }

        // 방향 버튼 클릭 처리
        document.getElementById('up').addEventListener('click', () => {
            if (direction.y === 0) nextDirection = { x: 0, y: -gridSize };
        });

        document.getElementById('down').addEventListener('click', () => {
            if (direction.y === 0) nextDirection = { x: 0, y: gridSize };
        });

        document.getElementById('left').addEventListener('click', () => {
            if (direction.x === 0) nextDirection = { x: -gridSize, y: 0 };
        });

        document.getElementById('right').addEventListener('click', () => {
            if (direction.x === 0) nextDirection = { x: gridSize, y: 0 };
        });

        // 키보드 화살표 입력 처리
        document.addEventListener('keydown', (e) => {
            switch (e.key) {
                case 'ArrowUp':
                    if (direction.y === 0) nextDirection = { x: 0, y: -gridSize };
                    break;
                case 'ArrowDown':
                    if (direction.y === 0) nextDirection = { x: 0, y: gridSize };
                    break;
                case 'ArrowLeft':
                    if (direction.x === 0) nextDirection = { x: -gridSize, y: 0 };
                    break;
                case 'ArrowRight':
                    if (direction.x === 0) nextDirection = { x: gridSize, y: 0 };
                    break;
            }
        });

        // 게임 업데이트
        function update() {
            direction = nextDirection; // 입력된 방향 적용

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

        // 게임 시작 버튼 클릭 시
        startBtn.addEventListener('click', () => {
            // 게임 영역 보이게 하기
            gameArea.style.display = 'block';

            // 시작 버튼 숨기기
            startBtn.style.display = 'none';

            generateFood();
            gameLoop();
        });
    </script>
</body>
</html>
