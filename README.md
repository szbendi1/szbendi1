 // Phaser játék inicializálása
var config = {
  type: Phaser.AUTO,
  width: 800,
  height: 600,
  physics: {
    default: 'arcade',
    arcade: {
      gravity: { y: 500 },
      debug: false
    }
  },
  scene: {
    preload: preload,
    create: create,
    update: update
  }
};

var game = new Phaser.Game(config);

// Előtöltés - betöltjük a szükséges erőforrásokat
function preload() {
  this.load.image('background', 'assets/background.png');
  this.load.image('platform', 'assets/platform.png');
  this.load.image('player', 'assets/player.png');
}

// Létrehozás - inicializáljuk a játékbeli elemeket
function create() {
  // Háttér kép hozzáadása
  this.add.image(400, 300, 'background');

  // Platform hozzáadása
  var platforms = this.physics.add.staticGroup();
  platforms.create(400, 568, 'platform').setScale(2).refreshBody();
  platforms.create(600, 400, 'platform');
  platforms.create(50, 250, 'platform');
  platforms.create(750, 220, 'platform');

  // Játékos hozzáadása
  player = this.physics.add.sprite(100, 450, 'player');
  player.setBounce(0.2);
  player.setCollideWorldBounds(true);

  // Játékos és platformok közötti ütközés kezelése
  this.physics.add.collider(player, platforms);
}

// Frissítés - játékállapot frissítése
function update() {
  // Játékos vezérlése
  cursors = this.input.keyboard.createCursorKeys();
  if (cursors.left.isDown) {
    player.setVelocityX(-160);
  } else if (cursors.right.isDown) {
    player.setVelocityX(160);
  } else {
    player.setVelocityX(0);
  }
  if (cursors.up.isDown && player.body.touching.down) {
    player.setVelocityY(-400);
  }
}
