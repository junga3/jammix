# Jammix

Jammix is a Spotify companion app concept for creating better group music.

Instead of letting one person control the music or letting everyone flood the queue, Jammix builds a shared mix from the listening habits of everyone in the room. As more people join, the queue evolves in real time.

## What It Does

Jammix lets a host create a room, invite friends with a link, QR code, or room code, and automatically generate a Spotify queue based on the group's combined taste.

Users sign in with Spotify, join the room, and Jammix looks at signals like:

- Recently played songs
- Top tracks
- Top artists
- Saved songs
- Shared artist and track overlap

The result is a live group queue that favors songs the room is most likely to enjoy.

## Core Features

- Create a Jammix room from any device
- Join with a room code, invite link, or QR code
- Sign in with Spotify
- Build a queue from group listening habits
- Update the queue as new people join
- Let the host set a vibe prompt, like "party pop" or "chill dinner"
- Let users vote to skip by majority
- Let users manually add protected songs
- Explain why songs were chosen
- Show Spotify profile names and images
- Save the final mix as a Spotify playlist

## Why Jammix Exists

Group music is usually awkward.

One person controls the speaker, everyone argues over the queue, or the group settles for a playlist that does not really fit who is there. Jammix is designed to make group listening feel more natural by answering one question:

> What should this room hear next?

## Spotify Integration

Jammix is designed as a companion app for Spotify, not an official Spotify product.

The planned MVP uses the public Spotify Web API to:

- Authenticate users with Spotify
- Read profile information
- Read top tracks and artists
- Read recently played tracks
- Read saved tracks
- Search for songs
- Add tracks to the host's Spotify queue
- Create and save Spotify playlists

Current public Spotify APIs do not appear to provide direct control over native Spotify Jam sessions. Because of that, Jammix keeps its own virtual queue and pushes upcoming songs to the host's Spotify queue.

## Recommended Tech Stack

The recommended build stack is:

- **Frontend:** Next.js, React, TypeScript
- **UI:** Tailwind CSS, shadcn/ui
- **Backend:** NestJS or Fastify with TypeScript
- **Database:** PostgreSQL via Supabase or Neon
- **ORM:** Prisma
- **Realtime:** Socket.IO
- **Cache and jobs:** Redis, BullMQ
- **Auth:** Spotify OAuth 2.0
- **Hosting:** Vercel for the frontend and Render, Fly.io, or Railway for the backend
- **Monitoring:** Sentry
- **Analytics:** PostHog or Plausible

See [ROADMAP.md](./ROADMAP.md) for the full technical roadmap.

## Project Status

Jammix is currently in the planning and product design phase.

This repository contains:

- [JAMMIX_WHITEPAPER.md](./JAMMIX_WHITEPAPER.md): product and technology whitepaper
- [ROADMAP.md](./ROADMAP.md): recommended implementation roadmap
- [README.md](./README.md): general project overview

Code has not been implemented yet.

## MVP Goal

The first working version should allow:

1. A host to create a Jammix room.
2. Guests to join with Spotify.
3. Jammix to generate a group queue.
4. The queue to update in real time as people join.
5. Users to vote to skip songs.
6. Users to manually add protected songs.
7. The host to push upcoming songs to Spotify.
8. The group to save the final mix as a Spotify playlist.

## Important Notes

- Spotify Premium is likely required for the host account when controlling playback or adding songs to the active queue.
- Jammix should use its own virtual queue as the source of truth.
- Jammix should not imply that it is owned, endorsed, or operated by Spotify.
- Spotify user data should be handled carefully and explained clearly before login.

## Vision

Jammix makes group music feel like it belongs to the room.

It is built for parties, dorms, friend groups, and shared spaces where music matters but managing the queue gets messy. The long-term goal is to make group listening smarter, fairer, and more fun.
