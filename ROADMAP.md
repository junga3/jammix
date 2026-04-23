# Jammix Project Roadmap

This roadmap outlines the recommended technical path for building Jammix: a Spotify companion app that creates a real-time, adaptive group queue from the listening habits of everyone in a room.

## Recommended Technology Stack

| Layer | Technology | Purpose |
| --- | --- | --- |
| Web app | Next.js, React, TypeScript | Host dashboard, guest mobile web view, routing, server-rendered pages |
| UI | Tailwind CSS, shadcn/ui | Fast, polished interface with reusable components |
| Backend API | NestJS or Fastify with TypeScript | Spotify OAuth, rooms, queue engine, votes, playlist export |
| Database | PostgreSQL via Supabase or Neon | Users, rooms, participants, tracks, votes, sessions |
| ORM | Prisma | Type-safe database schema and queries |
| Realtime | Socket.IO | Live room updates, presence, queue changes, skip votes |
| Cache and jobs | Redis, BullMQ | Background jobs, Spotify API rate limiting, queue recalculation |
| Auth | Spotify OAuth 2.0 Authorization Code with PKCE | Spotify login and secure access to listening data |
| Hosting | Vercel, Render, Fly.io, or Railway | Frontend hosting plus long-running backend/WebSocket service |
| Monitoring | Sentry | Error tracking across frontend and backend |
| Analytics | PostHog or Plausible | Product metrics such as rooms created, skip rate, and playlist saves |

## Guiding Architecture

Jammix should maintain its own virtual queue as the source of truth. Spotify's playback queue should be treated as an output target, not the main state store.

The app should:

- Store the planned queue in Jammix.
- Recalculate the queue when users join, vote, add songs, or the host changes the prompt.
- Preserve protected manual additions.
- Push only the next few songs into the host's Spotify queue.
- Save the final Jammix session as a Spotify playlist.

This approach avoids depending on Spotify's queue as a full queue-management system and gives Jammix control over explanations, voting, ordering, and real-time changes.

## Phase 0: Product and Technical Setup

Goal: Prepare the project so implementation can move quickly without reworking foundations.

Deliverables:

- Confirm MVP scope and non-goals.
- Create Spotify Developer app.
- Configure Spotify OAuth redirect URLs for local and production environments.
- Decide initial hosting provider.
- Create `.env.example` with required environment variables.
- Create initial database schema draft.
- Add project issue labels or GitHub Projects board.

Recommended environment variables:

```txt
SPOTIFY_CLIENT_ID=
SPOTIFY_CLIENT_SECRET=
SPOTIFY_REDIRECT_URI=
DATABASE_URL=
REDIS_URL=
SESSION_SECRET=
APP_BASE_URL=
```

Success criteria:

- Spotify OAuth app exists.
- Repo has a clear setup path.
- Technical decisions are documented.

## Phase 1: Web App Foundation

Goal: Create the first working Jammix web experience.

Deliverables:

- Scaffold Next.js app with TypeScript.
- Add Tailwind CSS and shadcn/ui.
- Create core routes:
  - `/`
  - `/login`
  - `/rooms/new`
  - `/rooms/[code]`
  - `/rooms/[code]/host`
- Add responsive layout for laptop host and mobile guest screens.
- Add placeholder UI for current song, queue, participants, prompt, and skip controls.

Success criteria:

- App runs locally.
- Host and guest views are visually understandable.
- Layout works on desktop and mobile.

## Phase 2: Spotify Authentication

Goal: Let every participant sign in with Spotify.

Deliverables:

- Implement Spotify OAuth Authorization Code with PKCE.
- Request only the scopes needed for the MVP.
- Store access and refresh tokens securely server-side.
- Add token refresh flow.
- Fetch and store Spotify profile data:
  - Spotify user ID
  - Display name
  - Profile image
  - Spotify profile URL
- Add logout/session clearing.

Likely Spotify scopes:

```txt
user-read-private
user-top-read
user-read-recently-played
user-library-read
user-read-playback-state
user-read-currently-playing
user-modify-playback-state
playlist-modify-private
playlist-modify-public
```

