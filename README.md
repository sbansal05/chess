#  Chess

A real-time, two-player online chess game. Players connect over WebSockets, get
matched into a game, and play chess with move validation handled by
[chess.js](https://github.com/jhlywa/chess.js) on the server.

## Overview

The project is split into two apps:

- **`frontend/`** — a React + TypeScript + Vite client (styled with Tailwind
  CSS) that renders the chessboard and handles player interaction.
- **`backend1/`** — a Node.js + TypeScript WebSocket server that pairs up
  players, tracks game state, and validates moves.

## Tech Stack

| Layer    | Tech |
|----------|------|
| Frontend | React 18, TypeScript, Vite, Tailwind CSS, React Router, chess.js |
| Backend  | Node.js, TypeScript, `ws` (WebSocket), chess.js |

## Project Structure

```
chess/
├── backend1/           # WebSocket game server
│   └── src/
│       ├── index.ts        # WebSocket server entry point
│       ├── GameManager.ts  # Matchmaking & message routing
│       ├── Game.ts         # Per-game state and move validation
│       └── messages.ts     # Shared message type constants
└── frontend/           # React client
    └── src/
        ├── screens/
        │   ├── Landing.tsx  # Landing / "play" screen
        │   └── Game.tsx     # Live game screen
        ├── components/
        │   ├── ChessBoard.tsx
        │   └── Button.tsx
        └── hooks/
            └── useSocket.ts # WebSocket connection hook
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org/) (v18+ recommended)
- npm

### 1. Clone the repository

```bash
git clone https://github.com/sbansal05/chess.git
cd chess
```

### 2. Run the backend

```bash
cd backend1
npm install
npx tsc -b        # compile TypeScript to dist/
node dist/index.js
```

The WebSocket server starts on **`ws://localhost:8080`**.

### 3. Run the frontend

In a separate terminal:

```bash
cd frontend
npm install
npm run dev
```

The client starts on the URL Vite prints (typically `http://localhost:5173`).

### 4. Play

Open the app in **two** browser tabs/windows and start a game in each — the
server pairs the first two waiting players together automatically.

## How It Works

1. A client connects and sends an `init_game` message.
2. The `GameManager` on the server pairs it with another waiting player and
   creates a new `Game`, assigning one player white and the other black.
3. Each move is sent as a `move` message; the server validates it with
   chess.js, updates the board state, and broadcasts the move to the
   opponent.
4. When the game ends (checkmate, stalemate, etc.), a `game_over` message is
   sent to both players with the result.

## Available Scripts

**Frontend** (`frontend/`)

| Command           | Description                     |
|--------------------|----------------------------------|
| `npm run dev`      | Start the Vite dev server        |
| `npm run build`    | Type-check and build for production |
| `npm run lint`     | Run ESLint                       |
| `npm run preview`  | Preview the production build     |

**Backend** (`backend1/`)

| Command             | Description                          |
|---------------------|---------------------------------------|
| `npx tsc -b`         | Compile TypeScript to `dist/`        |
| `node dist/index.js` | Run the compiled WebSocket server    |


- Deploy frontend and backend

## License

No license specified yet.
