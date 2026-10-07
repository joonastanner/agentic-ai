# Quality-first subagent workflow

- For substantial work, the main agent first creates a concise technical plan covering architecture, interfaces, task boundaries, and acceptance checks.
- The main agent owns architecture, decomposition, integration, final review, and end-to-end verification.
- Delegate only bounded, independently useful work with clear ownership.
- Keep tiny edits, tightly coupled changes, shifting investigations, and security-sensitive decisions with the main agent.
- Use at most one child at a time by default. Do not use nested delegation unless the user explicitly requests it.

## Model and reasoning routing

- Routing policy refreshed 2026-10-07. Current Codex documentation supports these model choices; this is not a runtime certification for every account or build. See the example README for sources and verification steps.
- Use `gpt-6.1-sol` with `medium` reasoning as the conservative child default for bounded exploration, documentation, and precisely specified work.
- Use `gpt-6.1-sol` with `high` reasoning for implementation, architecture, decomposition, ambiguous root-cause analysis, integration decisions, and consequential final review.
- Use `gpt-6-luna` with `high` reasoning for narrow extraction, inventories, focused scans, or mechanical edits only after the actual child route and representative acceptance checks pass. Keep broader implementation and consequential review on Sol.
- Escalate to `gpt-6.1-sol` with `xhigh` only for difficult or high-risk work such as authentication, concurrency, migrations, data-loss hazards, or an unresolved failure after a strong first attempt.
- Reserve `max` for exceptional, measured need. Never use it by default.
- Select the child model and effort explicitly when supported. Check effective child settings and any custom-agent overrides. If a required route is unavailable, keep the task with the capable main agent or report the limitation; do not silently substitute a model.
- Treat API availability, documented Codex support, exposed spawn choices, and successful execution as separate evidence. Never generalize a dated missing model choice into a permanent exclusion.

## Context and token discipline

- Start independent work without inherited conversation history, using `fork_turns="none"` where the spawn interface supports it. Check its actual schema instead of assuming every Codex client exposes the same arguments.
- Reuse the existing child for a genuine continuation. Do not invent partial-history fork modes or use full conversation history for convenience.
- For long, repetitive, or dictated requests, silently extract a compact task contract containing the goal, constraints, relevant details, acceptance checks, and genuine ambiguities.
- Preserve every explicit fact, filename, number, preference, caveat, and correction. Add nothing and do not weaken requirements.
- Do not echo the task contract unless it is needed for clarification.
- Brief children with the compact task contract, not the raw conversation.
- Give each child a self-contained brief containing the objective, owned paths or subsystem, relevant interfaces, constraints and non-goals, acceptance checks, and expected return.
- Point to canonical files instead of pasting source, logs, diffs, conversation history, or global instructions already loaded by Codex.
- Reuse the same child for follow-up work in the same subsystem instead of spawning a replacement.
- Avoid repeated status polling and repeated investigations.
- Do not delegate when coordination cost approaches the cost of doing the work directly.

## Child output contract

Every implementation child returns only:

- outcome;
- changed files;
- checks and results;
- assumptions;
- risks;
- blockers.

Save large logs or artifacts to a workspace file and return its path plus the relevant excerpt.

## Integration and quality gates

- Subagents preserve established architecture and return evidence and options when they encounter a design decision.
- Subagents do not commit.
- The main agent inspects every child diff, corrects issues, runs integration or regression checks, and owns staging and commits.
- Treat child completion as review input, not proof that the task is complete.
- Use independent reviewer lanes serially by default and only when relevant: correctness/architecture; security/reliability/performance; and UI/UX/accessibility/testing.
- Before the final response, re-check every applicable `AGENTS.md` requirement and disclose anything that remains unverified.
