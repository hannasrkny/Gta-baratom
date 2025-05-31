# Gta-baratom
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <title>GTA Barátom Edition</title>
  <style>
    canvas {
      background: #2e2e2e;
      display: block;
      margin: 0 auto;
      border: 3px solid #00ffcc;
    }
  </style>
</head>
<body>
<canvas id="gameCanvas" width="600" height="400"></canvas>
<script>
const canvas = document.getElementById("gameCanvas");
const ctx = canvas.getContext("2d");

let player = {
  x: 50,
  y: 50,
  size: 20,
  color: "#00ffcc",
  speed: 2,
};

const buildings = [
  { x: 100, y: 100, width: 80, height: 80, name: "House 1" },
  { x: 220, y: 100, width: 80, height: 80, name: "House 2" },
  { x: 340, y: 100, width: 80, height: 80, name: "Shop" },
];

let insideBuilding = null;

function drawPlayer() {
  ctx.fillStyle = player.color;
  ctx.fillRect(player.x, player.y, player.size, player.size);
}

function drawBuildings() {
  buildings.forEach((b) => {
    ctx.fillStyle = insideBuilding === b ? "#555" : "#999";
    ctx.fillRect(b.x, b.y, b.width, b.height);
    ctx.fillStyle = "#fff";
    ctx.font = "12px Arial";
    ctx.fillText(b.name, b.x + 5, b.y + 15);
  });
}

function detectEnterBuilding() {
  buildings.forEach((b) => {
    if (
      player.x < b.x + b.width &&
      player.x + player.size > b.x &&
      player.y < b.y + b.height &&
      player.y + player.size > b.y
    ) {
      insideBuilding = b;
    }
  });
}

function clearCanvas() {
  ctx.clearRect(0, 0, canvas.width, canvas.height);
}

let keys = {};
document.addEventListener("keydown", (e) => {
  keys[e.key] = true;

  if (e.key === "Enter" && insideBuilding) {
    alert("Beléptél ide: " + insideBuilding.name);
  }
});

document.addEventListener("keyup", (e) => {
  keys[e.key] = false;
});

function update() {
  if (keys["w"]) player.y -= player.speed;
  if (keys["s"]) player.y += player.speed;
  if (keys["a"]) player.x -= player.speed;
  if (keys["d"]) player.x += player.speed;

  insideBuilding = null;
  detectEnterBuilding();
}

function gameLoop() {
  clearCanvas();
  update();
  drawBuildings();
  drawPlayer();
  requestAnimationFrame(gameLoop);
}

gameLoop();
</script>
</body>
</html>
