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
javaScript

(() => {
  const mount = document.getElementById("game");
  const errorBox = document.getElementById("error");

  if (!window.THREE) {
    errorBox.hidden = false;
    errorBox.textContent =
      "Three.js could not load. Check your internet connection and reload the game.";
    document.getElementById("loading").textContent = "3D ENGINE UNAVAILABLE";
    return;
  }

  try {
    // WORLD AND CAMERA
    const scene = new THREE.Scene();
    scene.background = new THREE.Color(0xb7a6a0);
    scene.fog = new THREE.FogExp2(0xb7a6a0, 0.008);

    const camera = new THREE.PerspectiveCamera(
      60, innerWidth / innerHeight, 0.1, 400
    );

    const renderer = new THREE.WebGLRenderer({
      antialias: true,
      powerPreference: "high-performance"
    });

    renderer.setSize(innerWidth, innerHeight);
    renderer.setPixelRatio(Math.min(devicePixelRatio, 1.5));
    renderer.shadowMap.enabled = true;
    renderer.shadowMap.type = THREE.PCFSoftShadowMap;
    renderer.outputEncoding = THREE.sRGBEncoding;
    renderer.toneMapping = THREE.ACESFilmicToneMapping;
    renderer.toneMappingExposure = 1.15;

    mount.appendChild(renderer.domElement);

    // CINEMATIC LIGHTING
    scene.add(new THREE.HemisphereLight(0xdceaff, 0x55402d, 1.8));

    const sun = new THREE.DirectionalLight(0xffd4a0, 3);
    sun.position.set(-35, 55, 25);
    sun.castShadow = true;
    sun.shadow.mapSize.set(1024, 1024);
    sun.shadow.camera.left = -35;
    sun.shadow.camera.right = 35;
    sun.shadow.camera.top = 35;
    sun.shadow.camera.bottom = -35;
    sun.shadow.bias = -0.0005;
    scene.add(sun);

    const fillLight = new THREE.DirectionalLight(0x91b7e6, 0.65);
    fillLight.position.set(25, 18, -20);
    scene.add(fillLight);

    // MATERIALS
    const material = (color, roughness = 0.85, metalness = 0) =>
      new THREE.MeshStandardMaterial({
        color, roughness, metalness
      });

    const stone = material(0x9a8975);
    const lightStone = material(0xc2ad8c);
    const shadowStone = material(0x65574b);
    const marble = material(0xd4c7af, 0.5);
    const red = material(0x641f25);
    const green = material(0x263f34);
    const gold = material(0xd2ad63, 0.34, 0.55);
    const leather = material(0x38271f);
    const skin = material(0xb77c55);
    const grass = material(0x60664a);
    const road = material(0x8d8170);

    // HELPER FUNCTIONS
    function addBox(parent, w, h, d, mat, x, y, z) {
      const mesh = new THREE.Mesh(
        new THREE.BoxGeometry(w, h, d), mat
      );
      mesh.position.set(x, y, z);
      mesh.castShadow = true;
      mesh.receiveShadow = true;
      parent.add(mesh);
      return mesh;
    }

    function addSphere(parent, r, mat, x, y, z, sx = 1, sy = 1, sz = 1) {
      const mesh = new THREE.Mesh(
        new THREE.SphereGeometry(r, 20, 14), mat
      );
      mesh.position.set(x, y, z);
      mesh.scale.set(sx, sy, sz);
      mesh.castShadow = true;
      mesh.receiveShadow = true;
      parent.add(mesh);
      return mesh;
    }

    function addDome(parent, x, y, z, r, mat) {
      const dome = new THREE.Mesh(
        new THREE.SphereGeometry(
          r, 32, 20, 0, Math.PI * 2, 0, Math.PI / 2
        ),
        mat
      );
      dome.position.set(x, y, z);
      dome.castShadow = true;
      parent.add(dome);

      const tip = new THREE.Mesh(
        new THREE.ConeGeometry(r * 0.035, r * 0.4, 8), gold
      );
      tip.position.set(x, y + r + r * 0.18, z);
      parent.add(tip);
    }

    function addMinaret(parent, x, z, height = 12) {
      const g = new THREE.Group();
      parent.add(g);

      const base = new THREE.Mesh(
        new THREE.CylinderGeometry(0.7, 1.05, height, 12), lightStone
      );
      base.position.set(x, height / 2, z);
      base.castShadow = true;
      g.add(base);

      const balcony = new THREE.Mesh(
        new THREE.CylinderGeometry(1.05, 0.95, 0.3, 12), marble
      );
      balcony.position.set(x, height * 0.72, z);
      g.add(balcony);

      const top = new THREE.Mesh(
        new THREE.ConeGeometry(0.85, 2.3, 12), red
      );
      top.position.set(x, height + 1.1, z);
      g.add(top);

      const crescent = new THREE.Mesh(
        new THREE.TorusGeometry(0.25, 0.055, 8, 20, Math.PI * 1.6), gold
      );
      crescent.position.set(x, height + 2.5, z);
      g.add(crescent);
    }

    // TERRAIN
    const ground = new THREE.Mesh(
      new THREE.PlaneGeometry(300, 300),
      grass
    );
    ground.rotation.x = -Math.PI / 2;
    ground.position.y = -0.1;
    ground.receiveShadow = true;
    scene.add(ground);

    // STONE COURTYARD AND STREET
    addBox(scene, 10, 0.12, 100, road, 0, -0.015, 0);
    addBox(scene, 70, 0.12, 7, road, 0, -0.02, -2);

    // STONE TILES
    const tileMat = material(0xa99b87);
    for (let z = -30; z < 28; z += 3) {
      for (let x = -4; x <= 4; x += 2) {
        const tile = addBox(
          scene, 1.9, 0.035, 2.85, tileMat,
          x, 0.025, z
        );
        tile.position.x += (Math.abs(z) % 2) * 0.25;
      }
    }

    // IMPERIAL GATE AND WALLS
    addBox(scene, 46, 8, 2.5, stone, 0, 4, -19);
    addBox(scene, 2.5, 8, 48, stone, -23, 4, 4);
    addBox(scene, 2.5, 8, 48, stone, 23, 4, 4);

    addBox(scene, 10, 8, 3, lightStone, 0, 4, -19);
    addBox(scene, 5, 5.5, 3.4, shadowStone, 0, 2.75, -17.3);

    for (let x = -20; x <= 20; x += 5) {
      addBox(scene, 3.6, 0.8, 3.2, lightStone, x, 8.2, -19);
    }

    // MAIN PALACE
    const palace = new THREE.Group();
    scene.add(palace);

    addBox(palace, 16, 7, 12, lightStone, -5, 3.5, -34);
    addBox(palace, 17, 0.5, 13, marble, -5, 7.1, -34);

    addDome(palace, -5, 7.3, -34, 4.2, green);
    addDome(palace, -10, 7.1, -34, 2.1, red);
    addDome(palace, 0, 7.1, -34, 2.1, red);

    addMinaret(palace, -12, -35, 11);
    addMinaret(palace, 2, -35, 11);

    // CITY BUILDINGS
    function createBuilding(x, z, w, d, h) {
      const g = new THREE.Group();
      scene.add(g);

      addBox(g, w, h, d, stone, x, h / 2, z);
      addBox(g, w + 0.3, 0.3, d + 0.3, marble, x, h, z);

      // Windows and window frames
      for (let i = 0; i < Math.floor(w / 2.3); i++) {
        const wx = x - w / 2 + 1.2 + i * 2.2;
        addBox(g, 0.8, 1.4, 0.12, shadowStone, wx, h * 0.62, z - d / 2 - 0.07);
        addBox(g, 0.95, 0.12, 0.18, gold, wx, h * 0.62 + 0.75, z - d / 2 - 0.1);
      }

      addDome(g, x, h + 0.15, z, Math.min(w, d) * 0.28, green);

      return g;
    }

    for (let i = 0; i < 30; i++) {
      const side = i % 2 === 0 ? -1 : 1;
      const x = side * (14 + Math.random() * 25);
      const z = -8 - Math.random() * 65;

      createBuilding(
        x, z,
        5 + Math.random() * 5,
        5 + Math.random() * 5,
        4 + Math.random() * 7
      );
    }

    // DISTANT SKYLINE
    for (let i = 0; i < 14; i++) {
      const x = (Math.random() - 0.5) * 110;
      const z = -70 - Math.random() * 30;

      const building = new THREE.Mesh(
        new THREE.BoxGeometry(4 + Math.random() * 5, 7 + Math.random() * 13, 5),
        shadowStone
      );
      building.position.set(x, building.geometry.parameters.height / 2, z);
      scene.add(building);

      addDome(scene, x, building.position.y * 2, z, 1.8, red);
    }

    // WARRIOR MODEL
    function createWarrior(isPlayer) {
      const g = new THREE.Group();
      const cloth = isPlayer ? red : green;

      const body = new THREE.Mesh(
        new THREE.CylinderGeometry(0.38, 0.55, 1.25, 12), cloth
      );
      body.position.y = 1.35;
      body.castShadow = true;
      g.add(body);

      // Shoulder armour
      for (const side of [-1, 1]) {
        addSphere(g, 0.25, gold, side * 0.42, 1.78, 0, 1, 0.7, 1);

        const leg = new THREE.Mesh(
          new THREE.CylinderGeometry(0.14, 0.18, 0.72, 10), leather
        );
        leg.position.set(side * 0.22, 0.45, 0);
        leg.castShadow = true;
        g.add(leg);

        const arm = new THREE.Mesh(
          new THREE.CylinderGeometry(0.12, 0.17, 0.72, 10), cloth
        );
        arm.position.set(side * 0.48, 1.4, 0);
        arm.rotation.z = -side * 0.2;
        arm.castShadow = true;
        g.add(arm);

        addSphere(g, 0.14, gold, side * 0.49, 1.05, 0);
      }

      addSphere(g, 0.3, skin, 0, 2.25, 0);
      addSphere(g, 0.36, isPlayer ? red : green, 0, 2.49, 0, 1, 0.55, 1);
      addBox(g, 0.72, 0.1, 0.72, gold, 0, 2.58, 0);

      // Belt
      addBox(g, 0.83, 0.13, 0.58, gold, 0, 0.94, 0);

      // Sword and handle
      const sword = new THREE.Group();
      sword.position.set(0.57, 1.4, -0.03);

      const blade = new THREE.Mesh(
        new THREE.BoxGeometry(0.09, 1.0, 0.13), marble
      );
      blade.position.y = 0.53;
      sword.add(blade);

      const handle = new THREE.Mesh(
        new THREE.BoxGeometry(0.16, 0.27, 0.19), gold
      );
      sword.add(handle);

      g.add(sword);
      g.userData.sword = sword;

      scene.add(g);
      return g;
    }

    const player = createWarrior(true);
    player.position.set(0, 0, 8);

    const enemies = [];

    for (let i = 0; i < 6; i++) {
      const enemy = createWarrior(false);
      enemy.position.set((i % 3 - 1) * 5, 0, -3 - Math.floor(i / 3) * 5);
      enemy.userData.hp = 2;
      enemy.userData.cooldown = 0;
      enemies.push(enemy);
    }

    // MOBILE AND KEYBOARD INPUT
    const keys = {};
    let health = 100;
    let score = 0;
    let velocityY = 0;
    let onGround = true;
    let attackTimer = 0;

    const healthLabel = document.getElementById("health");
    const healthFill = document.getElementById("healthFill");
    const scoreLabel = document.getElementById("score");
    const message = document.getElementById("message");

    document.querySelectorAll("[data-key]").forEach(button => {
      const key = button.dataset.key;

      button.addEventListener("pointerdown", e => {
        e.preventDefault();
        keys[key] = true;
        button.setPointerCapture?.(e.pointerId);
      });

      ["pointerup", "pointercancel", "lostpointercapture"].forEach(type => {
        button.addEventListener(type, () => keys[key] = false);
      });
    });

    function jump() {
      if (onGround && health > 0) {
        velocityY = 8.5;
        onGround = false;
      }
    }

    function attack() {
      if (attackTimer > 0 || health <= 0) return;

      attackTimer = 0.48;
      player.userData.sword.rotation.z = -1.4;

      let target = null;
      let bestDistance = 3.8;

      enemies.forEach(enemy => {
        if (enemy.userData.hp <= 0) return;

        const distance = player.position.distanceTo(enemy.position);

        if (distance < bestDistance) {
          target = enemy;
          bestDistance = distance;
        }
      });

      if (!target) {
        message.textContent = "Move closer to an enemy to strike!";
        return;
      }

      target.userData.hp--;

      if (target.userData.hp <= 0) {
        scene.remove(target);
        score++;
        scoreLabel.textContent = score;
        message.textContent = score === 6
          ? "VICTORY! The imperial courtyard is yours."
          : "Enemy defeated. Continue your mission!";
      } else {
        message.textContent = "Strike landed! One more hit!";
      }
    }

    document.querySelectorAll("[data-action]").forEach(button => {
      button.addEventListener("pointerdown", e => {
        e.preventDefault();

        if (button.dataset.action === "jump") jump();
        if (button.dataset.action === "attack") attack();
      });
    });

    addEventListener("keydown", e => {
      const k = e.key.toLowerCase();
      keys[k] = true;

      if (["arrowup", "arrowdown", "arrowleft", "arrowright", " "].includes(k)) {
        e.preventDefault();
      }

      if (k === " " && !e.repeat) jump();
      if (k === "j" && !e.repeat) attack();
    });

    addEventListener("keyup", e => {
      keys[e.key.toLowerCase()] = false;
    });

    // GAME LOOP
    let previous = performance.now();

    function animate(now) {
      requestAnimationFrame(animate);

      const dt = Math.min((now - previous) / 1000, 0.04);
      previous = now;

      if (health > 0) {
        let dx = 0;
        let dz = 0;

        if (keys.left || keys.a || keys.arrowleft) dx -= 1;
        if (keys.right || keys.d || keys.arrowright) dx += 1;
        if (keys.up || keys.w || keys.arrowup) dz -= 1;
        if (keys.down || keys.s || keys.arrowdown) dz += 1;

        if (dx || dz) {
          const length = Math.hypot(dx, dz);
          dx /= length;
          dz /= length;

          player.position.x += dx * 7 * dt;
          player.position.z += dz * 7 * dt;
          player.rotation.y = Math.atan2(dx, dz) + Math.PI;
        }

        if (keys.jump) jump();
        if (keys.attack) attack();
      }

      velocityY -= 20 * dt;
      player.position.y += velocityY * dt;

      if (player.position.y <= 0) {
        player.position.y = 0;
        velocityY = 0;
        onGround = true;
      }

      attackTimer = Math.max(0, attackTimer - dt);

      if (attackTimer === 0) {
        player.userData.sword.rotation.z = 0;
      }

      enemies.forEach(enemy => {
        if (enemy.userData.hp <= 0 || health <= 0) return;

        const distance = enemy.position.distanceTo(player.position);

        if (distance < 12 && distance > 1.8) {
          const direction = player.position.clone()
            .sub(enemy.position).normalize();

          enemy.position.addScaledVector(direction, dt * 1.3);
          enemy.rotation.y = Math.atan2(direction.x, direction.z) + Math.PI;
        } else if (distance <= 1.8) {
          enemy.userData.cooldown -= dt;

          if (enemy.userData.cooldown <= 0) {
            health = Math.max(0, health - 5);
            healthLabel.textContent = health;
            healthFill.style.width = health + "%";
            enemy.userData.cooldown = 1;

            if (health === 0) {
              message.textContent = "You have fallen. Reload to restart.";
            }
          }
        }
      });

      // FOLLOW CAMERA
      const targetCamera = new THREE.Vector3(
        player.position.x,
        player.position.y + 5.5,
        player.position.z + 9
      );

      camera.position.lerp(targetCamera, 1 - Math.pow(0.001, dt));
      camera.lookAt(
        player.position.x,
        player.position.y + 1.2,
        player.position.z - 1
      );

      renderer.render(scene, camera);
    }

    addEventListener("resize", () => {
      camera.aspect = innerWidth / innerHeight;
      camera.updateProjectionMatrix();
      renderer.setSize(innerWidth, innerHeight);
      renderer.setPixelRatio(Math.min(devicePixelRatio, 1.5));
    });

    requestAnimationFrame(animate);

    const loading = document.getElementById("loading");
    loading.classList.add("hide");
    setTimeout(() => loading.remove(), 800);

  } catch (error) {
    errorBox.hidden = false;
    errorBox.textContent = "Game error: " + error.message;
    document.getElementById("loading").textContent = "GAME ERROR";
  }
})();
