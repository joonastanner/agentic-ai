# Dictation and token discipline

This standalone `AGENTS.md` example converts long, repetitive, conversational, or dictated requests into compact internal task contracts while preserving every requirement.

Use it independently when the full subagent-routing workflow is unnecessary. The quality-first subagent example already includes equivalent dictation rules.

## Install

Copy [`AGENTS.md`](AGENTS.md) into `~/.codex/AGENTS.md` for global behavior, or merge it into a repository-root `AGENTS.md`.

When merging, state each rule once. Repeated instructions add context and can make behavior less reliable.

## What it does

- Removes filler, false starts, abandoned phrasing, and safe verbal repetition from the internal working brief.
- Preserves explicit facts, filenames, systems, numbers, preferences, caveats, corrections, and acceptance checks.
- Treats later corrections as authoritative without discarding unrelated earlier requirements.
- Uses the compact task contract for planning and subagent briefs.
- Prevents full conversation history from being copied into independent children.

## What it does not do

This is not a pre-send speech processor. The main agent must receive the original dictation before it can interpret it, so the rule cannot remove those original input tokens. It reduces repeated context in planning and delegation.

## Reference

[OpenAI's current model guidance](https://developers.openai.com/api/docs/guides/latest-model) recommends lean prompts that retain goals, constraints, relevant context, evidence, and success criteria. OpenAI's published savings are directional internal-evaluation results, not a guarantee for this exact rule.
