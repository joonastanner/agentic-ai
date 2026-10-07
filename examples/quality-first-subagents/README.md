# Quality-first Codex subagents

This example uses GPT-6.1 Sol for architecture, implementation, integration, and consequential review, with an opt-in GPT-6 Luna route for narrow, verifiable child work. The copyable baseline keeps children on Sol until the cheaper route passes local checks.

It is designed to reduce cost and duplicated context without assuming that adding agents automatically saves tokens. Multi-agent workflows can consume more tokens when tasks are small, tightly coupled, poorly decomposed, or repeatedly briefed.

## Research status: October 7, 2026

Original harness research: **August 10, 2026**. This refresh checks current official documentation; it does **not** claim a new local Codex CLI runtime test. API support, documented Codex support, account access, and observed child execution are distinct.

### What is officially established

- [Current model guidance](https://developers.openai.com/api/docs/guides/latest-model) distinguishes Astra for the hardest work, GPT-6.1 Sol for complex work at lower cost, and GPT-6 Luna for focused, high-volume tasks. The old description of GPT-5.6 Luna as a nano-like tier is not a capability assessment of GPT-6 Luna.
- [Codex model guidance](https://learn.chatgpt.com/docs/models) recommends GPT-6.1 Sol when available and Luna for focused work. GPT-6 Sol remains listed as available; this example follows the newer recommendation. Access still depends on account, client, and workspace settings.
- [Codex subagent guidance](https://learn.chatgpt.com/docs/agent-configuration/subagents#choosing-models-and-reasoning) explicitly recommends `gpt-6.1-sol` for demanding agents and `gpt-6-luna` for narrow agents, with `high` as a Luna starting point. This is Codex-specific evidence, independent of the API releases.
- [Responses API multi-agent](https://developers.openai.com/api/docs/guides/responses-multi-agent) is documented in beta for GPT-6.1 Sol and all GPT-5.6 models. Its documented children share the request's model and tools. This does not establish mixed-model Codex routing, and this guide's TOML is not API request configuration.
- The [GPT-6.1 Sol](https://developers.openai.com/api/docs/models/gpt-6.1-sol) and [GPT-6 Luna](https://developers.openai.com/api/docs/models/gpt-6-luna) pages document their API capabilities and reasoning levels. API effort defaults and Codex client defaults need not match. Neither maximum effort nor a model release establishes quality parity on your workload.

### Verification status

| Checked | Surface / version | Evidence and limit |
| --- | --- | --- |
| 2026-08-10 | `codex-cli 0.147.0-alpha.6.5` | Original repository report: spawn overrides exposed GPT-5.6 Sol and Terra, not GPT-5.6 Luna; the Sol/Terra config worked there. Historical, not re-tested in this refresh. |
| 2026-10-07 | Responses API documentation | GPT-6.1 Sol multi-agent support is documented. This does not verify a Codex spawn target or a mixed-model API route. |
| 2026-10-07 | Current Codex documentation | GPT-6.1 Sol / GPT-6 Luna child routes and the bundled config keys are documented. This is not an account-specific execution result. |
| 2026-10-07 | Local CLI runtime | Not tested: no `codex` executable in the audit workspace. Effective child models, effort, and quality remain to be checked in the adopting environment. |

The August exclusion applied to **GPT-5.6 Luna in that alpha build**. It is not a permanent rule about Luna or later Codex releases. `AGENTS.md` cannot expose an unavailable model or bypass workspace access.

### Why Luna is opt-in here

Current Codex documentation supports Luna for narrow subagents. This example still starts with Sol children because no repository-specific comparison was run for this refresh. That is a quality policy, not an availability claim. Try Luna on extraction, inventories, focused scans, and mechanical edits with explicit acceptance checks; retain Sol for broader implementation and consequential judgment until evaluation justifies a change.

A cheaper first pass can cost more overall when the main agent must rediscover context, repair defects, or repeat tests.

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
4. Complete the verification below before adopting the route. If a configured model is unavailable, retain a previously verified setup or deliberately select and verify an available alternative. Do not silently substitute models.

This is a **local Codex client** setup. Copying TOML into a repository does not configure hosted ChatGPT Work. The [configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference) documents `model`, `model_reasoning_effort`, `agents.enabled`, `agents.default_subagent_model`, and `agents.default_subagent_reasoning_effort`.

Explicit spawn settings override `[agents]` defaults. A [custom agent file](https://learn.chatgpt.com/docs/agent-configuration/subagents#custom-agents) can override both; inspect it when the effective model or effort differs. A prose request alone is not runtime evidence.

### Verify after a model or client change

- Record the date, `codex --version` (or app build), client, sign-in method, and relevant workspace restrictions. Check the actual spawn schema and supported efforts, not just the main model picker.
- Run serial read-only checks on a known small file: one child using defaults, one explicit `gpt-6.1-sol` / `high` child, and, before enabling it, one `gpt-6-luna` / `high` child. Have each return a checkable inventory; do not allow edits or commits.
- Record requested and effective model/effort from runtime metadata or logs, plus the check result. A child's self-reported identity is not proof. If metadata is unavailable, record **unverified**, not pass.
- Keep a dated row with the build, route, evidence, and result beside the matrix above. Then run the representative quality comparison below before widening Luna's scope. A successful spawn establishes availability, not quality parity.

## Routing strategy

These are this example's starting policies, not measured equivalence across model generations. Here **Sol** means `gpt-6.1-sol`; **Luna** means `gpt-6-luna`.

- **Sol/medium:** conservative child default for bounded exploration, documentation, and precisely specified work.
- **Luna/high, opt-in:** evaluated narrow extraction, inventories, focused scans, and mechanical edits.
- **Sol/high:** implementation, architecture, decomposition, ambiguous diagnosis, integration decisions, and consequential final review.
- **Sol/xhigh:** authentication, concurrency, migrations, data-loss hazards, or a hard unresolved failure, where supported.
- **Max:** exceptional measured need only.

The config retains `high` for the main agent as an explicit quality preference. Tune effort on representative tasks; a familiar effort label does not guarantee equivalent behavior after a model upgrade. Astra is a documented option for harder work, but this example does not silently add an Astra escalation or claim it has been evaluated here.

## Where savings should come from

### Input tokens

- Independent children use fresh context, with `fork_turns="none"` when the actual spawn interface supports it. Do not assume that argument or partial-history modes exist in every client.
- Children receive compact task contracts rather than raw dictated or repetitive requests.
- Briefs point to canonical files instead of pasting source, logs, diffs, or global instructions.
- One child is reused for follow-up work in the same subsystem.

These rules reduce duplicated subagent input. They cannot remove tokens from the original message after the main agent has received it.

### Output tokens

- Children return a fixed, terse result contract.
- Large logs remain in workspace files.
- Higher reasoning effort is reserved for work where it produces a measured gain.

### Cost

The Sol-only baseline makes no cross-model cost-saving claim. GPT-6 Luna's documented API token prices are lower than GPT-6.1 Sol's; compare the linked model pages for current rates. Lower rates do not mean fewer tokens or a cheaper completed task after rework. For ChatGPT-authenticated Codex, measure the applicable plan usage rather than treating API prices as your bill.

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

- [OpenAI current model guidance](https://developers.openai.com/api/docs/guides/latest-model)
- [OpenAI GPT-6.1 Sol model page](https://developers.openai.com/api/docs/models/gpt-6.1-sol)
- [OpenAI GPT-6 Luna model page](https://developers.openai.com/api/docs/models/gpt-6-luna)
- [Responses API multi-agent](https://developers.openai.com/api/docs/guides/responses-multi-agent)
- [Codex models and availability](https://learn.chatgpt.com/docs/models)
- [Codex subagents and custom agents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [Codex configuration reference](https://learn.chatgpt.com/docs/config-file/config-reference)

Checked October 7, 2026. These are live documentation links; retain the dated verification record when their recommendations change.
