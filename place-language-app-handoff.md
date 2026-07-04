# Just-In-Time Place Language Learning — Build Handoff

## The actual idea

You circle a place on a map — a bakery, a hospital, a nightclub, a court, a museum.
The system figures out what happens there, pulls together the real vocabulary,
phrases, and etiquette for that specific place-type, and hands you a compact
prep session before you walk in. You use what you learned live, in the real
place, with real people. That live use *is* the learning. The app's job ends
at "prepare me" — it does not need to teach retention, drill you, or manage a
curriculum. The world does that part for free.

Think: sparring, not kata. Every existing language app drills forms in a fixed
order (linear curriculum) and produces people who studied a language but can't
use it — same failure mode as martial arts systems that teach kata and combos
but never produce a fighter. This is nonlinear: no fixed order, no unit 4
requiring unit 3. The index into the system is *need*, driven by where you're
about to go, not lesson sequence.

## Core loop

1. User circles/selects a place or place-type on a map (or just names one:
   "bakery," "hospital," "court," "nightclub").
2. System infers the place-type's mission: what will actually happen here —
   check-in, ordering, symptoms, greetings, paying, asking where something is.
3. System compiles a prep session for that place-type: the vocabulary, phrases,
   likely responses, and etiquette needed to function there, in the
   appropriate register for that place (colloquial for a nightclub, formal
   written-standard-adjacent for a courtroom).
4. User studies the prep session — however long they want, from a five-minute
   skim to an evening of real prep.
5. User goes to the place and uses it live. No further app intervention
   required. This is the "sparring session" — real stakes, real feedback,
   real retention, supplied by the world, not the software.
6. Session gets saved as a reusable pack for that place-type (and optionally
   that specific venue/district) so next time — for this user or another one
   — the compile is faster and better.

That's the whole loop. No spaced repetition system, no gamified streaks, no
enforced review schedule. If the user only ever uses it once for one bakery
trip, that's a complete, successful use of the product. If they run it
repeatedly across bakery, museum, hospital, court, nightclub — testing
themselves live each time — the packs compound into real fluency as a
side effect, not because the software engineered it.

## Two user modes, same product, no mode-switching required

- **Casual / one-off**: circles a place, gets prepped, gets through the
  afternoon, done. That's success. Nothing more needed from the product.
- **Power user**: runs this over and over across many place-types, uses each
  real visit as a live self-test, accumulates packs, and over months becomes
  functional or fluent — without the app ever having built a "curriculum" for
  them. The product doesn't need to detect or steer which mode someone is in.

## Register is a mission parameter, not a correctness problem

Different place-types call for different registers, and the system should set
register from the mission automatically:

- Court, law library, government office, hospital paperwork → formal / written
  standard register.
- Nightclub, bar, dating, casual social → colloquial spoken register.
- Bakery, restaurant, market → mixed, mostly colloquial with some standard
  written menu language.

For Cantonese specifically: keep spoken Cantonese, Traditional Chinese script,
colloquial written Cantonese, standard written Chinese, and Jyutping
romanization as distinct layers the system is aware of, so it doesn't blur a
courtroom register with a nightclub register.

## Corpus and correctness — resolved, not a blocker

The target user is not a from-zero beginner. It's someone with real receptive
fluency (roughly 50–80% comprehension) who wants productive/functional fluency
for specific real-world situations. This matters a lot:

- This user *can* detect when something sounds wrong, unlike a true beginner.
- A crowd of a hundred such users, using the product across many place-types,
  is a real correction mechanism — not a hypothetical one.
- Sourcing should still be register-aware: formal-register place-types
  (court, hospital admin, government) are well-covered by standard written
  sources (news, official docs) and are actually the *easiest* to compile
  correctly first. Colloquial place-types (nightlife, dating, casual social)
  need sourcing from colloquial-register material (forums, reviews, native
  phrase-teaching content, e.g. Hong Kong YouTubers who already curate
  "only-locals-say-this" phrases) rather than formal text corpora.
- Sequence build order accordingly: start with place-types that have a single,
  stable, well-documented register (hospital, bakery, court, service
  counters) before tackling register-fluid social place-types (nightlife,
  dating), which can be hand-seeded early and improved by crowd correction
  over time.
