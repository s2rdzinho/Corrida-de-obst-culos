const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

// Jogador
let player = {
  x: 50,
  y: 250,
  width: 40,
  height: 40,
  vy: 0,
  jumping: false,
};

const gravity = 1.2;
const ground = 250;
let obstacles = [];
let frame = 0;
let score = 0;
let gameOver = false;

// Desenha o jogador
function drawPlayer() {
  ctx.fillStyle = "#333";
  ctx.fillRect(player.x, player.y, player.width, player.height);
}

// Desenha os obstáculos
function drawObstacle(obs) {
  ctx.fillStyle = "#cc0000";
  ctx.fillRect(obs.x, obs.y, obs.width, obs.height);
}

// Desenha a pontuação
function drawScore() {
  ctx.fillStyle = "#000";
  ctx.font = "20px Arial";
  ctx.fillText(`Score: ${score}`, 650, 30);
}

// Reinicia o jogo
function resetGame() {
  obstacles = [];
  frame = 0;
  score = 0;
  player.y = ground;
  player.vy = 0;
  gameOver = false;
  loop();
}

// Loop principal
function loop() {
  if (gameOver) return;

  ctx.clearRect(0, 0, canvas.width, canvas.height);

  // chão
  ctx.fillStyle = "#999";
  ctx.fillRect(0, ground + player.height, canvas.width, 10);

  drawPlayer();

  // gravidade
  if (player.y < ground) {
    player.vy += gravity;
    player.y += player.vy;
  } else {
    player.y = ground;
    player.vy = 0;
    player.jumping = false;
  }

  // gera obstáculos
  if (frame % 90 === 0) {
    let height = 30 + Math.random() * 40;
    obstacles.push({
      x: canvas.width,
      y: ground + player.height - height,
      width: 20,
      height: height,
    });
  }

  // movimenta e desenha obstáculos
  obstacles.forEach((obs, index) => {
    obs.x -= 6;
    drawObstacle(obs);

    // colisão
    if (
      player.x < obs.x + obs.width &&
      player.x + player.width > obs.x &&
      player.y + player.height > obs.y
    ) {
      gameOver = true;
      ctx.fillStyle = "red";
      ctx.font = "30px Arial";
      ctx.fillText("💀 Game Over 💀", 300, 150);
      ctx.fillText("Toque ou pressione espaço para reiniciar", 170, 200);
    }

    // remove obstáculos antigos e aumenta score
    if (obs.x + obs.width < 0) {
      obstacles.splice(index, 1);
      score++;
    }
  });

  drawScore();
  frame++;

  if (!gameOver) requestAnimationFrame(loop);
}

// Faz o personagem pular
function jump() {
  if (!player.jumping) {
    player.vy = -18;
    player.jumping = true;
  }
}

// Controles
window.addEventListener("keydown", (e) => {
  if (e.code === "Space" || e.code === "ArrowUp") {
    if (gameOver) resetGame();
    else jump();
  }
});

// Toque na tela (mobile)
canvas.addEventListener("touchstart", () => {
  if (gameOver) resetGame();
  else jump();
});

// Inicia o jogo
loop();
