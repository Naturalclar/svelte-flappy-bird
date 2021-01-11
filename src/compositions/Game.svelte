<script>
  import { onMount } from "svelte";
  const interval = 24;

  const canvasSize = 400;

  // Bird Info
  const bird = new Image();
  const birdSize = 24;
  bird.src = "bird.png";

  // Bird's speed in y direction
  let birdDy = 0;

  // Bord's speed in x direction
  const birdDx = 0;

  // Bird's X coord
  let birdX = 0;
  // Bird's Y Coord
  let birdY = 200;

  // Pipe Info
  const pipeWidth = 24;
  let pipeX = 200;
  const pipeGap = 150;
  let topPipeBottomY = 24;

  // Score info
  let score = 0;
  let bestScore = 0;

  onMount(() => {
    const game = document.getElementById("game");
    const context = game.getContext("2d");

    setInterval(() => {
      context.fillStyle = "skyblue";
      context.fillRect(0, 0, canvasSize, canvasSize);
      updateBird();
      context.drawImage(bird, birdX, birdY, birdSize * (524 / 374), birdSize);
      context.fillStyle = "green";
      context.fillRect(pipeX, 0, pipeWidth, topPipeBottomY);
      context.fillRect(pipeX, topPipeBottomY + pipeGap, pipeWidth, canvasSize);
      updatePipe();
      context.fillStyle = "black";
      context.fillText(score++, 9, 25);
      bestScore = bestScore < score ? score : bestScore;
      context.fillText(`Best: ${bestScore}`, 9, 50);

      // Game over
      gameOver() && reset();
    }, interval);
  });

  function gameOver() {
    const touchingPipe =
      (birdY < topPipeBottomY || birdY > topPipeBottomY + pipeGap) &&
      pipeX < birdSize * (524 / 374);

    const outOfScreen = birdY > canvasSize;
    return touchingPipe || outOfScreen;
  }

  function updateBird() {
    birdY = birdY - birdDy;
    birdDy = birdDy - 0.5;
  }

  function updatePipe() {
    pipeX = pipeX - 8;
    pipeX < -pipeWidth && generatePipe();
  }

  function generatePipe() {
    pipeX = canvasSize;
    topPipeBottomY = pipeGap * Math.random();
  }

  function reset() {
    birdDy = 0;
    birdY = 200;
    score = 0;
    pipeX = canvasSize;
  }

  function handleClick() {
    birdDy = 9;
  }
</script>

<style>
  canvas {
    border: 2px solid skyblue;
  }
</style>

<canvas id="game" width="400" height="400" on:click={handleClick} />