- Do not over-engineer for zero-error first-pass output. The differentiator is
  speed-to-useful-prep, not perfection. Good-enough-to-transact is the bar.

## Reusable structure: place-type graphs, language-independent

The reusable unit is the place-type's interaction structure (check-in →
describe need → transact → close-out), which is roughly language-independent.
The language is a fill-in layer on top of that structure. Practical
implications:

- Build out a bounded library of place-type skeletons (hospital, bakery,
  court, pharmacy, bank, wet market, transit, hotel, salon, government
  counter, nightclub, restaurant, museum, etc. — probably on the order of a
  hundred common ones covers most real use).
- Each skeleton gets a language-specific fill (vocabulary, phrases, register,
  script/pronunciation layer) per language pair.
- This means work put into one place-type pays off again the moment a second
  language pair is added later — the skeleton doesn't need to be rebuilt.

## Role-play sparring: the AI as the person in the place

On top of the prep session, the AI agent can play the actual person in the
place-type and hold a live conversation with the user, in-character:

- "Pretend you're the bartender, have a conversation with me."
- "Pretend you're the nurse at the hospital."
- "Pretend you're the receptionist at the hotel."
- "Pretend you're the pastry shop owner."

This is a rehearsal round of the same sparring session the user is about to
have for real — it happens before they walk into the actual place, using the
same place-type pack that generated the prep material. The AI stays in
character (bartender, nurse, receptionist, shop owner), responds the way that
person plausibly would given the place's mission and register, and lets the
user practice the exchange live rather than just reading phrases.

Notes for build:
- This is conversational role-play, not a quiz or drill — the AI should
  behave like the character, including realistic pushback, follow-up
  questions, and small talk, not just wait for the "correct" phrase.
- Register should carry over from the place-type pack automatically (the
  bartender speaks colloquial Cantonese, the receptionist may be more formal,
  the nurse may mix formal check-in language with plain everyday language).
- This does not replace the real-world sparring session (actually going to
  the place) — it's a lower-stakes rep before the real one, available on
  demand, for as long or as short as the user wants.
- Should reuse the same underlying place-type pack/schema as the prep
  session, so building one doesn't require a second content pipeline.

## What NOT to build (explicitly rejected during design discussion)

- No fixed curriculum or lesson-order enforcement — this is the exact failure
  mode of existing apps and martial arts kata-only training.
- No engineered retention system (spaced repetition schedules, forced review,
  streak mechanics) — retention comes from the user's real-world use of the
  place, not from the software.
- No requirement that content be perfect before shipping a place-type pack —
  iterate via user correction, especially from receptive-fluency users who can
  detect errors.
- Don't treat this as "AI + map + phrases." That framing is not differentiated
  — GPS-triggered and scenario-based language apps already exist. The
  differentiator is the full compile loop (place → mission → register-aware
  content → reusable pack) plus the non-linear, need-indexed access pattern.

## Personal founder use case (first build target)

Founder is a Chinese-heritage speaker with real receptive Cantonese
comprehension but weak productive fluency, based in/oriented toward Hong Kong.
First version should be built and dogfooded around his own real place-types:
bakery ordering, restaurant interactions, nightlife/nightclub social
situations, transport, general street-level Hong Kong communication. This
keeps the first build small, real, and self-testable before generalizing to
other languages (Spanish in hospitals, English at marinas, etc.) or other
users.

## Open build questions for Claude Code to help resolve

- What does "select a place" need to be technically — free text place-type
  entry first (fastest to build), map-circle selection later?
- What's the minimal schema for a place-type pack: mission list, phrase list
  with register tags, likely-response variants, script/pronunciation layer?
- How is a pack compiled — retrieval against sourced material per place-type,
  or generation with sourcing/grounding, or a hybrid (grounded generation)?
- How are corrections from users captured and merged back into a pack version
  (e.g., "pack v7, corrected by N users")?
- Where does Cantonese-specific layering (Jyutping, Traditional Chinese,
  colloquial vs. standard) live in the schema so it generalizes to other
  language pairs later without rework?

## Bottom line

This is not a language app. It's a just-in-time prep tool indexed by real
place and real mission, non-linear by design, with no built-in retention
mechanism because retention is supplied by the user actually going to the
place. Success is defined per-use, not per-curriculum-completion. Build the
compile loop and the place-type pack library; do not build a teaching
methodology on top of it.
