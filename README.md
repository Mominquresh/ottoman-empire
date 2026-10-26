html

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, user-scalable=no">
  <meta name="theme-color" content="#111820">
  <title>OSMANLI SULTANAT 3D</title>
  <link rel="stylesheet" href="style.css">
  <script src="https://cdnjs.cloudflare.com/ajax/libs/three.js/r128/three.min.js"></script>
</head>
<body>
  <div id="game"></div>

  <header id="hud">
    <div class="brand">☾ OSMANLI <span>SULTANAT</span></div>
    <div class="subtitle">RISE OF AN EMPIRE</div>

    <div class="status">
      <div class="health-line">
        <span>HEALTH</span>
        <div class="health-track"><div id="healthFill"></div></div>
        <b id="health">100</b>
      </div>
      <div class="score-line">⚔ <span id="score">0</span> / 6 GUARDS</div>
    </div>
    <div id="message">Explore the imperial courtyard</div>
  </header>

  <div id="crosshair">✦</div>
  <div id="loading">PREPARING THE EMPIRE...</div>

  <div id="controls">
    <div class="movement">
      <button class="control up" data-key="up">▲</button>
      <button class="control left" data-key="left">◀</button>
      <button class="control down" data-key="down">▼</button>
      <button class="control right" data-key="right">▶</button>
    </div>
    <div class="combat">
      <button class="control jump" data-action="jump">JUMP</button>
      <button class="control attack" data-action="attack">⚔ ATTACK</button>
    </div>
  </div>

  <div id="desktopHint">WASD / ARROWS · SPACE TO JUMP · J TO ATTACK</div>
  <div id="error" hidden></div>

  <script src="script.js"></script>
</body>
</html>
css

* {
  box-sizing: border-box;
  -webkit-tap-highlight-color: transparent;
}

:root {
  --gold: #d6b16b;
  --dark: rgba(13, 19, 25, 0.82);
  --border: rgba(214, 177, 107, 0.65);
}

html, body {
  margin: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  background: #111820;
  color: #f7f0e2;
  font-family: Georgia, "Times New Roman", serif;
  touch-action: none;
}

#game {
  position: fixed;
  inset: 0;
  overflow: hidden;
}

#game canvas {
  display: block;
  width: 100%;
  height: 100%;
}

#hud {
  position: fixed;
  top: max(16px, env(safe-area-inset-top));
  left: 17px;
  z-index: 5;
  pointer-events: none;
  text-shadow: 0 2px 10px #000;
}

.brand {
  font-size: clamp(20px, 4.7vw, 32px);
  font-weight: bold;
  letter-spacing: 2px;
  color: var(--gold);
}

.brand span {
  color: #fff4dc;
}

.subtitle {
  margin-top: 4px;
  font-family: Arial, sans-serif;
  font-size: 9px;
  letter-spacing: 4px;
  color: #e7d5b1;
}

.status {
  margin-top: 18px;
  padding: 12px;
  width: 215px;
  background: var(--dark);
  border: 1px solid var(--border);
  border-radius: 3px;
  font-family: Arial, sans-serif;
  font-size: 10px;
  letter-spacing: 1px;
  backdrop-filter: blur(8px);
}

.health-line {
  display: flex;
  align-items: center;
  gap: 9px;
}

.health-track {
  flex: 1;
  height: 7px;
  background: #422323;
  border: 1px solid #84664b;
}

#healthFill {
  width: 100%;
  height: 100%;
  background: linear-gradient(90deg, #9b2828, #e2a65b);
  transition: width .2s;
}

#health {
  color: #f2d49a;
}

.score-line {
  margin-top: 11px;
  color: #e4c98e;
}

#score {
  font-weight: bold;
}

#message {
  margin-top: 10px;
  font-family: Arial, sans-serif;
  font-size: 11px;
  color: #f2d49a;
  max-width: 270px;
  line-height: 1.5;
}

#crosshair {
  position: fixed;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  color: #f2d49a;
  opacity: .65;
  pointer-events: none;
}

#loading {
  position: fixed;
  inset: 0;
  display: grid;
  place-items: center;
  background: #111820;
  z-index: 20;
  font-family: Arial, sans-serif;
  font-size: 12px;
  letter-spacing: 4px;
  color: var(--gold);
  transition: opacity .7s;
}

#loading.hide {
  opacity: 0;
  pointer-events: none;
}

#controls {
  position: fixed;
  left: 0;
  right: 0;
  bottom: max(18px, env(safe-area-inset-bottom));
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  padding: 0 18px;
  z-index: 10;
  pointer-events: none;
}

.movement, .combat {
  pointer-events: auto;
  display: grid;
  gap: 7px;
}

.movement {
  grid-template-columns: 48px 48px 48px;
  grid-template-rows: 46px 46px;
}

.up {
  grid-column: 2;
  grid-row: 1;
}

.left {
  grid-column: 1;
  grid-row: 2;
}

.down {
  grid-column: 2;
  grid-row: 2;
}

.right {
  grid-column: 3;
  grid-row: 2;
}

.combat {
  grid-template-columns: 62px 78px;
  align-items: end;
}

.control {
  color: #fff3d8;
  background: rgba(17, 25, 31, .76);
  border: 1px solid var(--border);
  border-radius: 9px;
  font-family: Arial, sans-serif;
  font-weight: bold;
  font-size: 16px;
  backdrop-filter: blur(8px);
  user-select: none;
  -webkit-user-select: none;
  touch-action: none;
}

.control:active {
  background: rgba(151, 105, 43, .9);
}

.combat .control {
  height: 54px;
  font-size: 10px;
}

.combat .attack {
  color: #f2d49a;
  border-color: #d5a553;
}

#desktopHint {
  position: fixed;
  bottom: 16px;
  left: 50%;
  transform: translateX(-50%);
  font: 9px Arial, sans-serif;
  letter-spacing: 2px;
  color: #fff9;
  z-index: 3;
  white-space: nowrap;
  pointer-events: none;
}

#error {
  position: fixed;
  inset: auto 10px 150px;
  z-index: 30;
  background: #481d1d;
  color: white;
  padding: 12px;
  font: 12px Arial, sans-serif;
  overflow-wrap: anywhere;
}

@media (min-width: 800px) {
  #controls {
    padding: 0 36px 30px;
  }

  .movement {
    transform: scale(1.12);
    transform-origin: bottom left;
  }

  .combat {
    transform: scale(1.12);
    transform-origin: bottom right;
  }
}

@media (max-height: 430px) {
  #hud {
    top: 8px;
    left: 10px;
  }

  .status {
    margin-top: 8px;
    padding: 7px;
  }

  #message {
    margin-top: 5px;
  }

  #controls {
    bottom: 8px;
  }

  .movement {
    grid-template-rows: 36px 36px;
  }

  .control {
    font-size: 14px;
  }

  .combat .control {
    height: 43px;
  }
}
