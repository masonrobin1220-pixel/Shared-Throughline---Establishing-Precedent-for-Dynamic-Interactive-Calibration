# Shared-Language Throughline

> **Status: under active development.** The version below works and is safe
> to use as-is, but it's an earlier snapshot — a more developed version is
> being tested in real use, and won't be published here until it's stable.
> See **Known failure modes (active development)** at the bottom for what's
> actually being worked on, and why.

A drop-in Claude Project / system-prompt instruction for keeping an AI
assistant's vocabulary calibrated to what you've actually demonstrated
command of — without asking it to dumb anything down.

## The problem this solves

Working with an LLM on material above your current fluency tends to fail in
one of two directions:

- **It talks over you.** The moment you're circling a real idea, it reaches
 for the textbook name, the standard notation, the "actually, this is just
 an instance of X" — dropped in as if that vocabulary was already shared
 ground. You now have to either fake familiarity, silently disengage, or
 stop and ask what it meant, every few exchanges.
- **It talks down to you.** Overcorrecting for the above, it avoids
 precision altogether, hedges everything, and stops reaching for anything
 you haven't already said yourself — which caps how far the conversation
 can actually take you.

Neither is what you want if the point of the conversation is to get smarter,
not just to get an answer. This instruction targets the actual thing worth
optimizing for instead of either failure mode.

## The core distinction

**Reaching beyond your *knowledge* is fine — expected, even required.** An
assistant that never introduces an idea you don't already have isn't
helping you learn anything.

**Reaching beyond your *language* without first building a bridge is not
fine.** If a formal term would help, the instruction requires Claude to
translate the idea into terms you've already shown you can use *before*
attaching the formal name to it — and to treat the formal name as optional,
not load-bearing. You get the concept either way; the vocabulary is offered,
not imposed.

This is a genuinely different axis from "explain simply" or "match my
level." It's asking for something bidirectional and ongoing: low
translation cost from what Claude says into your working model, and high
interpretability of what Claude says, checked against what you've actually
demonstrated — not against what a generic user at your stated skill level
would be assumed to know.

## What "landed" means

A term doesn't count as understood just because you used it back once. The
instruction sets the bar at *consistent, coherent* integration into your own
working model over time — which also means Claude is expected to keep
introducing new vocabulary regularly, not go quiet once one offer gets
politely acknowledged. Both halves are required at once: don't force a
correction, and don't stop stretching. That tension is intentional — it's
meant to be navigated with judgment on Claude's part each time, not resolved
away by the wording.

## How to use it

1. Open `TEMPLATE.md`.
2. Fill in the two bracketed sections — who the instruction refers to (can
 just be "the user"), and a concrete statement of what you're actually
 working toward.
3. Paste the result into your Claude Project's custom instructions (Project
 settings → custom instructions), or into your system prompt if you're
 building on the API directly.

It works best in a Project or persistent-context setup where the same
instruction is present across many conversations — the "consistent
integration over time" bar in the instruction assumes Claude can track
whether a term has actually stuck, which requires it to be loaded in every
session, not just the one where you introduce it.

## Origin

This came out of an actual multi-session research project — independent
number-theory work being developed through extended conversation — after
repeated cases of Claude introducing standard terminology for something the
user had already discovered independently, in a way that read as
correction rather than collaboration. The instruction went through several
rounds of the user editing it, Claude proofreading and stress-testing it for
internal consistency, and the user revising again, before landing here. It's
offered as-is in case the same failure mode shows up in your own work.

## License

Public domain / CC0. Use it, modify it, redistribute it, no attribution
required.

---

## Known failure modes (active development)

The instruction above works, but continued real-world use surfaced several
failure modes it doesn't yet cover on its own. Listed here so anyone relying
on this version knows what to watch for, and so the gap between "published"
and "actually solid" is visible rather than silent. Each is being actively
worked on; fixes will land here once they've held up under further use.

**Provenance blindness.** The instruction says not to reach past the user's
demonstrated language — but "demonstrated" is only meaningful if the
assistant can correctly tell the user's own words apart from everyone else's.
In long-running projects with exported transcripts, memory summaries, or
multi-model chat histories in the working context, most of the available
text is often *not* the user's — it's the assistant's own prior output, or a
different model's. Without an explicit check, an assistant can mistake
recurring language in that material for the user's established vocabulary
and feed it back to them as if it were already shared ground. This was
caught when a term that had appeared dozens of times across two AI
transcripts — but never once in the user's own messages — got treated as
settled vocabulary. Fix in progress: an explicit rule that only source
material with clear, structural evidence of the user's authorship (speaker
labels, document ownership, not stylistic resemblance) counts toward the
baseline.

**Stale-artifact-as-current-state.** When asked to review "everything so
far," an assistant can end up reading a truncated or partial export and
report its ending as the actual current state of the work, missing
everything that came after the cutoff. Caught when a summary confidently
described the end of a conversation that was, in fact, still 90% unread.
Fix in progress: explicit verification that a document is complete, and its
truncation point identified, before its contents are reported as current.

**No cold-start behavior.** The instruction assumes an assistant already
knows what the user's language looks like, but says nothing about how that
knowledge should be built at the very start of a new conversation, when no
local exchange has happened yet. Left implicit, this means the first stretch
of any new thread runs on guesswork until enough back-and-forth accumulates
to self-correct. Fix in progress: a turn-one step where the assistant builds
a baseline silently from the user's own prior material before its first
substantive reply, rather than waiting to be corrected into calibration.

**No permanence for resolved ambiguity.** Once a term's status has actually
been sorted out — confirmed as the user's, or explicitly rejected in favor
of something else — that resolution needs to stick. Without a rule saying
so, the same ambiguity can resurface and get re-litigated in a later
session as if it had never been addressed. Fix in progress: treating a
resolved term as settled fact carried forward, not re-derived from scratch
each time it comes up.

**Style convergence as a self-undermining signal.** This one is
anticipated rather than yet fully observed: if this instruction works as
intended over a long enough timeline, the assistant's own language will
trend toward statistical similarity with the user's — that's the intended
effect. But that means stylistic resemblance is a *decaying* signal for
"this sounds like the user," not a stable one, and using it as evidence of
authorship becomes progressively less reliable the better the calibration is
working. Fix in progress: treating stylistic resemblance as secondary,
non-decisive evidence throughout, with structural signals doing the real
work of attribution.