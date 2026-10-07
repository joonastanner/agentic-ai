# Agentic AI

A collection of practical, copyable patterns for agentic AI development workflows.

# Basic setup

Each repository should have an `AGENTS.md` for project instructions. Projects with a user interface should also document their design system, for example in `DESIGN.md`.

# Examples

- [Quality-first Codex subagents](examples/quality-first-subagents/README.md): dated API and Codex verification, a GPT-6.1 Sol baseline, and an opt-in GPT-6 Luna route for bounded work.
- [Dictation and token discipline](examples/dictation-token-discipline/README.md): turn repetitive or dictated requests into compact internal task contracts without dropping requirements.

# Tips

## Make Codex great at UI!

Add this to AGENTS.md to make Codex great at UI development, which is typically where it struggles. The idea is to use image generation to create a design mock and then have Codex implement that. Instead of relying on Codex to come up with the design on the fly which usually leads to subpar results.

```markdown
## UI Design Workflow

For any work that materially changes a screen’s visual design, layout, hierarchy, or interaction polish, create a visual target before implementation.

Preferred workflow:

1. Inspect the current screen and existing design system.
2. Generate a high-quality design mockup using GPT Image 2 or the available image-generation tool.
   - Use the current screenshot as a reference when possible.
   - Ask for a polished version that preserves the existing brand, flow, and constraints.
   - Do not treat the generated mock as a final asset unless explicitly requested.
3. Implement the UI to match the mock while using the app’s real components, tokens, spacing, typography, and accessibility patterns.
4. Keep the change scoped to the requested screen or component.
5. Verify the result with screenshots at mobile and desktop/tablet widths when relevant.
6. Iterate if text wraps poorly, spacing feels off, controls overlap, or the result diverges from the mock.

Use this workflow for:
- onboarding screens
- landing pages
- empty states
- cards
- modals
- profile/setup flows
- store screenshots
- any visual polish work

Skip or simplify this workflow for:
- tiny copy edits
- bug fixes with no visual impact
- backend/API changes
- mechanical refactors

The image mock is a visual spec, not production code. The final implementation must still be responsive, accessible, performant, and consistent with the existing app.
```
