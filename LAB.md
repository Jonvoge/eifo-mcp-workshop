# Lab guide

You will not write any DAX today. You describe what you want in plain language and the agent builds and checks it. Your job is to read the result and decide whether it is right. If a number looks wrong, tell the agent and ask it to fix it.

## Getting started

1. Open this folder in VS Code (File, Open Folder). Open GitHub Copilot Chat and set it to Agent mode.
2. Click the tools icon in the chat box and confirm `powerbi-modeling` and `fabric-core` are listed and enabled. If they are missing, tell a facilitator. It is almost always the Copilot MCP setting.
3. Smoke test. Paste this and check you get a sensible reply: "List the workspaces I can access in Fabric." The first run asks you to sign in through the browser. Approve it.

When the modeling MCP asks you to approve its first change or first query, approve it. That prompt is your safety net.

## Step 0: choose your data

Pick one source. The track steps are the same whichever you pick.

Option A, Fabric. Build a fresh local model on real data.
1. Open Power BI Desktop, Get Data, More, search Lakehouse (or SQL Server, or the Lakehouse SQL endpoint). Sign in with your work account, pick the finance lakehouse, select a few tables, Load. Choose Import, not DirectQuery.
2. File, Save As, Power BI project (.pbip), into a new folder in this repo. Leave Desktop open.
3. In Copilot Chat: "Connect to my open Power BI Desktop model and list the tables." From here on you use the agent, not the Desktop UI.

Option B, your own Excel. The same as A, but Get Data, Excel, your file. Bring one clean flat table, or use `data/FinanceSample.xlsx` as a template: keep the headers, replace the numbers. Workbooks with merged cells, total rows, or several stacked tables will fight you.

Option C, the sample. Open `sample/FinanceSample.pbip` in Power BI Desktop, Refresh once, then "Connect to my open Power BI Desktop model." Works offline.

Whatever you build today is local and disposable. Do not point a write-enabled agent at the production semantic model.

## Track A: ask the model questions

You want answers without building anything. The agent writes and runs DAX and answers from your data.

1. What is total actuals vs total budget for the latest year, by cost centre?
2. Which cost centres are over budget, and by how much? Sort worst first.
3. Show actuals by account category (Revenue, COGS, Opex) by quarter.
4. What is the gross margin (Revenue minus COGS) trend across the period?
5. Pick one answer and ask: show me the DAX you used and explain it in one sentence.

Deliverable: five questions answered, plus the DAX behind one of them in a `notes.md`. Stretch: ask for the same answer two ways and check they agree.

## Track B: build the measures finance needs

You know the business logic, not DAX. Describe each measure and have the agent validate it.

1. Add a measure "Actual vs Budget Variance" = total actuals minus total budget. Format as a whole number.
2. Add "Variance %" = variance over total budget, as a percentage.
3. Add "YoY Actuals %" comparing each period to the same period last year.
4. Add "YTD Actuals" and "Rolling 3-Month Actuals".
5. Validate every new measure with a small DAX query and show the top 5 cost centres. Put the measures in a "Finance" display folder.

Deliverable: the new measures and a screenshot of the validation. Stretch: ask the agent to make any measure that could divide by zero safe.

## Track C: document and govern the model

You are responsible for audit-readiness and onboarding.

1. Document every measure: name, what it calculates in business terms, and the DAX, as a markdown table.
2. Generate a Mermaid diagram of the table relationships.
3. List every relationship and the direction the filter flows.
4. Check the model against best practices and list issues by severity, with a one-line fix for each.
5. Find columns not used by any measure or relationship, as candidates to hide.

Deliverable: a `MODEL-DOC.md` in the repo. Stretch: a five-line summary of what the model is for, for a new finance hire.

## Track D: discover data, then scaffold

You are scoping a new dataset.

1. List the workspaces I can access and the tables in the finance lakehouse, with a one-line description of each.
2. Which tables look like facts and which like dimensions?
3. In a fresh model (Option B or C): add a "Budget Utilization %" measure = actuals over budget.
4. Add a cost centre scorecard: measures for variance, variance %, and an "Over" / "Under" flag.
5. Create a calculation group that switches a measure between Actual, Budget, and Variance.

Deliverable: a one-paragraph inventory of what is in Fabric, plus the new measures. Stretch: add a small Scenario dimension and relate it in.

## Capstone: turn your model into a report

You cannot publish to the service without licenses. You can build a report locally. Pick one.

### Option 1: HTML report

No extra installs. The agent queries your model and writes one web page anyone can open.

1. Run a DAX query for actuals vs budget and variance % by cost centre for the latest year. Then build an HTML report from `report-template.html`: KPI cards, a bar chart of variance by cost centre, a monthly actual-vs-budget line, a detail table, and a three-sentence summary of the drivers.
2. Use the local chart library in `assets/`, no internet.
3. Colour variance red over budget, green under.

Compare to `sample-report.html`. Deliverable: your `.html` opened in a browser. It is a snapshot and it holds finance numbers, so think about where it goes.

### Option 2: native Power BI report (stretch or demo)

Uses the `powerbi-report-authoring` skill to author a real `.pbir` you open in Power BI Desktop, all local.

1. Using the report-design skill, propose a one-page layout for an actual-vs-budget report on this model.
2. Author it as PBIR: KPI cards for the key measures, a bar chart of variance by cost centre, and a date slicer. Validate the report.
3. Open or reload it in Power BI Desktop.

Before you start, commit your work: the PBIR file on disk is the source of truth, and a baseline lets you revert. Tell the agent to use modern visuals only. This needs the report-author and Desktop bridge CLIs; if they are not installed, do Option 1 and watch the demo.

## Share-back, about 6 minutes each

- Show one thing that works.
- Say where it would help your real work, and what you would not trust the agent to do on its own.
- One thing that surprised you.
