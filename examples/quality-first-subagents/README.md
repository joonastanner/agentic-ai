# Quality-first Codex subagents

This example routes bounded work to GPT-5.6 Terra while GPT-5.6 Sol retains architectural judgment, integration, and consequential review.

It is designed to reduce cost and duplicated context without assuming that adding agents automatically saves tokens. Multi-agent workflows can consume more tokens when tasks are small, tightly coupled, poorly decomposed, or repeatedly briefed.

## Install

1. Merge [`config.toml`](config.toml) into `~/.codex/config.toml`. Do not overwrite unrelated settings.
2. Copy [`AGENTS.md`](AGENTS.md) into `~/.codex/AGENTS.md` for global behavior, or merge it into a repository-root `AGENTS.md` for shared project behavior.
3. Restart Codex after changing `config.toml`, then test in a new task.
4. Confirm that the account and Codex build expose the configured models and agent settings. Do not silently substitute models.

## Routing strategy

- **Terra/medium:** exploration, documentation, focused log analysis, mechanical edits, and precisely specified work.
- **Terra/high:** bounded implementation, understood bug fixes, stable-interface refactors, and costly tool-heavy work.
- **Sol/high:** architecture, decomposition, ambiguous diagnosis, integration decisions, and consequential final review.
- **Sol/xhigh:** authentication, concurrency, migrations, data-loss hazards, or a hard unresolved failure.
- **Max:** exceptional measured need only.

Luna is deliberately excluded from the default. Add it only after representative evaluations show that it preserves quality for a narrower mechanical or high-volume workload.

## Where savings should come from

### Input tokens

- Independent children use `fork_turns="none"` instead of receiving the full conversation.
- Children receive compact task contracts rather than raw dictated or repetitive requests.
- Briefs point to canonical files instead of pasting source, logs, diffs, or global instructions.
- One child is reused for follow-up work in the same subsystem.

These rules reduce duplicated subagent input. They cannot remove tokens from the original message after the main agent has received it.

### Output tokens

- Children return a fixed, terse result contract.
- Large logs remain in workspace files.
- Higher reasoning effort is reserved for work where it produces a measured gain.

### Cost

Terra has a lower per-token price than Sol. Model routing therefore saves cost more reliably than it saves raw token count.

## Quality gates

- Sol retains architecture, integration, and consequential review.
- Children receive bounded ownership and explicit acceptance checks.
- The main agent reviews every diff and runs integration or regression checks.
- Children never commit.
- Tiny, tightly coupled, shifting, and security-sensitive tasks remain with the main agent.

## Evaluate before standardizing

Run the old and routed setups against the same 10–20 representative tasks. Compare:

- acceptance-test pass rate;
- regressions and review findings;
- human rework;
- input, cached-input, reasoning, and output tokens;
- total cost;
- wall-clock time.

Keep the setup only if quality stays equal or improves. `AGENTS.md` is behavioral guidance rather than a deterministic external scheduler, so verify the effective child model and reasoning effort when metadata is available.

## References

- [OpenAI GPT-5.6 model and prompting guidance](https://developers.openai.com/api/docs/guides/latest-model)
- [OpenAI model catalog](https://developers.openai.com/api/docs/models)

OpenAI reports directional token and cost improvements from leaner prompts in internal evaluations, but those figures are not a guaranteed savings estimate for this workflow. Validate on your own workload.
