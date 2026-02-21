

 ## 🎮 Jogo do Super Mario no Perfil!

<div align="center">
<canvas id="gameCanvas" width="800" height="200" style="border:1px solid #000; background: skyblue;"></canvas>
<button onclick="jump()" style="font-size:20px;">Pular (Espaço)</button>
<p>Score: <span id="score">0</span></p>
</div>

<script>
const canvas = document.getElementById('gameCanvas');
const ctx = canvas.getContext('2d');
let mario = { x: 50, y: 150, vy: 0, jumping: false };
let obstacle = { x: 800, y: 150, width: 50, height: 50 };
let score = 0;
let gameRunning = true;

function drawMario() {
  ctx.fillStyle = 'red';
  ctx.fillRect(mario.x, mario.y, 30, 50);
}

function drawObstacle() {
  ctx.fillStyle = 'green';
  ctx.fillRect(obstacle.x, obstacle.y, obstacle.width, obstacle.height);
}

function update() {
  if (!gameRunning) return;
  ctx.clearRect(0, 0, canvas.width, canvas.height);
  
  // Mario gravity/jump
  if (mario.jumping) mario.vy += 0.5;
  mario.y += mario.vy;
  if (mario.y > 150) { mario.y = 150; mario.jumping = false; mario.vy = 0; }
  
  // Obstacle move
  obstacle.x -= 5;
  if (obstacle.x < -50) { obstacle.x = 800; score++; document.getElementById('score').textContent = score; }
  
  // Collision
  if (mario.x < obstacle.x + obstacle.width && mario.x + 30 > obstacle.x &&
      mario.y < obstacle.y + obstacle.height && mario.y + 50 > obstacle.y) {
    gameRunning = false;
    alert('Game Over! Score: ' + score + '\nClique para reiniciar.');
    location.reload();
  }
  
  drawMario();
  drawObstacle();
  requestAnimationFrame(update);
}

function jump() {
  if (!mario.jumping) { mario.jumping = true; mario.vy = -12; }
}

document.addEventListener('keydown', (e) => { if (e.code === 'Space') jump(); });
update();
</script>
