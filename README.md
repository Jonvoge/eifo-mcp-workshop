# Finance Fabric/Power BI MCP workshop

A starter repo for a 3-hour workshop where a finance team explores the Fabric and Power BI MCP servers from VS Code and Copilot. They have no Power BI experience and no Power BI licenses, so all the work is local. They describe what they want; the agent builds and validates it.

## Contents

```
.vscode/mcp.json                 Both MCP servers, configured
.github/copilot-instructions.md  Finance instructions the agent reads automatically
AGENTS.md                        Same, for other agent tools
LAB.md                           Step 0, four tracks, and the report capstone
data/FinanceSample.xlsx          Tidy star-schema dataset and bring-your-own template
sample/FinanceSample.pbip        Backup model, data generated inline (no connection)
skills/                          Vendored Microsoft Skills for Fabric (MIT)
assets/chart.umd.js              Vendored Chart.js (MIT) for offline HTML reports
report-template.html             Skeleton for the HTML capstone
sample-report.html               Worked example with real numbers
```

Give each group one track from `LAB.md`. They all start at Step 0.

## Check these before the day

Two things will stop the workshop dead if they are wrong, and both need lead time.

1. Copilot Enterprise must allow MCP servers. For enterprise accounts this is off by default and only an admin can turn it on, in Copilot settings on GitHub.com. If it is off, no MCP tools show up in Copilot Chat. Confirm it this week, then have one person check the tools appear in the Copilot tool picker.
2. The laptops need install rights for VS Code, the GitHub Copilot and Copilot Chat extensions, Power BI Desktop (free), and Node.js LTS. Finance machines are often locked.

Windows only. The Power BI Modeling MCP and Desktop's local engine do not run on macOS or Linux.

## How the MCP is configured

`.vscode/mcp.json` registers two servers when the folder opens.

`powerbi-modeling` runs locally via `npx @microsoft/powerbi-modeling-mcp`. It connects to an open Power BI Desktop model or to PBIP/TMDL files, creates and edits tables, relationships, and measures, and runs and validates DAX. Write operations ask for approval before the first change and the first query. Leave that on. Do not add `--skipconfirmation`. For a group that only explores, add `--readonly` to its args.

`fabric-core` is the remote server at `https://api.fabric.microsoft.com/v1/mcp/core` for workspace and data discovery. First use opens an Entra sign-in. It reads what each user already has access to and does not need a Power BI license.

An alternative to `npx` is the Power BI Modeling MCP VS Code extension (`aka.ms/powerbi-modeling-mcp-vscode`), which bundles the server and registers itself. If you use it, drop the `powerbi-modeling` block from `mcp.json` and enable it in the tool picker.

## Skills

Two kinds of skill ship here.

The finance instructions in `.github/copilot-instructions.md` (and `AGENTS.md`) load automatically when the folder opens. They tell the agent to validate every measure, use `DIVIDE`, treat Variance as Actual minus Budget, and stay off production. The labs run better because of them. Show the file in the wrap so people see why the agent behaved the way it did.

Microsoft's skills are vendored under `skills/` (the `powerbi-authoring` plugin, MIT, v0.3.3; see `skills/NOTICE.md`). They are opt-in per task: semantic-model-authoring, report-design, report-authoring, report-planning, and a consumption skill for querying.

One slide for the intro, the layers of the stack:

| Layer | Tool |
|---|---|
| Model: tables, measures, DAX | Modeling MCP + semantic-model-authoring skill |
| Data queries | Modeling MCP query tools + consumption skill |
| Report layer: pages, visuals, themes | report-authoring skill |
| Planning and design | report-planning + report-design skills |

The skills are built for the Copilot CLI but work with VS Code Copilot. Vendoring them here is the no-install path; the official plugin install is the alternative. If you want the native PBIR capstone to be hands-on, pre-test its CLI installs first (see the capstone notes below).

## The report capstone

The team cannot publish a Power BI report to the service without licenses, but they can build one locally. `LAB.md` has two ways.

The HTML report needs no extra installs. The agent queries the model and writes one self-contained page using the local `assets/chart.umd.js`, so it works offline. `report-template.html` is the skeleton and `sample-report.html` is a worked example. It is a snapshot, and an HTML file of real finance numbers is no longer governed once it is shared, so set expectations on that.

The native PBIR report uses the report-authoring skill to produce a real `.pbir` opened in Power BI Desktop. It is in preview and needs the `powerbi-report-author` and Desktop bridge CLIs. Run it as a stretch task or a facilitator demo, with the HTML report as the fallback.

`sample-report.html` was built by following the report-design skill: an executive-summary layout, a diverging red/green variance treatment, one color per measure, and year-over-year context on the KPIs. The scope is operating spend (COGS plus Opex) so the totals are coherent rather than mixing revenue and cost.

## Licensing, for the intro

- Power BI Desktop is free. Building models, writing and validating DAX, and building reports locally needs no license.
- The local modeling MCP works against Desktop's engine or PBIP files, so the whole loop is license-free.
- Connecting Desktop to the Lakehouse SQL endpoint or OneLake is governed by Fabric workspace roles, not Power BI Pro. A finance user with workspace access can pull live Lakehouse data into a local model. Confirm the tenant's settings.
- F64 capacity removes the per-user Pro requirement to consume published content, but publishing to the service still needs Pro or PPU, which the team does not have. So the work stays local.

## The sample model

`sample/FinanceSample.pbip` is the backup if a group cannot connect to Fabric or their own Excel. It is a small star schema (Date, CostCenter, Account, Actuals, Budget) with data generated in Power Query, so it needs no connection or credentials: open it in Power BI Desktop, refresh once, and it is populated and offline. It ships with two measures, Total Actuals and Total Budget. The variance, YoY, and YTD measures are left for the labs.

Open it once before the day to check it loads. It was authored to the PBIP/TMDL spec but not opened in Desktop during creation, and the format is version-sensitive. If Desktop rejects it, rebuild from the Excel: Get Data, Excel, `data/FinanceSample.xlsx`, load the five sheets, set the relationships (or ask the agent to), then Save As a .pbip and commit that. That also makes a clean Step-0 demo.

## Run of show

Intro and demo 40, smoke test 10, hands-on block 1 40, unstick 10, hands-on block 2 40, prep 10, share-back 25, wrap 5.
