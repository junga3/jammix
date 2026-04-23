# Jammix Whitepaper

Product name: Jammix

Prepared as an investor-ready product and technology whitepaper for a Spotify companion app.

As of April 23, 2026, this document assumes Jammix is a third-party companion app for Spotify users. It does not assume access to a private Spotify Jam API. It does assume use of the public Spotify Web API where available.

## 1. Executive Summary

Jammix is a social music companion app that creates a living Spotify queue for groups. Instead of one person controlling the music, Jammix builds a shared queue from the listening habits of everyone in the room. As more people join, the queue evolves in real time, balancing the tastes of the group with the host's optional vibe prompt.

The initial use cases are parties, dorm rooms, and friend groups: social settings where music matters, but managing the queue often becomes awkward, repetitive, or dominated by one person's taste. Jammix solves this by turning a group of Spotify profiles into a dynamic mix that favors songs the room is most likely to enjoy.

The product is intentionally simple at the surface. Anyone can create a Jammix room, receive a room code, and invite others through a QR code, link, room code, or another user's invite. Every participant signs in with Spotify, allowing Jammix to read their music taste signals and contribute to the session. The host can add a prompt such as "90s hip-hop," "chill dinner," "party pop," or "no country." Without a prompt, Jammix defaults to the combined top songs, artists, saved music, playlists, and recent listening behavior of the group.

Jammix is not positioned as a replacement for Spotify. It is a companion layer that makes group listening more intelligent, fair, and fun. The product can use Spotify's public Web API to read user taste data, create playlists, save the final mix, and add tracks to a user's playback queue for Spotify Premium users. Current public documentation does not show a dedicated Spotify Jam API, so the near-term product should operate as an external session layer that controls the host's Spotify playback queue rather than directly creating or modifying native Spotify Jam sessions.

## 2. Brand Direction

The product name is Jammix.

Jammix combines the language of a live music jam with the utility of an adaptive mix. The name is short, social, and flexible enough to work as both a product name and a user action: create a Jammix, join a Jammix, save a Jammix.

The brand should feel adjacent to Spotify's social listening language without implying official Spotify ownership, endorsement, or co-branding. Spotify's developer branding guidance says app names should not include "Spotify," should not sound confusingly similar to Spotify, and should not imply endorsement. Jammix avoids using Spotify's name directly while still communicating the product's relationship to group listening.

The public descriptor can be: "Jammix, a social queue companion for Spotify." This communicates compatibility without implying official sponsorship. In public materials, Jammix should be presented as an independent companion app, not as an official Spotify product.

## 3. Problem

Group music has a social friction problem. In parties, dorms, and friend groups, the music usually falls into one of three patterns:

1. One person controls the speaker and plays mostly their own taste.
2. Everyone manually adds songs, creating a chaotic queue with clashing moods.
3. The group settles for a static playlist that does not reflect who is actually there.

Spotify Jam helps users listen together, but a group still needs a better decision layer for what should play next. The issue is not only access to a queue. The real problem is group taste negotiation.

Jammix addresses that gap by automatically answering a simple question: "What should this room hear next?"

## 4. Product Vision

Jammix turns a collection of Spotify profiles into a live, adaptive group mix.

The product vision is to make group music feel effortless, personal, and shared. A good Jammix session should feel like the room has a great DJ who knows everyone, reads the moment, and avoids letting any one person take over.

The core promise:

Jammix creates better group music by finding the overlap between everyone's taste, then continuously adapting as people join, vote, and add songs.

## 5. Target Audience

The first target users are Spotify listeners in casual social environments:

- Dorm rooms
- House parties
- Pregames
- Friend hangouts
- Small gatherings
- Shared apartments
- Study breaks
- Group game nights

These settings are ideal because the cost of a bad queue is social, not technical. The value of Jammix is felt immediately when the room no longer has to argue over the next song.

## 6. Core User Flow

### Host Flow

