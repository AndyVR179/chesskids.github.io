# Workspace

## Overview

Full-stack chess platform ("Chess") inspired by ChessKid.com. A kid-friendly chess website with working chess gameplay, friend system, leaderboard, and user profiles.

## Stack

- **Monorepo tool**: pnpm workspaces
- **Node.js version**: 24
- **Package manager**: pnpm
- **TypeScript version**: 5.9
- **API framework**: Express 5
- **Database**: PostgreSQL + Drizzle ORM
- **Validation**: Zod (`zod/v4`), `drizzle-zod`
- **API codegen**: Orval (from OpenAPI spec)
- **Build**: esbuild (CJS bundle)
- **Frontend**: React + Vite + Tailwind CSS
- **Chess engine**: chess.js
- **Auth**: Cookie-based sessions (bcryptjs for password hashing)

## Structure

```text
artifacts-monorepo/
├── artifacts/
│   ├── api-server/         # Express API server (auth, users, friends, games)
│   └── chess/              # React + Vite frontend
├── lib/
│   ├── api-spec/           # OpenAPI spec + Orval codegen config
│   ├── api-client-react/   # Generated React Query hooks
│   ├── api-zod/            # Generated Zod schemas from OpenAPI
│   └── db/                 # Drizzle ORM schema + DB connection
├── scripts/                # Utility scripts
├── pnpm-workspace.yaml
├── tsconfig.base.json
├── tsconfig.json
└── package.json
```

## Features

- **Chess gameplay**: Full interactive chess board with move validation, check/checkmate/stalemate detection, ELO rating system
- **Bot opponents**: 4 difficulty levels (Easy/Medium/Hard/Expert) powered by minimax AI with alpha-beta pruning and piece-square tables
- **Authentication**: Username/password registration and login with secure cookie sessions
- **Friend system**: Send/accept/decline friend requests, view friends list, challenge friends to games
- **Leaderboard**: Top players ranked by ELO rating
- **Player profiles**: Stats, game history, rating
- **Real-time polling**: Games poll every 2 seconds for opponent moves
- **Fun styling**: Fredoka/Nunito fonts, vibrant green/yellow color scheme, polka-dot background

## Database Schema

- `users` - Player accounts with ratings and stats
- `sessions` - Auth sessions (cookie-based)
- `friend_requests` - Pending friend requests
- `friendships` - Accepted friend connections
- `games` - Chess games with FEN, PGN, move history

## TypeScript & Composite Projects

Every package extends `tsconfig.base.json` which sets `composite: true`. The root `tsconfig.json` lists all packages as project references.

## Root Scripts

- `pnpm run build` — runs `typecheck` first, then recursively runs `build` in all packages
- `pnpm run typecheck` — runs `tsc --build --emitDeclarationOnly` using project references

## API Routes

- `POST /api/auth/register` — Register new account
- `POST /api/auth/login` — Login
- `POST /api/auth/logout` — Logout
- `GET /api/auth/me` — Get current user
- `GET /api/users/search?q=` — Search users by username
- `GET /api/users/:username` — Get user profile
- `GET /api/friends` — Get friends list
- `GET /api/friends/requests` — Get pending friend requests
- `POST /api/friends/send` — Send friend request
- `POST /api/friends/accept` — Accept friend request
- `POST /api/friends/decline` — Decline friend request
- `POST /api/friends/remove` — Remove friend
- `GET /api/friends/status/:userId` — Check friend status with user
- `GET /api/games` — Get user's games
- `POST /api/games` — Create new game vs friend
- `POST /api/games/bot` — Create new game vs bot (difficulty: easy/medium/hard/expert)
- `GET /api/games/:id` — Get game by ID
- `POST /api/games/:id/move` — Make a chess move (auto-applies bot response if bot game)
- `POST /api/games/:id/resign` — Resign from game
- `GET /api/leaderboard` — Get top players

## Production

- Push DB: `pnpm --filter @workspace/db run push`
- Codegen: `pnpm --filter @workspace/api-spec run codegen`
