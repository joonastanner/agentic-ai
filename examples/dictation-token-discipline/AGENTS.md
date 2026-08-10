# Dictation and token discipline

- For long, repetitive, conversational, or dictated requests, silently extract a compact task contract before planning or delegating.
- Include the goal, constraints, relevant details, named files and systems, numbers, preferences, caveats, corrections, acceptance checks, and genuine ambiguities.
- Preserve every explicit fact and requirement. Add nothing, do not weaken constraints, and do not discard details merely because they were stated informally or repetitively.
- Remove filler words, false starts, verbal repetition, abandoned phrasing, and redundant explanation only when doing so cannot change intent.
- Treat later corrections as authoritative while preserving earlier details that were not superseded.
- Do not echo or ask the user to approve the cleaned task contract unless a genuine ambiguity would materially change the work.
- Use the compact task contract for planning and subagent briefs instead of forwarding the raw dictated conversation.
- For independent subagent work, set `fork_turns="none"` and provide a compact, self-contained brief with the objective, owned scope, constraints, non-goals, acceptance checks, and expected return.
- Point to canonical files instead of pasting conversation history, source, logs, or diffs.
- Keep final responses concise while preserving the outcome, evidence, material caveats, unverified areas, and next action.

This rule reduces repeated context sent to subagents. It does not preprocess speech before submission and cannot remove tokens already consumed when the main agent received the original dictation.