1. The host opens Jammix on a laptop, tablet, or phone.
2. The host signs in with Spotify.
3. The host creates a new Jammix room.
4. Jammix generates a room code, QR code, and invite link.
5. The host optionally enters a prompt, such as "high energy pop," "2010s rap," or "chill but not sleepy."
6. Jammix begins generating a queue from the host's listening data.
7. As users join, Jammix updates the queue in real time.
8. The host can see the room, current song, upcoming queue, votes, prompt, and group taste breakdown.
9. At the end, the host and participants can save the final mix as a Spotify playlist.

### Guest Flow

1. A guest joins through QR code, invite link, room code, or another participant's invite.
2. The guest signs in with Spotify.
3. Jammix imports the guest's profile image, display name, and taste signals.
4. The room queue immediately adapts to include the guest's taste.
5. The guest can vote to skip, view the current song, see upcoming songs, and manually add protected songs.
6. The guest can save the final mix as a playlist when the session ends.

## 7. Feature Set

### Room Creation

Any authenticated Spotify user can create a Jammix room. Each room generates:

- A short room code
- A QR code
- A shareable invite link
- A join flow that allows guests to invite more guests

### Spotify Login

All participants must authenticate with Spotify. This is required because Jammix depends on each user's listening data and profile metadata.

### Live Queue Generation

Jammix generates and updates a queue in real time as participants join. The queue is not static. It recalculates based on:

- Recently played tracks
- Top tracks
- Top artists
- Saved songs
- User playlists, where permission is granted
- Manual song additions
- Skip votes
- Host prompt

Spotify podcasts and episodes should be excluded from automated queue generation. The product should focus on songs by default.

### Host Prompt

The host can add or update a vibe prompt. Prompts act as gentle guidance, not strict filters. This means the prompt nudges the recommendation engine toward a sound, genre, era, energy level, or exclusion, while still allowing high-confidence group favorites to appear.

Example prompts:

- "90s hip-hop"
- "party pop"
- "chill dinner"
- "high energy only"
- "no country"
- "late-night R&B"
- "songs everyone knows"

Only the host can set or change the prompt.

### Manual Additions

Participants can manually add songs. Manually added songs are protected: they are not replaced when new users join or when the algorithm recalculates the queue. Jammix should still prevent duplicate clutter, but manual additions should have clear priority because they represent active user intent.

### Majority Skip

Users can vote to skip the current song. A skip happens when a majority of active participants vote to skip. This preserves democratic control without allowing one person to dominate the session.

### Song Explanation

Jammix should explain why a song was selected. For example:

"Added because Maya, Chris, and Jordan all listen to SZA, and Maya is the top listener for this track."

The explanation layer should show:

- Which users like the song or artist
- The top listener for the song or artist
- Whether the song fits the host prompt
- Whether it is a shared favorite

### Save the Mix

At the end of a session, users can save the generated mix as a Spotify playlist. The playlist should preserve the session's final track order and include a description such as:

"Created with Jammix from a live group session."

## 8. Recommendation and Queue Logic

Jammix should prioritize consensus first, then popularity within the group, then personal representation.

The ranking system can use a group affinity score:

Group Affinity Score =

- Shared track match
- Shared artist match
- Saved-library overlap
- Recent listening frequency
- Top-track ranking
- Top-artist ranking
- Playlist recurrence
- Prompt fit
- Manual boost
- Anti-repeat penalty
- Skip penalty

### Tier 1: Everyone-Likes-It Songs

Songs or artists that appear across many users' taste profiles should receive the highest priority. These tracks create the strongest sense that the music belongs to the whole room.

### Tier 2: Most-Users-Like-It Songs

When there are few universal matches, Jammix should choose songs liked by the largest subgroup. This keeps the queue broadly appealing while avoiding generic recommendations.

### Tier 3: Discovery and Representation

Jammix can occasionally introduce songs that strongly represent a smaller subset of users, especially if they fit the prompt. This prevents the queue from becoming too predictable and gives every participant some presence in the mix.

### Manual Override Layer

Manual additions should be treated as protected queue items. They can be skipped by majority vote, but should not be removed simply because the room changes.

### Real-Time Recalculation

When a new user joins:

