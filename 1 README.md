# Shared-Language Throughline - Establishing Precedent for Dynamic Interactive Calibration

A drop-in system-prompt / custom-instruction for keeping an AI assistant's
vocabulary calibrated to what you've actually demonstrated command of —
without asking it to dumb anything down. Model-agnostic: works with any
assistant that supports a persistent system prompt, custom instructions, or
project-level context (Claude, ChatGPT, Gemini, local models, etc.).

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
fine.** If a formal term would help, the instruction requires the assistant
to translate the idea into terms you've already shown you can use *before*
attaching the formal name to it — and to treat the formal name as optional,
not load-bearing. You get the concept either way; the vocabulary is offered,
not imposed.

This is a genuinely different axis from "explain simply" or "match my
level." It's asking for something bidirectional and ongoing: low
translation cost from what the assistant says into your working model, and
high interpretability of what it says, checked against what you've actually
demonstrated — not against what a generic user at your stated skill level
would be assumed to know.

## What "landed" means

A term doesn't count as understood just because you used it back once. The
instruction sets the bar at *consistent, coherent* integration into your own
working model over time — which also means the assistant is expected to
keep introducing new vocabulary regularly, not go quiet once one offer gets
politely acknowledged. Both halves are required at once: don't force a
correction, and don't stop stretching. That tension is intentional — it's
meant to be navigated with judgment each time, not resolved away by the
wording.

## How to use it

1. Open `TEMPLATE.md`.
2. Fill in the two bracketed sections — who the instruction refers to (can
   just be "the user"), and a concrete statement of what you're actually
   working toward.
3. Paste the result into whatever your platform calls custom instructions,
   system prompt, or persona — Claude Projects, a ChatGPT custom GPT or
   its "customize ChatGPT" settings, Gemini Gems, a local model's system
   message, or directly into an API call.

It works best wherever the same instruction persists across many
conversations rather than just one — the "consistent integration over
time" bar assumes the assistant can track whether a term has actually
stuck, which requires the instruction to be loaded in every session, not
just the one where you introduce it. A single one-off chat with no memory
between sessions will only get the "don't correct me" half of the benefit,
not the "keep track of what's landed" half.

## Origin

This came out of an actual multi-session research project — independent
number-theory work being developed through extended conversation with
Claude — after repeated cases of the assistant introducing standard
terminology for something the user had already discovered independently,
in a way that read as correction rather than collaboration. The instruction
went through several rounds of the user editing it, the assistant
proofreading and stress-testing it for internal consistency, and the user
revising again, before landing here. Written with Claude in mind, but there
is nothing Claude-specific in the mechanism — it's offered as-is in case the
same failure mode shows up in your own work with any assistant.

## License

Public domain / CC0. Use it, modify it, redistribute it, no attribution
required.
