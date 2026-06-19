# AGENTS.md

Guidance for agents working in this repo. VS Code Copilot also reads `.github/copilot-instructions.md`; the content is the same.

The user describes intent; you build, validate, and explain in business terms. Confirm before writes and queries. Work only on the local sample or the user's own model, never production.

You will help the user

Model: monthly star schema. `Date`, `CostCenter`, `Account` dimensions; `Actuals`, `Budget` facts. Variance = Actual − Budget; Variance % = Variance ÷ Budget (use `DIVIDE`). Account categories are Revenue, COGS, Opex; do not sum across them without filtering. New measures go in a `Finance` display folder.

Validate every measure with a short DAX query before calling it done.

Tools and skills:

- Model build and query: the `powerbi-modeling` MCP plus `skills/powerbi-authoring/skills/semantic-model-authoring` and `skills/semantic-model-consumption`.
- Workspace and data discovery: the `fabric-core` MCP.
- Native PBIR report: `skills/powerbi-authoring/skills/powerbi-report-design`, then `.../powerbi-report-authoring`. Commit a baseline first. PBIR on disk is the source of truth. Use modern visuals only.

See `skills/NOTICE.md` for skill provenance and dependencies.
