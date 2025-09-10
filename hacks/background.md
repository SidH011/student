---
layout: base
title: Background with Object
description: Use JavaScript to have an in motion background.
sprite: images/platformer/sprites/flying-ufo.png
background: images/platformer/backgrounds/alien_planet2.jpg
permalink: /background
---

<!-- Canvas where the game world will be rendered -->
<canvas id="world"></canvas>

<script>
  // Reference the canvas element
  const canvas = document.getElementById("world");
  const ctx = canvas.getContext('2d');

  // Create image objects for the background and sprite
  const backgroundImg = new Image();
  const spriteImg = new Image();

  // Load paths defined in the page’s front matter
  backgroundImg.src = '{{page.background}}';
  spriteImg.src = '{{page.sprite}}';

  // Track when both images are loaded
  let imagesLoaded = 0;

  // Once background is loaded, increment counter and try to start
  backgroundImg.onload = function() {
    imagesLoaded++;
    startGameWorld();
  };

  // Once sprite is loaded, increment counter and try to start
  spriteImg.onload = function() {
    imagesLoaded++;
    startGameWorld();
  };

  // Initialize the game world once both images are ready
  function startGameWorld() {
    if (imagesLoaded < 2) return; // Don’t start until both are loaded

    // Base class for all objects in the game
    class GameObject {
      constructor(image, width, height, x = 0, y = 0, speedRatio = 0) {
        this.image = image;     // The visual asset
        this.width = width;     // Drawn width
        this.height = height;   // Drawn height
        this.x = x;             // X position
        this.y = y;             // Y position
        this.speedRatio = speedRatio; // Speed factor relative to world speed
        this.speed = GameWorld.gameSpeed * this.speedRatio; // Actual speed
      }
      update() {} // Default update does nothing
      draw(ctx) {
        ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
      }
    }

    // Background that scrolls horizontally
    class Background extends GameObject {
      constructor(image, gameWorld) {
        // Stretch the background to cover the entire canvas
        super(image, gameWorld.width, gameWorld.height, 0, 0, 0.1);
      }
      update() {
        // Move background to the left and wrap around for seamless loop
        this.x = (this.x - this.speed) % this.width;
      }
      draw(ctx) {
        // Draw two copies: one starting at x, another right after it
        ctx.drawImage(this.image, this.x, this.y, this.width, this.height);
        ctx.drawImage(this.image, this.x + this.width, this.y, this.width, this.height);
      }
    }

    // Player sprite that floats up and down
    class Player extends GameObject {
      constructor(image, gameWorld) {
        // Scale sprite to half its original size
        const width = image.naturalWidth / 2;
        const height = image.naturalHeight / 2;
        // Place it in the center of the canvas
        const x = (gameWorld.width - width) / 2;
        const y = (gameWorld.height - height) / 2;
        super(image, width, height, x, y);
        this.baseY = y; // Store starting vertical position
        this.frame = 0; // Animation frame counter
      }
      update() {
        // Apply sine wave to make the sprite float up and down
        this.y = this.baseY + Math.sin(this.frame * 0.05) * 20;
        this.frame++;
      }
    }

    // The main game controller
    class GameWorld {
      static gameSpeed = 5; // Base speed for objects

      constructor(backgroundImg, spriteImg) {
        // Setup canvas dimensions to fit window
        this.canvas = document.getElementById("world");
        this.ctx = this.canvas.getContext('2d');
        this.width = window.innerWidth;
        this.height = window.innerHeight;
        this.canvas.width = this.width;
        this.canvas.height = this.height;

        // Style canvas to cover the window
        this.canvas.style.width = `${this.width}px`;
        this.canvas.style.height = `${this.height}px`;
        this.canvas.style.position = 'absolute';
        this.canvas.style.left = `0px`;
        this.canvas.style.top = `${(window.innerHeight - this.height) / 2}px`;

        // Initialize objects in the game: background + player
        this.gameObjects = [
         new Background(backgroundImg, this),
         new Player(spriteImg, this)
        ];
      }

      // Main loop: update and draw all objects
      gameLoop() {
        this.ctx.clearRect(0, 0, this.width, this.height); // Clear screen
        for (const obj of this.gameObjects) {
          obj.update();       // Update state (movement/animation)
          obj.draw(this.ctx); // Redraw
        }
        // Call the next frame
        requestAnimationFrame(this.gameLoop.bind(this));
      }

      // Start the loop
      start() {
        this.gameLoop();
      }
    }

    // Create and start the world
    const world = new GameWorld(backgroundImg, spriteImg);
    world.start();
  }
</script>