1. Fetch and normalize their listening signals.
2. Recalculate group affinity scores.
3. Preserve currently playing and protected manual songs.
4. Reorder only the algorithmic portion of the upcoming queue.
5. Add new high-confidence songs immediately.
6. Update explanation metadata for visible songs.

This creates the feeling that the queue is alive without making it unstable or confusing.

## 9. Product Experience

The host interface should be optimized for laptops because the host is likely managing the music in a room. The guest interface should be mobile-first because most guests will join from their phones.

### Host Dashboard

The host dashboard should include:

- Current song
- Album art and Spotify attribution
- Room code
- QR code
- Invite link
- Active participants
- Host prompt
- Upcoming queue
- Manual additions
- Skip vote status
- Group taste summary
- Top shared artists
- Save playlist action
- Playback/device status

### Guest View

The guest view should include:

- Current song
- Upcoming queue preview
- Vote to skip
- Manual add song
- Room participants
- Song explanation
- Save mix action

### Identity

Users should appear with their Spotify profile picture and display name. Jammix should not expose full private listening history by default. It should only reveal specific taste signals when they explain a selected song, such as "top listener" or "also likes this artist."

## 10. Technical Architecture

Jammix should be built as a real-time web application with mobile-responsive guest views and an expanded host dashboard.

### Suggested Architecture

- Frontend: React, Next.js, or similar web framework
- Mobile: Responsive web first, native apps later if needed
- Backend: Node.js or Python API service
- Auth: Spotify OAuth Authorization Code with PKCE
- Realtime: WebSockets or server-sent events
- Database: PostgreSQL for sessions, participants, votes, queue state
- Cache/Queue: Redis for real-time session state and rate-limit protection
- Deployment: Vercel/Netlify for frontend, Render/Fly.io/AWS for backend

### Main Services

- Auth Service: Handles Spotify login, token refresh, and user scopes.
- Room Service: Creates rooms, room codes, invite links, QR codes, and participant state.
- Taste Ingestion Service: Fetches listening signals from Spotify.
- Queue Engine: Scores, ranks, and updates songs.
- Prompt Parser: Converts host prompts into scoring adjustments.
- Playback Service: Adds tracks to the host's Spotify queue where API access allows.
- Playlist Export Service: Creates the final playlist and adds tracks.
- Explanation Service: Generates user-facing reasons for each selected song.

## 11. Spotify API Feasibility

Based on current public Spotify developer documentation, the product is feasible as a companion app with important constraints.

### Feasible With Public Web API

Jammix can:

- Read a user's top artists and tracks with `GET /me/top/{type}` using `user-top-read`.
- Read recently played tracks with `GET /me/player/recently-played` using `user-read-recently-played`.
- Read saved tracks with `GET /me/tracks` using `user-library-read`.
- Read the current user's profile with `GET /me`, including display name and images.
- Read the user's current playback queue with `GET /me/player/queue`.
- Add an item to a user's playback queue with `POST /me/player/queue` using `user-modify-playback-state`.
- Create playlists with `POST /me/playlists`.
- Add tracks to playlists with `POST /playlists/{playlist_id}/tracks`.

### Key Constraints

- Queue modification through the Spotify Web API works only for Spotify Premium users.
- The public Web API supports adding items to a user's playback queue, but command ordering is not guaranteed when used with other Player API endpoints.
- Current public documentation does not expose a dedicated Spotify Jam API for creating or managing native Spotify Jam sessions.
- New Spotify apps begin in development mode, which limits access and requires allowlisting. Wider public release requires extended quota mode and Spotify review.
- Spotify's rate limits are calculated over a rolling 30-second window, so Jammix should cache user taste data, batch requests, and use backoff when receiving `429` responses.
- Spotify developer policy and branding rules must be followed carefully, especially around app naming, attribution, commercial use, and content usage.

### Recommended MVP Interpretation

The MVP should not attempt to be a native Spotify Jam controller. Instead, it should create a Jammix room and control the host's Spotify playback queue. Users join Jammix, not necessarily Spotify Jam. The host plays Spotify on an active device, while Jammix continuously adds the best next tracks to the host queue.

This approach keeps the product technically realistic while preserving the core user experience: a shared, evolving group mix.

