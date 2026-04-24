# Jammix

Jammix is a social queue companion for Spotify. Instead of letting one person control the music or letting everyone pile random songs into the queue, Jammix builds a shared mix from the listening habits of everyone in the room.

The idea is simple: create a room, invite people to join, read the group's taste signals, and keep the queue evolving as the room changes.

## What Jammix Does

- Lets a host create a room from a laptop or phone
- Lets guests join with a room code, invite link, or QR code
- Uses Spotify login for each participant
- Pulls taste signals like top tracks, top artists, recently played songs, and saved tracks
- Builds a live room queue from the overlap in the group's listening habits
- Lets the host guide the room with a simple prompt like `party pop` or `no country`
- Lets guests vote to skip by majority
- Lets guests add protected songs manually
- Explains why songs were chosen
- Saves the final mix as a Spotify playlist

## Why It Exists

Group music usually breaks down in one of three ways:

1. One person owns the speaker and everyone else just deals with it.
2. Everyone adds songs and the queue becomes chaos.
3. The group falls back to a static playlist that does not actually fit the people in the room.

Jammix is meant to answer one question well:

> What should this room hear next?

## Core Product Shape

### Host Flow

1. Open Jammix.
2. Sign in with Spotify.
3. Create a room.
4. Share the room code, invite link, or QR code.
5. Optionally set a vibe prompt.
6. Let Jammix generate and keep updating the queue.
7. Save the final mix as a Spotify playlist.

### Guest Flow

1. Join with the room code, invite link, or QR code.
2. Sign in with Spotify.
3. Add your taste to the room.
4. Vote to skip, browse the queue, and add protected songs.
5. Save the final mix later if desired.

## MVP Scope

### In Scope

- Web app with desktop host view and mobile guest view
- Spotify OAuth for all participants
- Room creation and joining
- Real-time participant and queue updates
- Taste ingestion from top tracks, top artists, recently played tracks, and saved tracks
- Queue generation from combined group taste
- Host prompt input
- Majority skip voting
- Protected manual song adds
- Basic "why this song" explanations
- Playlist export to Spotify

### Not In Scope

- Native Spotify Jam integration
- Apple Music, YouTube Music, SoundCloud, or multi-platform support in MVP
- Native iOS or Android apps
- Advanced ML recommendation systems
- Persistent social graph features
- Monetization or payments
- Public launch materials beyond product documentation

## Product Principles

- Jammix owns the room state and virtual queue.
- Spotify is the playback destination, not the source of truth.
- The queue should feel fair, social, and adaptive.
- Manual additions should survive recalculation.
- Privacy should be clear and conservative.

## Spotify Integration Notes

Jammix is designed as a companion app for Spotify, not an official Spotify product.

The planned MVP uses Spotify's public Web API to:

- Authenticate users
- Read profile information
- Read top tracks and top artists
- Read recently played tracks
- Read saved tracks
- Search tracks
- Add tracks to the host's queue
- Create and save playlists

Current public Spotify APIs do not expose a native Jam management API, so Jammix should run its own room and queue logic, then push upcoming songs into the host's Spotify playback queue.

## Development Plan

### Current Phase

Phase `0`: product and technical setup

Current repo state:

- Static GitHub Pages landing page in [index.html](./index.html)
- Single project brief in this `README.md`
- Environment template in [.env.example](./.env.example)
- Prisma configuration in [prisma.config.ts](./prisma.config.ts)
- Initial schema draft in [prisma/schema.prisma](./prisma/schema.prisma)

### Roadmap

| Phase | Focus | Outcome |
| --- | --- | --- |
| 0 | Product and technical setup | Clear scope, setup assumptions, env, schema |
| 1 | Web app foundation | First frontend and backend shells |
| 2 | Spotify authentication | Working Spotify sign-in and session handling |
| 3 | Rooms | Hosts can create rooms and guests can join |
| 4 | Realtime | Queue and room updates sync live |
| 5 | Taste ingestion | Spotify taste signals are fetched and normalized |
| 6 | Queue engine | Shared group queue starts working |
| 7 | Host prompts | Prompt nudges queue scoring |
| 8 | Voting and manual adds | Democratic control plus protected additions |
| 9 | Playback integration | Jammix pushes songs to Spotify playback |
| 10 | Playlist export | Final session becomes a Spotify playlist |
| 11 | MVP polish | UX, analytics, accessibility, and onboarding cleanup |
| 12 | Closed beta | Real-world testing with groups |