Success criteria:

- Users can sign in with Spotify.
- App can identify each user by Spotify profile.
- Expired Spotify access tokens refresh correctly.

## Phase 3: Room Creation and Joining

Goal: Build the social room layer.

Deliverables:

- Create room model with:
  - Room code
  - Host user
  - Prompt
  - Active status
  - Created time
- Generate short room codes.
- Generate invite links.
- Add QR code display for hosts.
- Allow guests to join from room code, link, or QR code.
- Track participant join and leave events.
- Add host-only permissions for prompt changes.

Success criteria:

- Any Spotify-authenticated user can create a room.
- Guests can join from another device.
- Host can see participants appear in the room.

## Phase 4: Realtime Infrastructure

Goal: Make rooms feel live.

Deliverables:

- Add Socket.IO server.
- Add room channels.
- Broadcast events:
  - `participant.joined`
  - `participant.left`
  - `queue.updated`
  - `prompt.updated`
  - `vote.updated`
  - `track.added`
  - `track.skipped`
- Add Redis adapter if multiple backend instances are planned.
- Add reconnect handling for mobile browsers.

Success criteria:

- Multiple devices in the same room update without refresh.
- Participant list, queue, and votes stay synchronized.
- Room state recovers after a temporary disconnect.

## Phase 5: Spotify Taste Ingestion

Goal: Collect enough listening data to generate a strong group queue.

Deliverables:

- Fetch each user's top tracks.
- Fetch each user's top artists.
- Fetch each user's recently played tracks.
- Fetch each user's saved tracks.
- Optionally fetch user playlists later if rate limits allow.
- Normalize track and artist data into Jammix tables.
- Exclude podcasts and episodes from automatic recommendations.
- Cache Spotify API responses to reduce repeated calls.

Recommended ingestion approach:

- Run ingestion in a background job after a user joins.
- Store raw Spotify IDs and normalized metadata.
- Track ingestion freshness by user and data source.
- Avoid repeatedly fetching the same user data during one session.

Success criteria:

- Jammix can build a taste profile for each participant.
- Room queue can be generated from more than one user's listening data.
- Spotify API rate usage remains controlled.

## Phase 6: Queue Engine MVP

Goal: Generate the first useful Jammix queue.

Deliverables:

- Build scoring model using:
  - Shared track matches
  - Shared artist matches
  - Saved-library overlap
  - Recent listening
  - Top-track ranking
  - Top-artist ranking
  - Prompt fit
  - Manual-add protection
  - Skip penalties
- Prefer songs liked by everyone, then songs liked by the most users.
- Generate explanation metadata for each selected song.
- Preserve currently playing track.
- Preserve manually added tracks.
- Recalculate algorithmic queue items when new users join.

Success criteria:

- Queue updates when a new user joins.
- Shared favorites rise to the top.
- Manual additions are not removed by recalculation.
- Each queue item has a basic "why this song" explanation.

## Phase 7: Host Prompt System

Goal: Let the host guide the session without over-constraining it.

Deliverables:

- Add host prompt input and update flow.
- Parse simple prompt signals:
  - Genre
  - Era
  - Energy
  - Exclusions
  - Mood
- Apply prompt as a scoring boost or penalty, not a strict filter.
- Display current prompt to all participants.
- Keep prompt changes host-only.

Success criteria:

- Host can enter prompts like "party pop" or "no country."
- Queue changes in a noticeable but not chaotic way.
- High-confidence group favorites can still appear when appropriate.

## Phase 8: Voting and Manual Adds

Goal: Add democratic control without letting the queue become chaotic.

Deliverables:

- Add majority skip vote.
- Track one skip vote per active participant.
- Trigger Spotify skip when majority is reached.
- Allow users to search Spotify tracks.
- Allow users to manually add a track.
- Mark manual additions as protected.
- Prevent obvious duplicate clutter.

Success criteria:

- Majority skip works across multiple devices.
- Users can manually add songs.
- Manual songs survive queue recalculation.

