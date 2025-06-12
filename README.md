# Agrinho2025HTMLLCSS
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(220);
}let player;
let obstacles = [];
let gameOver = false;

function setup() {
  createCanvas(400, 600);
  player = new Player();
}

function draw() {
  background(30);

  if (!gameOver) {
    player.update();
    player.show();

    if (frameCount % 60 === 0) {
      obstacles.push(new Obstacle());
    }

    for (let obs of obstacles) {
      obs.update();
      obs.show();

      if (obs.hits(player)) {
        gameOver = true;
      }
    }

    obstacles = obstacles.filter(o => !o.offscreen());
  } else {
    textAlign(CENTER, CENTER);
    fill(255, 0, 0);
    textSize(32);
    text('Game Over', width / 2, height / 2);
  }
}

function keyPressed() {
  if (keyCode === LEFT_ARROW) {
    player.move(-1);
  } else if (keyCode === RIGHT_ARROW) {
    player.move(1);
  }
}

// Player class
class Player {
  constructor() {
    this.size = 40;
    this.x = width / 2 - this.size / 2;
    this.y = height - this.size * 2;
    this.speed = 7;
    this.dir = 0;
  }

  move(direction) {
    this.dir = direction;
  }

  update() {
    this.x += this.dir * this.speed;
    this.x = constrain(this.x, 0, width - this.size);
    this.dir = 0;
  }

  show() {
    fill(0, 255, 0);
    rect(this.x, this.y, this.size, this.size);
  }
}

// Obstacle class
class Obstacle {
  constructor() {
    this.w = random(30, 80);
    this.h = 20;
    this.x = random(0, width - this.w);
    this.y = 0;
    this.speed = 5;
  }

  update() {
    this.y += this.speed;
  }

  show() {
    fill(255);
    rect(this.x, this.y, this.w, this.h);
  }

  hits(player) {
    return (
      player.x < this.x + this.w &&
      player.x + player.size > this.x &&
      player.y < this.y + this.h &&
      player.y + player.size > this.y
    );
  }

  offscreen() {
    return this.y > height;
  }
}