### Immediate Phase 0 Work

- Create the Spotify Developer app
- Add local and production callback URLs
- Copy credentials into local `.env`
- Create the GitHub board and labels
- Start Phase 1 app scaffolding

### Phase 0 Notes

- Added a static GitHub Pages landing page with a public project overview.
- Consolidated the planning and product notes into this single `README.md`.
- Removed the extra roadmap and whitepaper documents to keep the repo simpler.
- Kept the environment and Prisma setup files needed for development handoff.

## Technical Direction

### Stack

- Frontend: `Next.js` with TypeScript
- UI: `Tailwind CSS` and `shadcn/ui`
- Backend API: `NestJS` with TypeScript
- Realtime: `Socket.IO`
- Jobs and cache: `Redis` and `BullMQ`
- Database: `PostgreSQL`
- ORM: `Prisma`
- Monitoring: `Sentry`
- Analytics: `PostHog`

### Hosting Plan

- Frontend on `Vercel`
- Backend, workers, and Redis on `Railway`
- PostgreSQL on `Supabase`

### Local Assumptions

- Frontend runs on `http://localhost:3000`
- Backend runs on `http://localhost:4000`
- Spotify callback hits the backend
- Redis and Postgres can run locally or be replaced with hosted dev instances

## Spotify App Setup

Create a Spotify Developer app with:

- App name: `Jammix`
- Public description: `A social queue companion for Spotify`
- Local callback: `http://localhost:4000/auth/spotify/callback`
- Production callback: `https://api.jammix.app/auth/spotify/callback`

Scopes planned for MVP:

- `user-read-private`
- `user-top-read`
- `user-read-recently-played`
- `user-library-read`
- `user-read-playback-state`
- `user-read-currently-playing`
- `user-modify-playback-state`
- `playlist-modify-private`
- `playlist-modify-public`

## Environment Variables

Start from [.env.example](./.env.example). The key values are:

```txt
APP_BASE_URL=http://localhost:3000
API_BASE_URL=http://localhost:4000
SPOTIFY_CLIENT_ID=
SPOTIFY_CLIENT_SECRET=
SPOTIFY_REDIRECT_URI=http://localhost:4000/auth/spotify/callback
DATABASE_URL=postgresql://postgres:postgres@localhost:5432/jammix
REDIS_URL=redis://localhost:6379
SESSION_SECRET=
JWT_SECRET=
```

## Initial Data Model

The schema draft in [prisma/schema.prisma](./prisma/schema.prisma) currently covers:

- Users and Spotify accounts
- Rooms and participants
- Tracks and artists
- User taste signals
- Spotify sync state
- Queue items and votes
- Playlist exports

## Project Workflow

### Suggested Board Columns

- Backlog
- Ready
- In Progress
- In Review
- Blocked
- Done

### Suggested Labels

- `type:feature`
- `type:bug`
- `type:chore`
- `type:docs`
- `type:research`
- `type:infra`
- `area:frontend`
- `area:backend`
- `area:auth`
- `area:database`
- `area:realtime`
- `area:queue-engine`
- `area:spotify`
- `area:analytics`
- `area:product`
- `priority:p0`
- `priority:p1`
- `priority:p2`
- `priority:p3`
- `blocked`
- `needs-decision`
- `good-first-issue`

### Suggested First Tickets

- Scaffold Next.js frontend
- Scaffold NestJS backend
- Add Tailwind CSS and shadcn/ui
- Add Prisma and connect PostgreSQL
- Implement Spotify OAuth with PKCE
- Create room model and room creation API
- Build room join flow
- Add Socket.IO room presence events
- Create taste ingestion jobs
- Draft the queue scoring service

## Notes

- Spotify Premium will likely be required for host playback control.
- Jammix should not imply Spotify endorsement or ownership.
- Jammix should explain clearly that joining a room means your Spotify taste will shape the queue.
