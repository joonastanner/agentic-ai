# Quality-first Codex subagents

This example routes bounded work to GPT-5.6 Terra while GPT-5.6 Sol retains architectural judgment, integration, and consequential review.

It is designed to reduce cost and duplicated context without assuming that adding agents automatically saves tokens. Multi-agent workflows can consume more tokens when tasks are small, tightly coupled, poorly decomposed, or repeatedly briefed.

## Research status: August 10, 2026

The following separates official model capabilities from behavior observed in the tested Codex harness. That distinction matters: a model can exist in the API catalog without being selectable as a Codex subagent.

### What is officially established

- [OpenAI describes GPT-5.6 Sol](https://developers.openai.com/api/docs/guides/latest-model) as the frontier route, Terra as the intelligence/cost balance, and Luna as the efficient high-volume route.
- [The Luna model page](https://developers.openai.com/api/docs/models/gpt-5.6-luna) says Luna roughly corresponds to the older nano tier. It supports reasoning, function calling, structured outputs, image input, and the standard Responses API tools, including hosted shell, apply patch, skills, computer use, MCP, and tool search.
- Luna's lower per-token price can reduce cost. It does not inherently reduce the number of tokens a workflow consumes.
- OpenAI recommends starting reasoning at a representative baseline and testing lower or higher effort. It does not claim that maximum reasoning, Luna, or multi-agent orchestration preserves quality automatically.
- OpenAI's documented multi-agent feature is beta in the Responses API. That does not prove identical model-routing behavior in every Codex desktop or CLI build.

### What does not currently work in the tested Codex setup

The tested environment used `codex-cli 0.147.0-alpha.6.5` on August 10, 2026.

- Its Codex subagent spawn surface exposed explicit model overrides for `gpt-5.6-sol` and `gpt-5.6-terra`, but not `gpt-5.6-luna`.
- Adding “use Luna” to `AGENTS.md` cannot make an unavailable spawn target callable. `AGENTS.md` guides behavior; the harness controls which models can actually be selected.
- API/Playground access to Luna does not establish Luna availability inside Codex subagents. These are separate execution surfaces.
- A Luna default or explicit Luna child route has therefore not been verified in this Codex build. The copyable configuration below uses Terra rather than presenting an untested Luna route as working.
- Per-spawn reasoning overrides are behavioral requests, not proof of the effective runtime. Consequential tasks should inspect child metadata when the harness exposes it.
- The `[agents]` configuration used below works in the tested alpha build, but Codex settings can change. Confirm the keys and effective child metadata after upgrades.

### Why Luna is not the default here

Luna is aimed at cost-sensitive, high-volume workloads and is closer to a nano tier. That makes it a candidate for classification, extraction, formatting, inventorying, or other mechanically verifiable work—not the safest untested default for implementation, commits, architecture, ambiguous debugging, or consequential review.

Even after Codex exposes Luna as a child override, introduce it only behind acceptance checks. A cheaper first pass can cost more overall when the main agent must rediscover context, repair defects, or repeat tests.

Do not use a low-cost child as a “code committer.” Children should return scoped diffs and evidence; the main agent should inspect the integrated diff, run checks, and own staging and commits.

### Claims this example deliberately does not make

- It does not claim that subagents always reduce total tokens.
- It does not equate lower per-token price with fewer tokens.
- It does not apply OpenAI's internal lean-prompt savings percentages directly to this setup.
- It does not claim quality parity without an A/B evaluation on the adopting team's repositories.
- It does not claim that `AGENTS.md` is a deterministic model scheduler.

## Copy the setup

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

Luna is deliberately excluded from the default. Add it only after the Codex harness actually exposes the route and representative evaluations show that it preserves quality for a narrower mechanical or high-volume workload.

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
- [OpenAI GPT-5.6 Luna model page](https://developers.openai.com/api/docs/models/gpt-5.6-luna)

OpenAI reports directional token and cost improvements from leaner prompts in internal evaluations, but those figures are not a guaranteed savings estimate for this workflow. Validate on your own workload.
