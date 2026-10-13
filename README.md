# AI Navigation — Technical Documentation Hub

Welcome to the central technical documentation repository for the **AI Navigation** project. All architectural designs, data flow specifications, module contracts, learning mechanics, and execution logs are maintained within this dedicated directory (`docs/`).

---

## 📑 Documentation Structure

| Document | Purpose & Scope |
| :--- | :--- |
| **[01_ARCHITECTURE.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/01_ARCHITECTURE.md)** | End-to-end system architecture, component hierarchies, and topological flow. |
| **[02_DATA_PIPELINE.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/02_DATA_PIPELINE.md)** | Data sources, sensor streams, preprocessing pipelines, tensor dimensions, and datasets. |
| **[03_MODEL_LEARNING.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/03_MODEL_LEARNING.md)** | Algorithmic formulation, loss/reward dynamics, network architectures, and training loop mechanics. |
| **[04_MODULES_SPECIFICATION.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/04_MODULES_SPECIFICATION.md)** | Modular breakdown, interface contracts, input/output schemas, and class hierarchies. |
| **[05_VERIFICATION_AND_TESTING.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/05_VERIFICATION_AND_TESTING.md)** | Unit verification, numerical sanity tests, trajectory validation metrics, and test protocols. |
| **[EXECUTION_LOG.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/EXECUTION_LOG.md)** | Chronological ledger of implementations, verification outcomes, and milestone progression. |

---

