# Wumpus World — AI Knowledge-Based Agent

> An interactive implementation of the classic **Wumpus World** problem using **Propositional Logic** and **Resolution-Based Inference** in Python (Flask).

---

## Authors

| Name | Student ID |
|------|-----------|
| Taimoor Ahmad | F23-0551 |

**Section:** BCS - 6E

---

## Overview

This project simulates the **Wumpus World**, a classic AI environment from *Artificial Intelligence: A Modern Approach* by Russell & Norvig. An agent navigates a grid-based cave system, using a **propositional logic knowledge base** and **resolution theorem proving** to infer which cells are safe — without directly seeing the grid.

The agent perceives:
- **Breeze** — a nearby pit
- **Stench** — the Wumpus is adjacent
- **Glitter** — gold is at the current cell

Based on these percepts, the **Inference Engine** reasons about the environment and identifies provably safe cells to move into.

---

## Features

- 🧠 **Knowledge Base** built on propositional logic clauses
- 🔍 **Resolution-based entailment** to prove safety of cells
- 🗺️ **Configurable grid** (default 5×5)
- ⚠️ **Hazards:** 2 Pits + 1 Wumpus placed randomly
- 🏆 **Win/Loss detection:** find the gold to win, hit a pit or Wumpus to lose
- 📊 **Inference step counter** tracking reasoning effort
- 🌐 **Web interface** served via Flask

---

## Project Structure

```
wumpus-world/
├── app.py              # Flask backend — game logic, agent, API routes
├── kb.py               # Propositional logic engine (Clause + KnowledgeBase)
├── requirements.txt    # Python dependencies
└── templates/
    └── index.html      # Frontend (HTML/JS game interface)
```

---

## How It Works

### Knowledge Representation
Each cell `(r, c)` has two propositions:
- `P_r_c` — there is a **pit** at row `r`, column `c`
- `W_r_c` — the **Wumpus** is at row `r`, column `c`

### Percept → Clause Conversion
| Percept | Inference |
|---------|-----------|
| **No Breeze** | All adjacent cells are pit-free (unit clauses added) |
| **Breeze** | At least one adjacent cell contains a pit (disjunctive clause) |
| **No Stench** | All adjacent cells are Wumpus-free |
| **Stench** | Wumpus is in at least one adjacent cell |

### Entailment via Resolution
To check if a cell `(r, c)` is safe, the engine:
1. Copies the current KB
2. Adds the **negation** of the safety claim
3. Runs `resolve_all()` — iterative pairwise resolution
4. If an **empty clause** (contradiction) is derived → the cell is provably safe ✅

---

## Installation & Running

### 1. Clone / Download the project
```bash
git clone <your-repo-url>
cd wumpus-world
```

### 2. Install dependencies
```bash
pip install -r requirements.txt
```

### 3. Run the Flask server
```bash
python app.py
```

### 4. Open in browser
```
http://127.0.0.1:5000
```

---

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| `GET` | `/` | Serves the game frontend |
| `POST` | `/create_grid` | Creates a new game world |
| `POST` | `/move` | Moves the agent (`up`, `down`, `left`, `right`) |

### `/create_grid` — Request Body
```json
{
  "rows": 5,
  "cols": 5
}
```

### `/move` — Request Body
```json
{
  "action": "up"
}
```

### `/move` — Response Example
```json
{
  "agent_pos": { "row": 1, "col": 0 },
  "game_over": false,
  "inference_steps": 4,
  "current_percepts": "Breeze: True, Stench: False, Glitter: False",
  "safe_cells": [{ "row": 1, "col": 1 }],
  "visited_cells": [{ "row": 0, "col": 0 }, { "row": 1, "col": 0 }]
}
```

---

## Game Rules

1. The agent always **starts at (0, 0)**, which is guaranteed safe.
2. The agent can move **up, down, left, right** — one cell at a time.
3. Moving into a **pit** or the **Wumpus cell** → instant death 💀
4. Reaching the **gold cell** → victory 🏆
5. The agent should only move into cells that the KB has proven **safe**.

---

## Dependencies

| Package | Version |
|---------|---------|
| Flask | >= 2.0 |

---

## Concepts Demonstrated

- Propositional Logic & Clause Representation
- Resolution Theorem Proving (Refutation)
- Knowledge-Based Agent Design
- REST API with Flask
- Game State Management

---

## License

This project was developed as part of an academic assignment. All rights reserved by the authors.# DynamicWumpusLogicAgent
