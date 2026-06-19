# Vendored skills

These folders are copied unmodified from Microsoft's Skills for Fabric so the workshop runs without a live plugin install.

- Source: https://github.com/microsoft/skills-for-fabric
- Plugin: `powerbi-authoring`, version 0.3.3
- License: MIT (see `SKILLS-LICENSE.txt`), © 2026 Microsoft Corporation
- Copied: June 2026

## Contents

`powerbi-authoring/` is the full plugin. Its skills:

- `semantic-model-authoring`: create and edit tables, columns, measures, DAX
- `powerbi-report-planning`: requirements to brief to build
- `powerbi-report-design`: produces a design brief (layout, charts, color, theme)
- `powerbi-report-authoring`: authors the PBIR report layer
- `powerbi-report-management`: Fabric publishing
- `check-updates`: version check helper

`semantic-model-consumption/` covers querying an existing model. It comes from the `fabric-consumption` plugin and is included for the analysis track.

## Notes

- The skills are in preview and pinned at this version. To update, re-copy from the source repo and change the version above.
- `powerbi-report-authoring` drives the `powerbi-report-author` CLI and the Power BI Desktop bridge CLI, which are separate installs. The native PBIR report needs them; the HTML report does not.
- The skills run an update check once per session. Offline, that check fails quietly, which is fine for the workshop.
- `.github/copilot-instructions.md` and `AGENTS.md` point the agent at these files. The official install (Copilot CLI plugin or the VS Code extension) is the alternative to this vendored copy.