## 12. Privacy and Data Model

Jammix requires users to connect Spotify and contribute listening data. Because this is central to the product, privacy should be handled with clarity rather than hidden behind vague settings.

User-facing principle:

"When you join a Jammix, your Spotify taste helps shape the queue."

Jammix should collect only what it needs:

- Spotify user ID
- Display name
- Profile image
- Top artists
- Top tracks
- Recently played tracks
- Saved tracks
- Selected playlist metadata and tracks, if used
- Session votes and manual additions

Jammix should not publicly display a user's full listening history. The product can use data behind the scenes and reveal limited explanation details only when relevant to the current queue.

Because the user requirement is that participants cannot hide once they join, the onboarding flow should make this explicit before OAuth:

"Joining means your Spotify taste will be used to shape this room's music. Other users may see when you are a top listener for a song or artist."

Session data should be temporary by default. Saved playlists can persist in Spotify, but Jammix should not keep raw listening histories longer than needed unless the user creates an account-level history feature later.

## 13. Compliance and Brand Considerations

Jammix should present itself as a companion app for Spotify, not an official Spotify product. It should:

- Avoid using "Spotify" in the app name.
- Avoid names that sound confusingly similar to Spotify.
- Avoid using Spotify logos or brand elements as part of the Jammix logo.
- Attribute Spotify content properly wherever Spotify metadata, album art, track names, or artist names appear.
- Link Spotify metadata back to Spotify where required.
- Avoid training any AI or machine learning model on Spotify content or platform data.
- Avoid commercial streaming functionality without the required approvals.

The investor story should therefore frame Jammix as a social queue intelligence layer and potential Spotify partnership opportunity, not as an unofficial clone of a Spotify-owned feature.

## 14. MVP Scope

The first version should focus on proving the core loop:

1. Host creates room.
2. Users join with Spotify.
3. Jammix reads taste data.
4. Queue updates in real time.
5. Host prompt influences song choices.
6. Majority skip works.
7. Manual additions are protected.
8. Final mix can be saved as a Spotify playlist.

### MVP Features

- Spotify login
- Room code and QR join
- Host dashboard
- Guest mobile view
- Top tracks/artists ingestion
- Recently played ingestion
- Saved tracks ingestion
- Podcast exclusion
- Real-time participant updates
- Queue ranking engine
- Host prompt
- Manual song add
- Majority skip voting
- Song explanation
- Save as playlist

### Post-MVP Features

- Native mobile apps
- Better prompt understanding
- Session history
- Replay previous Jammix sessions
- Smart warmup and cooldown phases
- Party mode visualizer
- Friend group memory
- Collaborative playlist export
- Spotify partnership packaging

## 15. Roadmap

### Phase 1: Prototype

Build a working web app that lets a host create a room, authenticate with Spotify, and generate a queue from one or more authenticated users.

Success criteria:

- At least three users can join a room.
- The queue changes when a new user joins.
- The host can save the final track list as a playlist.

### Phase 2: MVP

Add real-time updates, majority skip voting, protected manual adds, and prompt-based queue guidance.

Success criteria:

- A group can run a full session without manual developer intervention.
- The queue feels responsive but stable.
- Users understand why songs are chosen.

### Phase 3: Closed Beta

Test in dorms, parties, and friend groups. Measure whether users prefer Jammix over a normal shared queue or static playlist.

Success criteria:

- High session completion rate
- Multiple users per room
- High save-playlist conversion
- Low skip rate after first 10 minutes
- Positive qualitative feedback around fairness and music quality

### Phase 4: Spotify Partnership Readiness

Prepare a polished product demo, compliance documentation, privacy policy, data-flow diagram, API usage summary, and partnership pitch.

Success criteria:

- Clear technical feasibility
- Clear user demand
- Responsible data handling
- Demonstrated alignment with Spotify listening behavior

## 16. Success Metrics

Core product metrics:

- Rooms created
- Average participants per room
- Average session duration
- Songs played per session
- Skip rate
- Manual adds per session
- Prompt usage rate
- Playlist save rate
- Return host rate
- Return participant rate

Quality metrics:

- Percentage of songs liked by multiple users
- Percentage of songs matching prompt
- Time from new user joining to queue update
- User rating of queue quality
- Number of "why this song" interactions

Social metrics:

- Invite link shares
- QR joins
- Participants invited by other participants
- Saved playlists shared after session

## 17. Investment Thesis

Jammix sits at the intersection of three strong behaviors:

1. Spotify users already have deep personal taste profiles.
2. Group listening is common but still awkward to coordinate.
3. Social music experiences work best when they feel personal, live, and easy.

The opportunity is not to build another playlist generator. The opportunity is to own the group decision layer for music.

In the short term, Jammix is a useful companion app for parties and friend groups. In the long term, it could become a social listening layer that integrates directly into larger music platforms, event tools, college communities, or Spotify itself.

The strongest strategic exit or partnership path is Spotify. Jammix extends the value of Spotify Jam by solving the next-song problem. It gives Spotify a way to make group listening more personalized without forcing users to manually build queues or negotiate taste in the moment.

## 18. Risks and Mitigations

### API Access Risk

Risk: Spotify API access, quota rules, or playback-control permissions may limit scale.

Mitigation: Build within documented Web API behavior, cache aggressively, avoid unnecessary calls, and prepare for Spotify review early.

### Native Jam Integration Risk

Risk: There is no public API for native Spotify Jam session control.

Mitigation: Treat Jammix as a companion queue controller in the MVP. Position native Jam integration as a partnership opportunity.

### Privacy Risk

Risk: Users may be uncomfortable exposing listening data in a group setting.

Mitigation: Use clear onboarding language, reveal only limited explanation data, and avoid displaying full listening history.

### Queue Quality Risk

Risk: The queue may become too generic if it only optimizes for overlap.

Mitigation: Balance consensus tracks with occasional discovery and representation picks.

### Brand Risk

Risk: The product could appear unofficially co-branded with Spotify.

Mitigation: Use an independent name and identity, include "for Spotify" only as a compatibility descriptor, and follow Spotify branding guidance.

## 19. Conclusion

Jammix makes group music feel like it belongs to the room.

By combining Spotify listening data, real-time room participation, host prompts, democratic skip voting, manual additions, and playlist export, Jammix creates a shared music experience that is smarter than a static playlist and less chaotic than an open queue.

The MVP is technically achievable as a Spotify companion app using public Web API capabilities, with clear constraints around Premium playback control, public release approval, branding, and lack of native Jam API access. Those constraints do not weaken the concept. They sharpen the initial strategy: prove that Jammix can make group music better, then use that proof to approach Spotify or publish as a compliant companion product.

Jammix's long-term value is simple: it helps Spotify users stop arguing over the aux and start hearing the room.

## Sources

- Spotify Web API: Add Item to Playback Queue: https://developer.spotify.com/documentation/web-api/reference/add-to-queue
- Spotify Web API: Get User's Queue: https://developer.spotify.com/documentation/web-api/reference/get-queue
- Spotify Web API: Get Recently Played Tracks: https://developer.spotify.com/documentation/web-api/reference/get-recently-played
- Spotify Web API: Get User's Top Items: https://developer.spotify.com/documentation/web-api/reference/get-users-top-artists-and-tracks
- Spotify Web API: Get User's Saved Tracks: https://developer.spotify.com/documentation/web-api/reference/get-users-saved-tracks
- Spotify Web API: Get Current User's Profile: https://developer.spotify.com/documentation/web-api/reference/get-current-users-profile
- Spotify Web API: Create Playlist: https://developer.spotify.com/documentation/web-api/reference/create-playlist
- Spotify Web API: Add Items to Playlist: https://developer.spotify.com/documentation/web-api/reference/add-tracks-to-playlist
- Spotify Web API Rate Limits: https://developer.spotify.com/documentation/web-api/concepts/rate-limits
- Spotify Web API Quota Modes: https://developer.spotify.com/documentation/web-api/concepts/quota-modes
- Spotify Developer Policy: https://developer.spotify.com/policy
- Spotify Design and Branding Guidelines: https://developer.spotify.com/documentation/design