## 🎯 Documentation Principles
1. **Single Source of Truth**: All technical decisions, pipeline specifications, and mathematical formulations are tracked here.
2. **Deterministic Validation**: Every module or feature documented includes a formal verification test to prove correct implementation.
3. **Traceability**: Changes made during execution will be appended to [EXECUTION_LOG.md](file:///d:/Proof%20Of%20projects/AI%20Navigation/docs/EXECUTION_LOG.md) with concise reports and validation checkpoints.


# AI-Based Autonomous Navigation System

## Executive Summary
The AI-Based Autonomous Navigation System is a modular mobile robot simulation platform designed for deterministic path planning, kinematic tracking, and environmental perception. The system incorporates a discrete two-dimensional occupancy-grid simulation environment, an optimal four-directional A* search engine using an admissible Manhattan heuristic, and an interactive local dashboard for real-time visualization and algorithm inspection.

The project follows test-driven development methodologies, maintaining comprehensive test coverage across coordinate geometry, obstacle containment, search optimality, and grid immutability.

---

## Visual Demonstration

![Simulation Dashboard Overview](images/dashboard_overview.png)
*Figure 1: Interactive 2D occupancy grid dashboard with live telemetry HUD and real-time path planning.*

![A* Pathfinding Demonstration](images/pathfinding_demo.png)
*Figure 2: Optimal four-directional A* search path navigating around concave obstacle barriers.*

*(Note: Image files can be placed directly in the `images/` directory to update visual assets).*

---

## Core Capabilities

### 1. Deterministic 2D Occupancy Grid Engine
- Backed by an optimized two-dimensional NumPy array representation with shape `(height, width)`.
- Adheres to explicit coordinate conventions where external calls use `(x, y)` and internal buffer access uses `grid[y, x]`.
- Implements defensive copies and read-only memory flags to prevent external mutation of internal simulation state.
- Supports single-point and atomic batch obstacle placement with boundary validation.

### 2. Four-Directional A* Path Planning
- Implements optimal heuristic search constrained to four orthogonal movements: North, East, South, and West. Diagonal movements are disallowed.
- Utilizes the Manhattan distance heuristic ($L_1$ norm), which is both admissible and consistent under unit-cost movement assumptions:
  $$h(p_1, p_2) = |x_1 - x_2| + |y_1 - y_2|$$
- Employs a priority queue with a monotonic FIFO tie-breaking counter, guaranteeing strictly deterministic route outputs across identical map configurations.
- Enforces strict endpoint validation, explicitly rejecting out-of-bounds or obstacle coordinates.

### 3. Interactive Web Simulation Dashboard
- Built with a modern, glassmorphic dark-theme user interface running locally via Python standard library HTTP server.
- Provides interactive canvas drawing tools allowing users to place walls, reposition start and goal endpoints, generate maze barriers, and run random obstacle scatters.
- Supports real-time robot path animation with adjustable speed controls.
- Delivers live telemetry metrics including step count, Manhattan distance, backend latency in milliseconds, and obstacle density percentage.

---

## System Architecture

```
+-----------------------------------------------------------------------------+
|                            Client Browser Layer                             |
|    - HTML5 Canvas Viewport        - Telemetry HUD                           |
|    - Dynamic Tool Selector        - Real-Time Animation Controller          |
+--------------------------------------^--------------------------------------+
                                       | HTTP / REST API (port 8000)
+--------------------------------------v--------------------------------------+
|                           Application Server Layer                          |
|    - web_server.py (ThreadingHTTPServer)                                    |
|    - Endpoints: GET /, GET /api/health, POST /api/plan                      |
+--------------------------------------^--------------------------------------+
                                       | Python Function Calls
+--------------------------------------v--------------------------------------+
|                           Core Algorithmic Engine                           |
|  +--------------------------------+    +---------------------------------+  |
|  |     simulation.environment     |    |       src.navigation.astar      |  |
|  |     - GridEnvironment Class    |    |       - astar_search()          |  |
|  |     - Coordinate Validation    |<-->|       - manhattan_distance()    |  |
|  |     - Defensive Array Views    |    |       - Priority Queue (heapq)  |  |
|  +--------------------------------+    +---------------------------------+  |
+-----------------------------------------------------------------------------+
```

---

## Repository Structure

```
AI-Autonomous-Navigation-System/
├── README.md                          # Master documentation and technical overview
├── requirements.txt                   # Production and testing dependencies
├── pytest.ini                         # Automated test discovery configuration
├── .gitignore                         # Git exclusion rules for caches, envs, and artifacts
├── .env.example                       # Configuration template without credentials
├── main.py                            # Primary execution entry point
├── web_server.py                      # Standalone HTTP server and planning API bridge
├── config/
│   ├── __init__.py
│   └── settings.py                    # Global dimensions, grid defaults, and cell constants
├── data/
│   ├── maps/                          # Serialized grid maps and benchmark scenarios
│   └── scenarios/                     # Structured navigation test scenarios
├── docs/
│   ├── architecture.md                # Planned architecture specification
│   ├── implementation_plan.md         # Phased development roadmap
│   ├── testing.md                     # Test strategy and validation methodology
│   ├── limitations.md                 # Design boundaries and assumptions
│   └── EXECUTION_LOG.md               # Chronological audit log of all implemented phases
├── images/
│   ├── README.md                      # Asset guidelines
│   └── .gitkeep                       # Image asset directory
├── simulation/
│   ├── __init__.py
│   ├── environment.py                 # Core GridEnvironment implementation
│   └── renderer.py                    # Desktop Pygame renderer placeholder
├── src/
│   ├── __init__.py
│   ├── navigation/
│   │   ├── __init__.py
│   │   ├── astar.py                   # Deterministic A* planning implementation
│   │   ├── controller.py              # Motion controller placeholder
│   │   └── obstacle_avoidance.py      # Reactive obstacle avoidance placeholder
│   ├── robot/
│   │   ├── __init__.py
│   │   ├── robot_model.py             # Differential-drive kinematic model placeholder
│   │   └── sensors.py                 # Simulated range sensor placeholder
│   └── utils/
│       ├── __init__.py
│       └── path_utils.py              # Geometric conversion and path helpers
└── tests/
    ├── __init__.py
    ├── test_astar.py                  # Unit tests for A* search and heuristics (27 tests)
    ├── test_collision.py              # Unit tests for boundary containment (3 tests)
    └── test_environment.py            # Unit tests for GridEnvironment and immutability (37 tests)
```

---

## Installation and Setup

### Prerequisites
- Python 3.10 or higher (Python 3.13 recommended)
- Git installed on your system

### Step 1: Clone the Repository
```bash
git clone https://github.com/YOUR_USERNAME/AI-Autonomous-Navigation-System.git
cd AI-Autonomous-Navigation-System
```

### Step 2: Set Up Virtual Environment (Recommended)
```bash
python -m venv .venv
# On Windows:
.venv\Scripts\activate
# On Linux / macOS:
source .venv/bin/activate
```

### Step 3: Install Dependencies
```bash
pip install -r requirements.txt
```

---

## Running the Application

### Option 1: Primary Entry Point
Launch the interactive web simulation server using:
```bash
python main.py
```

### Option 2: Standalone Web Server
Alternatively, launch the server module directly:
```bash
python web_server.py
```

Once running, navigate to the following URL in any web browser:
```
http://localhost:8000
```

---

## Testing and Quality Assurance

The codebase includes an automated test suite executed via pytest. Tests validate mathematical correctness, array integrity, boundary edge cases, and deterministic search outputs.

To execute the complete test suite:
```bash
pytest -v
```

Or using an explicit Python executable:
```bash
python -m pytest -v
```

### Current Test Suite Coverage
- `tests/test_environment.py`: 37 tests covering initialization, coordinates, boundaries, obstacle assignment, and defensive copy immutability.
- `tests/test_astar.py`: 27 tests covering empty grids, obstacle detours, unreachable goals, endpoint validation, non-diagonal constraints, and determinism.
- `tests/test_collision.py`: 3 tests covering boundary and obstacle collision queries.
- Total: 67 passed tests (100% pass rate).

---

## Security and Configuration Management

- Zero Hardcoded Credentials: No API keys, passwords, or cloud credentials exist in source code or revision history.
- Environment Isolation: All local environment files (`.env`, `.env.*`) are excluded via `.gitignore`.
- Configuration Template: A clean `.env.example` file is provided for optional local parameter customization.

---

## Development Roadmap

- Phase 1: Occupancy-Grid Environment and Grid Validation (Completed).
- Phase 2: Deterministic Four-Directional A* Path Planning (Completed).
- Phase 3: Kinematic Robot Modeling and Differential-Drive Motion Control (Planned).
- Phase 4: Simulated Proximity Range Sensors and Ray Casting (Planned).
- Phase 5: Reactive Obstacle Avoidance Integration (Planned).
- Phase 6: Benchmark Metrics and Empirical Comparative Evaluation (Planned).
