# Eero's Rocket Software 🚀

A model rocket launch simulation game built by Eero (age 10) and his aunt Ivana using Claude Code.

## What this game does

- **Build a rocket** — pick a real Estes kit or make your own from parts
- **Customize it** — change the nose cone, body tube, fins, motor, and fuel tank
- **Launch it** — watch it fly up the screen with real physics
- **See what happens** — parachute comes out at the top, or things go wrong (crash!)
- **Beat your high score** — how high can you go? (saved on your computer)
- **Ask the rocket assistant** — a built-in chat helper that knows everything about model rockets

---

## Tech Stack

- **One file only:** `index.html` (HTML + CSS + JavaScript — no frameworks, no downloads needed)
- **Open in any browser** — just double-click the file!
- **Works offline** — no internet needed

---

## Rules (important!)

- ALWAYS keep everything in one `index.html` file
- NEVER use React, Vue, or any other framework
- NEVER use `npm install` or any packages
- Keep the code simple and easy to read — this is a learning project
- After every feature you finish, remind Eero to save to GitHub
- ⚠️ This repo is PRIVATE — never change it to public (the code belongs to Eero)

---

## How to run

Just open `index.html` in any browser. That's it!

---

## Game Vision (Eero's plan)

### Screen 1 — Rocket Builder
- Choose a **preset real Estes kit** (see list below) or click "Build My Own"
- Drag or click to change parts:
  - **Nose cone** — different shapes (ogive, conical, parabolic)
  - **Body tube** — change the length
  - **Fins** — number of fins (3 or 4), shape (delta, trapezoidal, elliptical)
  - **Motor** — real Estes codes (A8-3, B6-4, C6-5, D12-3, etc.)
  - **Recovery** — parachute size
- See a **live drawing** of the rocket update as you change parts
- The **Rocket Assistant chat** is open on the side — ask it anything!

### Screen 2 — Launch
- Side view: rocket sits on a launchpad at the bottom
- Countdown: 5… 4… 3… 2… 1… LAUNCH!
- Rocket flies UP the screen — the camera follows it
- Background changes as it climbs (green ground → blue sky → dark upper sky)
- Real physics happening every frame (see Physics section below)
- **Things that can go wrong:**
  - Rocket tips over if fins are too small
  - Motor misfires (random small chance)
  - Structural failure if motor is too powerful for the rocket weight
- At max height: parachute pops out, rocket floats slowly back down
- If it crashes: show a crash animation and message

### Screen 3 — Results
- Max altitude reached (in meters and feet)
- Flight time
- Personal best record (saved with localStorage)
- Button to try again or change the design

---

## Preset Estes Rockets (real kits!)

| Name | Length | Mass (no motor) | Recommended Motor |
|------|--------|-----------------|-------------------|
| Estes Alpha | 43 cm | 28 g | B6-4 |
| Estes Viking | 48 cm | 31 g | C6-5 |
| Estes Big Bertha | 69 cm | 113 g | B6-4 |
| Estes Wizard | 38 cm | 22 g | A8-3 |
| Estes Mosquito | 20 cm | 9 g | 1/2A3-2T |
| Estes Bull Pup | 51 cm | 43 g | D12-3 |

---

## Real Motor Data (Estes)

These are the real numbers! Use them in the physics simulation.

| Motor Code | Avg Thrust | Total Impulse | Burn Time | Delay | Propellant Mass |
|------------|-----------|---------------|-----------|-------|-----------------|
| 1/2A3-2T | 3.3 N | 0.6 N·s | 0.18 s | 2 s | 1.3 g |
| A8-3 | 8.0 N | 2.5 N·s | 0.32 s | 3 s | 3.1 g |
| B6-4 | 6.0 N | 5.0 N·s | 0.85 s | 4 s | 5.0 g |
| C6-5 | 6.0 N | 10.0 N·s | 1.9 s | 5 s | 11.0 g |
| D12-3 | 12.0 N | 20.0 N·s | 1.7 s | 3 s | 20.7 g |
| E12-4 | 12.0 N | 40.0 N·s | 3.0 s | 4 s | 37.0 g |

**Motor code explained:** `C6-5` means:
- `C` = total impulse class (C = 5–10 N·s)
- `6` = average thrust in Newtons
- `5` = delay in seconds before ejection charge fires

---

## Physics Engine

Use simple **Euler integration** — update position and velocity every animation frame (every ~16ms).

### Forces on the rocket:

```
Thrust (up)   — from motor during burn time
Weight (down) — mass × 9.81 m/s²
Drag (down)   — 0.5 × Cd × air_density × area × velocity²
```

### Key values:
- Air density at sea level: **1.225 kg/m³**
- Drag coefficient (Cd): **0.5** (typical for model rockets)
- Gravity: **9.81 m/s²**
- Mass changes as propellant burns! Start with (rocket mass + propellant mass), subtract propellant linearly during burn

### Flight phases:
1. **Powered** — motor burning, thrust > 0
2. **Coast** — motor burned out, coasting upward (delay timer counting down)
3. **Ejection** — delay timer hits 0, parachute deploys
4. **Descent** — parachute open, high drag, slow fall

### Stability check:
- If (fin area × 3) < (body tube diameter × length × 0.1) → rocket is unstable → it tips!

---

## Rocket Assistant (Built-in)

A chat popup in the corner. The user types a question, and the assistant replies with pre-written helpful answers based on keywords and the current rocket design.

**What it knows about:**
- What each part does (nose cone, fins, body tube, motor, parachute)
- How to go higher (lighter rocket + bigger motor)
- Stability (fins need to be big enough)
- Motor choice for different rockets
- What the motor codes mean
- Real model rocketry safety rules
- Fun facts about real rockets

**How it works:**
- Check the typed message for keywords (e.g. "higher", "fins", "motor", "crash", "parachute")
- Also check the current rocket's stats (is it stable? is the motor too powerful?)
- Pick the best matching pre-written response
- If nothing matches: "Great question! Try asking about fins, motors, altitude, or stability."

---

## Visual Style

- Technical and realistic (not cartoon)
- Dark background for the launch screen (space feel)
- Clean, engineering-style UI for the builder (like a real rocket design tool)
- Rocket drawn with clean lines and accurate proportions
- Show real measurements and numbers

---

## High Score

- Saved with `localStorage` (stays even when you close the browser)
- Key: `eeros_rocket_highscore`
- Show: altitude in meters, which rocket design achieved it, date

---

## Building Order (suggested steps for Eero)

1. Start with a basic `index.html` with three screens (builder, launch, results)
2. Draw a simple rocket shape on a canvas using JavaScript
3. Add the physics engine — make the rocket go up with real numbers
4. Add the launch animation (rocket flies up, camera follows)
5. Add parachute and landing
6. Add the preset Estes rockets to choose from
7. Add the customization controls
8. Add the rocket assistant chat
9. Add crashes and things going wrong
10. Add high score saving
11. Polish the visuals and add details

---

## GitHub

Repo: `https://github.com/ivanaist/eeros-rocket-software` (private)

After each step, save to GitHub:
```
git add index.html
git commit -m "add [what you just built]"
git push
```
