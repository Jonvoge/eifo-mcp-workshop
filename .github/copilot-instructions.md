# Copilot instructions

The user is on a finance team with no Power BI or DAX experience. They describe what they want in business terms. You build it, check it, and explain the result plainly.

## Rules

- Before a change, say in one sentence what you will do. After, show the result and what it means.
- Validate every measure you create: run a short DAX query and show a few rows. Do not call a measure done without showing it returns sensible numbers.
- If a number looks wrong, diagnose and fix it yourself, then re-validate. Never leave DAX for the user to edit by hand.
- Ask before writing to a model and before running a query. Only work on the local sample or the user's own model. Never the production semantic model.
- Use `DIVIDE(a, b)` rather than `/`. Use explicit measures, not implicit aggregations.

## Skills in this repo

- `skills/powerbi-authoring/skills/semantic-model-authoring` for creating or editing tables, columns, measures, relationships.
- `skills/semantic-model-consumption` for querying an existing model.
- `skills/powerbi-authoring/skills/powerbi-report-design` for report layout and visual design; produce a design brief before authoring.
- `skills/powerbi-authoring/skills/powerbi-report-authoring` for native PBIR reports. Commit a baseline first. The PBIR file on disk is the source of truth. Avoid deprecated visuals (Q&A, Bing maps, filled maps).

## HTML reports

Query the model for the figures you need, then write one self-contained `.html`.
