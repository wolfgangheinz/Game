Ok, how about a micro machines inspired simple racing game. On the tracks, there are many items. The good ones speed you up or bring points, others slow you down or block you. 

Would that be doable as a browser game?

Short answer: Yes, absolutely. A Micro Machines–style “PI Racing” game with good/bad items is very doable as a browser game.

Here’s how I’d think about it:

⸻

1. Technical feasibility

You can build this entirely in the browser with pretty standard tech:
	•	Rendering:
	•	Plain HTML5 Canvas + JavaScript, or
	•	A light 2D engine like Phaser.js (top-down racers are a classic Phaser use case).
	•	Controls:
	•	Keyboard (left/right/accelerate/brake) or WASD.
	•	Optional gamepad support if you feel fancy later.
	•	Performance:
	•	Top-down racing with a single car and items is lightweight.
	•	As long as you keep sprites + effects moderate, even weaker laptops will handle it.

So yes, from a tech POV this is one of the simpler genres to get running in a browser.

⸻

2. Simple game loop (MVP scope)

Keep the first version brutally simple:
	1.	Track:
	•	One static top-down track image or tilemap.
	•	Car follows a path; off-track slows you down.
	2.	Player car:
	•	Position, direction, speed.
	•	Basic physics: acceleration, max speed, friction, turning.
	3.	Items on the track (PI metaphors):
	•	Good:
	•	Features / Stories / Enablers → speed boost or score.
	•	PI Objectives → big score or temporary “flow mode”.
	•	Bad:
	•	Defects → slow you down.
	•	Incidents / Unplanned Work → strong brake or spin.
	•	Emails / Meetings → short “stun” or steering inverted for a second.
	4.	Win/Lose / Scoring:
	•	Time to finish the lap(s).
	•	Score = features collected – penalties from bad stuff.
	•	Optional: bronze/silver/gold rating = how well you “planned” your route.

That’s more than enough for a playable prototype.

⸻

3. PI / SAFe angle (how you explain it)

You can make the metaphor pretty explicit without changing the simple arcade feel:
	•	The track = PI timeline.
	•	Shortcuts / lines on the track = dependencies / alignment (risky fast lines vs safe slower ones).
	•	Boost pads / good items = well-defined, high-value work that fits capacity.
	•	Slowdowns / hazards = unplanned work, production issues, excessive busywork.
	•	At the end of a race you show a “PI Summary” screen:
	•	Features delivered
	•	Unplanned work hit
	•	“Business value” total

⸻

4. Implementation path

If you want to actually build this, a realistic path:
	1.	Start with a Phaser.js top-down car example (or plain Canvas if you prefer bare JS).
	2.	Replace generic pickups with PI-themed sprites.
	3.	Add a simple HUD: score, lap time, “PI value”.
	4.	Add 1–2 tracks with slightly different item patterns:
	•	Track A: many emails/incidents, few features.
	•	Track B: mostly features, but with risky placements.

⸻

If you tell me your tech comfort level (plain JS vs framework), I can sketch a minimal Starter Code (HTML + JS) for the car + items so you have something you can actually run in the browser.

Is it possible to create a game without sprites?

Yes, absolutely. “No sprites” just means: no image files. You can draw everything as rectangles, circles and lines on <canvas>.

Think:
	•	Car = small rectangle
	•	Track = lines / thick path / background color
	•	Items = colored circles or squares

Here’s a super-minimal top-down “racer” with good/bad items, no sprites, all in one file:

<!DOCTYPE html>
<html>
<head>
  <meta charset="UTF-8">
  <title>PI Racing – No Sprites</title>
  <style>
    body { background:#222; color:#eee; font-family:sans-serif; text-align:center; }
    canvas { background:#333; margin-top:20px; }
  </style>
</head>
<body>
  <h1>PI Racing (Prototype)</h1>
  <p>Arrow keys to drive – green = good, red = bad</p>
  <canvas id="game" width="800" height="600"></canvas>

  <script>
    const canvas = document.getElementById('game');
    const ctx = canvas.getContext('2d');

    // Input
    const keys = {};
    window.addEventListener('keydown', e => keys[e.code] = true);
    window.addEventListener('keyup',   e => keys[e.code] = false);

    // Car (just a rectangle)
    const car = {
      x: 400,
      y: 500,
      angle: -Math.PI / 2,
      speed: 0,
      maxSpeed: 6,
      accel: 0.2,
      friction: 0.05,
      turnSpeed: 0.06,
      width: 20,
      height: 40
    };

    // Items: good (green) and bad (red)
    const items = [];
    const NUM_GOOD = 12;
    const NUM_BAD = 8;

    function spawnItems() {
      items.length = 0;
      for (let i = 0; i < NUM_GOOD; i++) {
        items.push({
          x: Math.random() * 760 + 20,
          y: Math.random() * 400 + 50,
          r: 10,
          type: 'good'
        });
      }
      for (let i = 0; i < NUM_BAD; i++) {
        items.push({
          x: Math.random() * 760 + 20,
          y: Math.random() * 400 + 50,
          r: 12,
          type: 'bad'
        });
      }
    }

    spawnItems();

    let score = 0;

    function update() {
      // Acceleration
      if (keys['ArrowUp'])    car.speed += car.accel;
      if (keys['ArrowDown'])  car.speed -= car.accel;

      // Clamp speed
      if (car.speed > car.maxSpeed) car.speed = car.maxSpeed;
      if (car.speed < -car.maxSpeed / 2) car.speed = -car.maxSpeed / 2;

      // Friction
      if (!keys['ArrowUp'] && !keys['ArrowDown']) {
        if (car.speed > 0) car.speed = Math.max(0, car.speed - car.friction);
        if (car.speed < 0) car.speed = Math.min(0, car.speed + car.friction);
      }

      // Turning
      if (car.speed !== 0) {
        if (keys['ArrowLeft'])  car.angle -= car.turnSpeed;
        if (keys['ArrowRight']) car.angle += car.turnSpeed;
      }

      // Move
      car.x += Math.cos(car.angle) * car.speed;
      car.y += Math.sin(car.angle) * car.speed;

      // Keep car in bounds
      if (car.x < 20) car.x = 20;
      if (car.x > canvas.width - 20) car.x = canvas.width - 20;
      if (car.y < 20) car.y = 20;
      if (car.y > canvas.height - 20) car.y = canvas.height - 20;

      // Collision with items (simple circle vs point)
      for (let i = items.length - 1; i >= 0; i--) {
        const it = items[i];
        const dx = car.x - it.x;
        const dy = car.y - it.y;
        const dist = Math.sqrt(dx * dx + dy * dy);
        if (dist < it.r + 10) {
          if (it.type === 'good') {
            score += 10;
            car.speed = Math.min(car.speed + 1.5, car.maxSpeed + 1); // boost
          } else {
            score -= 15;
            car.speed = Math.max(car.speed - 3, -1); // strong slowdown
          }
          items.splice(i, 1);
        }
      }

      // Respawn when all items collected
      if (items.length === 0) {
        spawnItems();
      }
    }

    function drawTrack() {
      // Simple "track" using lines/rectangles – no images
      ctx.strokeStyle = '#777';
      ctx.lineWidth = 10;
      ctx.strokeRect(40, 40, canvas.width - 80, canvas.height - 120);

      ctx.lineWidth = 2;
      ctx.setLineDash([10, 10]);
      ctx.strokeStyle = '#aaa';
      ctx.strokeRect(80, 80, canvas.width - 160, canvas.height - 200);
      ctx.setLineDash([]);
    }

    function drawCar() {
      ctx.save();
      ctx.translate(car.x, car.y);
      ctx.rotate(car.angle);
      ctx.fillStyle = '#0cf';
      ctx.fillRect(-car.width / 2, -car.height / 2, car.width, car.height);
      // "front" marker
      ctx.fillStyle = '#fff';
      ctx.fillRect(-car.width / 4, -car.height / 2, car.width / 2, 6);
      ctx.restore();
    }

    function drawItems() {
      for (const it of items) {
        ctx.beginPath();
        ctx.arc(it.x, it.y, it.r, 0, Math.PI * 2);
        ctx.fillStyle = it.type === 'good' ? '#3f3' : '#f33';
        ctx.fill();
      }
    }

    function drawHUD() {
      ctx.fillStyle = '#fff';
      ctx.font = '18px sans-serif';
      ctx.fillText(`Score: ${score}`, 10, 20);
    }

    function loop() {
      update();
      ctx.clearRect(0, 0, canvas.width, canvas.height);
      drawTrack();
      drawItems();
      drawCar();
      drawHUD();
      requestAnimationFrame(loop);
    }

    loop();
  </script>
</body>
</html>

This gives you:
	•	No sprites, no images.
	•	One car (rectangle) you can drive with arrow keys.
	•	Green circles (good) → points + speed boost.
	•	Red circles (bad) → points lost + slowdown.

From here, you can start adding your PI/SAFe flavor:
	•	Rename “good” items to Stories / Features / Enablers.
	•	Rename “bad” items to Defects / Incidents / Emails.
	•	Show an “End of PI” summary after X seconds or after N laps.