## Phase 9: Spotify Playback Integration

Goal: Connect the Jammix virtual queue to the host's Spotify playback.

Deliverables:

- Fetch host's available Spotify devices.
- Show active playback/device status.
- Add selected tracks to the host's Spotify queue.
- Push only the next few songs just in time.
- Handle Premium-only playback errors clearly.
- Handle Spotify `429` rate-limit responses with retry/backoff.
- Handle restricted or unavailable devices.

Success criteria:

- Host can connect an active Spotify device.
- Jammix can add upcoming songs to the host's Spotify queue.
- Playback errors are understandable to the host.

## Phase 10: Save as Playlist

Goal: Let users keep the session after it ends.

Deliverables:

- Create Spotify playlist from final Jammix queue.
- Add tracks in session order.
- Let host save the playlist.
- Let guests save or follow the playlist where API behavior allows.
- Add playlist description with Jammix attribution.

Success criteria:

- A completed Jammix session can become a Spotify playlist.
- Playlist order matches the session queue.
- Users understand where the playlist was saved.

## Phase 11: MVP Polish

Goal: Make the app feel credible enough for demos and early users.

Deliverables:

- Add loading, empty, and error states.
- Add mobile-first guest polish.
- Add host dashboard polish.
- Add privacy/onboarding copy before Spotify login.
- Add basic accessibility checks.
- Add analytics events.
- Add Sentry error tracking.
- Add README setup instructions once code exists.

Success criteria:

- A small group can use Jammix without developer help.
- The app feels understandable to a first-time user.
- Key product metrics are tracked.

## Phase 12: Closed Beta

Goal: Test Jammix in real social settings.

Deliverables:

- Run tests with dorm rooms, parties, and friend groups.
- Collect qualitative feedback.
- Track:
  - Rooms created
  - Average participants per room
  - Average session length
  - Skip rate
  - Manual adds per session
  - Playlist save rate
  - Prompt usage rate
- Adjust scoring based on real behavior.

Success criteria:

- Users prefer Jammix over a static playlist or open shared queue.
- Skip rate decreases after early-session calibration.
- Users save playlists after sessions.

## Phase 13: Spotify Pitch Readiness

Goal: Prepare Jammix for a polished demo or partnership conversation.

Deliverables:

- Product demo script.
- Updated whitepaper.
- API usage summary.
- Privacy and data flow documentation.
- Screenshots or demo video.
- Metrics from beta usage.
- Clear explanation of why Jammix complements Spotify Jam.

Success criteria:

- Jammix can be shown as a credible Spotify companion product.
- Technical limitations and API assumptions are documented.
- Product story is clear to non-technical reviewers.

## Future Enhancements

- Native iOS and Android apps with Expo.
- Session history and replay.
- Friend group memory across sessions.
- Smarter prompt parsing.
- Better song explanation cards.
- Party visualizer mode.
- More advanced fairness controls.
- Admin dashboard for beta management.
- Spotify partnership integration if native Jam APIs become available.

## Key Technical Risks

| Risk | Mitigation |
| --- | --- |
| No public Spotify Jam API | Build Jammix as a companion room and virtual queue system. |
| Spotify queue is not fully editable | Push songs just in time instead of relying on Spotify as the full queue store. |
| Spotify Premium required for playback control | Detect host account/device state and explain requirements clearly. |
| Spotify API rate limits | Cache taste data, batch calls, and use Redis-backed retry/backoff. |
| Prompt matching may be imperfect | Treat prompts as gentle scoring guidance, not strict guarantees. |
| Users may worry about listening privacy | Add clear onboarding copy and limit visible taste details. |

## MVP Definition of Done

Jammix reaches MVP when:

- A host can create a room.
- Guests can join with Spotify.
- The queue updates as guests join.
- The host can set a prompt.
- Users can vote to skip.
- Users can manually add protected songs.
- Jammix can add songs to the host's Spotify queue.
- The final mix can be saved as a Spotify playlist.
- The app works cleanly on a laptop host view and mobile guest view.
