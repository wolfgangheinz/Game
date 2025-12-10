# Taskmaster PI Planning Game

A lightweight browser game to introduce PI planning concepts through a playful boat‑racing metaphor.

You steer a small planning boat through four iterations, collecting Stories and Features (good work) while avoiding Defects and Incidents (unplanned work and production pain). Your final score represents your **total velocity** for the PI.

---

## How to Start the Game

1. **Download the game HTML file**
   - From the repo: download `src/index.html` to your computer.
   - Or in Confluence: attach `index.html` to this page (see “Download button” below).

2. **Save it locally**
   - Make sure the file is saved as `index.html` (or any name ending in `.html`).

3. **Open in a browser**
   - Double‑click the file or open it via your browser (`File → Open`).
   - No server or installation is needed; it’s a pure client‑side HTML5 game.

4. **Controls**
   - `↑` / `↓` – Accelerate / brake.
   - `←` / `→` – Steer left / right (boat turns more when moving).
   - `Start` button – Begin the current iteration (with a short countdown).
   - `Enter full screen` – Toggle full screen mode for a more immersive demo.

---

## Game Elements

### Coins / Items

All items are round coins with a letter:

- **S (Story)** – Yellow  
  - Good work, small score boost, small speed boost.
- **F (Feature)** – Orange  
  - Good work, bigger score boost, stronger speed boost.
- **D (Defect)** – Purple  
  - Bad work, negative score, and in hazard iterations it slows you down.
- **I (Incident)** – Red  
  - Production incidents / major unplanned work. Strong negative score and can cost you a life.

### Basic Rules

- Your boat moves around a top‑down track.
- An **iteration ends when all Stories and Features have been collected**.  
  (Defects and Incidents can remain on the board.)
- Your **score** in an iteration:
  - Sum of points from items:
    - Feature: +10  
    - Story: +3  
    - Defect: −3  
    - Incident: −10
  - Plus a **time bonus/penalty**:
    - Finish faster than **60 seconds** → **+1 point per second under 60**.  
    - Take longer than **60 seconds** → **−1 point per second over 60**.
- The HUD shows:
  - Iteration number, time, speed, current iteration score.
  - Lives (for hazard iterations).
  - Counts of S, F, D, I collected.

---

## Iterations

### Iteration 1 – Static Work

- All items are **static**.
- Goal: Collect all **S** and **F** as fast as possible.
- This is the “cleanest” iteration: a relatively calm board with no movement.

---

### Iteration 2 – Work in Motion

- All items move slowly and **bounce off the walls**.
- Same scoring rules as Iteration 1.
- You now have to plan and react to moving work on the board.

---

### Iteration 3 – Hazards and Lives

- Moving items + hazard rules:
  - **Defect (D)**:
    - Immediately slows you: your effective max speed is limited to **50%** for **3 seconds**.
  - **Incident (I)**:
    - Costs **1 life** and “kills” you:
      - The boat stops.
      - After **1 second**, you respawn back at the starting position.
- You start the iteration with **3 lives**:
  - If you lose all lives:
    - The run **ends immediately**.
    - Your final score only includes **Iteration 1 + 2**.
    - You still see a “PI planning complete” message referencing the total velocity from those two iterations.

---

### Iteration 4 – Legacy Systems Breaking Down

- Same hazard rules as Iteration 3 (Defects slow you, Incidents cost lives).
- You still have 3 lives.
- After **15 seconds**:
  - A message appears: **“The legacy systems are breaking down”**.
  - From then on:
    - **Every 1 second**: a new **Defect (D)** coin is added.
    - **Every 3 seconds**: a new **Incident (I)** coin is added.
- The board gradually fills with more Defects and Incidents, illustrating how legacy systems can generate accelerating unplanned work.
- If you survive and collect all Stories and Features:
  - The run is **successfully completed** with combined score from **Iterations 1–4**.
  - You see:
    - Final combined score.
    - Message:  
      **“PI planning complete! The score is shown at total velocity.”**

---

## Combined Score and High Scores

- **Combined score** is your sum of all iteration scores (including time bonuses/penalties), up to:
  - Iterations 1–2 (if you die in Iteration 3),
  - Iterations 1–3 (if you stop there),
  - or all Iterations 1–4 (if you finish everything).
- A **High Scores** list on the right shows the top 5 combined scores:
  - Stored in your browser’s `localStorage`.
  - Survives page reloads on the same browser/machine.
  - A “run” is exactly one complete sequence up to death or end of Iteration 4.

---

## Suggested Download Button (for Confluence)

After you attach `index.html` to this Confluence page, you can turn the link below into a proper download button using Confluence’s UI or macros.

```markdown
[⬇️ Download Taskmaster PI Planning Game (HTML)](index.html)
```

- In Confluence:
  1. Attach `index.html` to the page.
  2. Edit the link target to point to that attachment (Confluence usually autocompletes it).
  3. Optionally style it as a button using Confluence’s formatting options.

Once that’s set up, users can:
- Click the **Download** link/button.
- Save `index.html` locally.
- Open it in their browser to start playing the game.

