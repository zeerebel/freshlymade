# Tech Stack — Place-Language Prep App

Based on `place-language-app-handoff.md`. First build target: solo founder,
dogfooding on Hong Kong / Cantonese place-types (bakery, restaurant,
nightlife, transport, street-level). Optimize for speed to a working
prototype, not scale.

## Guiding constraints from the handoff

- Core loop is: place/place-type input → mission inference → compiled
  "pack" (phrases, vocab, register, script/pronunciation) → prep session →
  optional role-play sparring. All generative/LLM-driven, not hand-authored
  curriculum.
- Packs are reusable, versioned, and correctable by users — needs a real
  data store, not just LLM calls on the fly.
- Cantonese needs explicit layering: spoken Cantonese, Traditional Chinese
  script, colloquial written Cantonese, standard written Chinese, Jyutping
  romanization — schema must carry these as distinct fields, not blend them.
- First version: free-text place-type entry (map-circle selection is
  explicitly "later"). Mobile-first (used standing outside/entering a
  place), but a plain responsive web app is enough for v1 dogfooding.
- No spaced repetition / curriculum engine to build — this significantly
  shrinks scope vs. a normal language app.

## Proposed stack

**Frontend**
- Next.js (React, App Router) + TypeScript — single codebase for the
  place-entry UI, prep-session view, and role-play chat UI; deploys as a
  responsive web app usable on a phone browser day one, native wrapper
  later if needed.
- Tailwind CSS for fast UI iteration.
- PWA config (manifest + service worker) so it can be added to the home
  screen and used offline-ish for a saved pack — no App Store loop needed
  for founder dogfooding.

**Backend / API**
- Next.js API routes (or a small separate Node/TypeScript service if it
  outgrows that) — handles pack compilation requests, pack storage, and
  correction submissions.
- Anthropic API (Claude) for: mission inference from place-type input,
  grounded generation of the pack content, and the role-play sparring
  character (system prompt carries the place-type's register + mission,
  conversation is a normal multi-turn chat completion).

**Data / storage**
- Postgres (e.g. Supabase or Neon — either gets you Postgres + auth +
  storage without standing up infra) as the source of truth for:
  - `place_type_skeletons` (language-independent interaction structure:
    check-in → describe need → transact → close-out, per place-type)
  - `packs` (language-specific fill per place-type + language pair,
    versioned: `pack_id`, `version`, `place_type_id`, `language_pair`)
  - `pack_items` (phrase/vocab rows with register tag, script layer,
    romanization, and which mission step they belong to)
  - `corrections` (user-submitted fixes tied to a pack version, merged
    into the next version)
- This is the piece that makes "reusable place-type graphs" and "pack v7,
  corrected by N users" real rather than aspirational — plain LLM calls
  with no storage would regenerate and lose corrections every time.

**Cantonese-specific schema layer**
- `pack_items` carries parallel fields rather than one "text" field:
  `spoken_cantonese`, `traditional_chinese`, `colloquial_written`,
  `standard_written`, `jyutping`. Other language pairs later just leave
  irrelevant layers null — same table, no schema rework.

**Auth**
- Minimal — single-user (founder) for v1. If Supabase is used, its
  built-in auth covers the "second user" case for free later without
  extra work now.

**Hosting**
- Vercel for the Next.js app (pairs naturally with Next.js, generous free
  tier, zero-ops). Supabase/Neon for the DB, also hosted/managed.

## What this deliberately avoids

- No mobile native app framework (React Native/Swift) for v1 — the core
  loop doesn't need device APIs beyond geolocation-maybe-later; a
  responsive PWA is enough to dogfood immediately.
- No map/GIS stack (Mapbox, PostGIS) yet — place-type is free text per
  the handoff's own sequencing ("free text first, map-circle later").
- No vector DB / RAG infra up front — start with direct grounded
  generation (Claude + a per-place-type prompt seeded with hand-curated
  source notes); add retrieval only once the skeleton library and
  sourcing pipeline are real enough to need it.
- No retention/spaced-repetition system, no gamification stack — explicitly
  rejected in the handoff.

## Suggested build order

1. `place_type_skeletons` + `packs`/`pack_items` schema in Postgres.
2. Free-text place-type input → Claude call producing a structured pack
   (mission + register + phrases in all script layers) → store as v1 pack.
3. Prep-session view rendering a pack.
4. Role-play chat reusing the same pack/schema as system-prompt grounding.
5. Correction submission UI + merge into next pack version.
