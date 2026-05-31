# NFT-Boxing-Game
This game is inspired by NFT culture, where digital characters like BAYC-style monkeys and CryptoPunk-style avatars represent unique, collectible identities in a playful blockchain-driven art world.
let player, enemy;
let gameMsg = "";
let cooldown = 0;

function setup() {
  createCanvas(windowWidth, windowHeight);
  textFont("monospace");

  player = {
    name: "BAYCand 🐵",
    x: width * 0.25,
    y: height * 0.65,
    hp: 100,
    action: "idle"
  };

  enemy = {
    name: "CryptoPunk 👾",
    x: width * 0.75,
    y: height * 0.65,
    hp: 100,
    action: "idle"
  };
}

function draw() {
  background(20);

  drawArena();
  drawFighters();
  drawUI();

  if (cooldown > 0) cooldown--;

  if (player.hp <= 0 || enemy.hp <= 0) {
    drawGameOver();
    noLoop();
  }
}

function drawArena() {
  fill(40);
  rect(0, height * 0.5, width, height * 0.5);

  fill(255);
  textSize(16);
  textAlign(CENTER);
  text("🥊 TAP PUNCH / BLOCK TO DEFEND", width / 2, 20);
}

// ========================
// 🥊 FIGHTERS (NFT STYLE)
// ========================
function drawFighters() {

  // ======================
  // 🐵 BAYC MONKEY
  // ======================
  push();
  translate(player.x, player.y);

  // head
  fill(180, 130, 70);
  ellipse(0, -60, 70, 75);

  // ears
  ellipse(-35, -65, 20, 25);
  ellipse(35, -65, 20, 25);

  // face
  fill(220, 180, 120);
  ellipse(0, -55, 50, 45);

  // eyes
  fill(0);
  ellipse(-12, -60, 8, 6);
  ellipse(12, -60, 8, 6);

  // mouth
  noFill();
  stroke(0);
  arc(0, -45, 20, 10, 0, PI);

  noStroke();

  // body
  fill(255);
  rect(-25, -30, 50, 70, 10);

  // punch animation
  if (player.action === "punch") {
    fill(255, 220, 0);
    rect(25, -10, 25, 10, 5);
  }

  // block
  if (player.action === "block") {
    fill(100);
    rect(-40, -10, 10, 40);
  }

  pop();


  // ======================
  // 👾 CRYPTOPUNK ENEMY
  // ======================
  push();
  translate(enemy.x, enemy.y);

  // pixel head
  noStroke();
  fill(120, 0, 200);
  rect(-30, -90, 60, 60);

  // punk hair spikes
  fill(255, 0, 150);
  rect(-25, -100, 10, 10);
  rect(-10, -105, 10, 15);
  rect(5, -100, 10, 10);
  rect(20, -110, 10, 20);

  // eyes
  fill(0);
  rect(-15, -70, 10, 10);
  rect(10, -70, 10, 10);

  // glitch mouth
  fill(0, 255, 255);
  rect(-10, -50, 20, 5);

  // body
  fill(80);
  rect(-30, -30, 60, 70, 8);

  // enemy punch
  if (enemy.action === "punch") {
    fill(0, 255, 255);
    rect(-55, -10, 25, 10, 5);
  }

  pop();
}

// ========================
// UI
// ========================
function drawUI() {

  // HP bars
  fill(0, 255, 0);
  rect(20, 50, player.hp * 2, 10);

  fill(255, 0, 0);
  rect(width - 220, 50, enemy.hp * 2, 10);

  fill(255);
  textSize(14);
  textAlign(LEFT);
  text(player.name, 20, 80);

  textAlign(RIGHT);
  text(enemy.name, width - 20, 80);

  // buttons
  fill(255);
  rect(20, height - 120, 120, 50, 10);
  rect(width - 140, height - 120, 120, 50, 10);

  fill(0);
  textAlign(CENTER, CENTER);
  text("PUNCH", 80, height - 95);
  text("BLOCK", width - 80, height - 95);

  fill(255);
  text(gameMsg, width / 2, height - 150);
}

// ========================
// INPUT
// ========================
function mousePressed() {
  handleInput(mouseX, mouseY);
}

function touchStarted() {
  handleInput(mouseX, mouseY);
  return false;
}

function handleInput(x, y) {

  if (cooldown > 0) return;

  // punch
  if (x > 20 && x < 140 && y > height - 120 && y < height - 70) {

    player.action = "punch";
    enemy.hp -= 10;
    gameMsg = "You punched CryptoPunk!";

    enemyAttack();
    cooldown = 30;
  }

  // block
  if (x > width - 140 && x < width - 20 && y > height - 120 && y < height - 70) {

    player.action = "block";
    gameMsg = "Blocking!";
    cooldown = 20;
  }
}

// ========================
// SIMPLE AI
// ========================
function enemyAttack() {

  enemy.action = "punch";

  setTimeout(() => {

    if (player.action !== "block") {
      player.hp -= 8;
      gameMsg = "You got hit!";
    } else {
      gameMsg = "Blocked!";
    }

    enemy.action = "idle";
    player.action = "idle";

  }, 400);
}

// ========================
// GAME OVER
// ========================
function drawGameOver() {

  fill(0, 200);
  rect(0, 0, width, height);

  fill(255);
  textSize(32);
  textAlign(CENTER);

  let result =
    player.hp <= 0
      ? "CryptoPunk Wins!"
      : "BAYCand Wins!";

  text(result, width / 2, height / 2);
}

// ========================
function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
}
