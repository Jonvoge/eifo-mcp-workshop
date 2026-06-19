# Lab guide

You will not write any code today. You describe what you want in plain language and the agent builds and checks it. Your job is to read the result and decide whether it is right. If a number or concept looks wrong, tell the agent and ask it to fix it.

## Getting started

1. Open this folder in VS Code (File, Open Folder). Open GitHub Copilot Chat and set it to Agent mode.
2. Click the tools icon in the chat box and confirm `powerbi-modeling` and `fabric-core` are listed and enabled. If they are missing, tell a facilitator.
3. Smoke test. Paste this and check you get a sensible reply: "List the workspaces I can access in Fabric." The first run asks you to sign in through the browser. Approve it.

## Exercise 1: choose your data

Pick one source. 

Option A, Fabric. Build a fresh local model on real data.
1. Open Power BI Desktop, OneLake Catalog, select a data source. Sign in with your work account, select a few tables, Load.
2. File, Save As, Power BI project (.pbip), into a new folder in this repo. Leave Desktop open and move into VS Code.
3. In VS Code, open a Github Copilot Chat and prompt: "Connect to my open Power BI Desktop model and list the tables." From here on you use the agent, not the Desktop UI.

Option B, use your own Excel file. Process is the same as A, but instead of clicking "OneLake Catalog" you choose Get Data -> Excel and select your file. Bring one clean flat table, or use `data/FinanceSample.xlsx` as a template.

Option C, the finished sample. If all else fails, open `sample/FinanceSample.pbip` in Power BI Desktop. Then open VS Code and a Github Copilot chat and prompt "Connect to my open Power BI Desktop model." 

## Exercise 2: select one or more tracks to explore in your group

## Track A: Chat with your data!

You want answers without building anything. The agent writes and runs queries for you and finds answers from your data.

Spend some time quizzing the agent about your data. Make it reason. Push it. Find its boundaries and where it excels.

1. What is total actuals vs total budget for the latest year, by cost centre?
2. Which cost centres are over budget, and by how much? Sort worst first.
3. Show actuals by account category (Revenue, COGS, Opex) by quarter.
4. What is the gross margin (Revenue minus COGS) trend across the period?
5. Pick one answer and ask: show me the DAX you used and explain it in one sentence.

Deliverable suggestion: Examples of questions it answers well vs. questions it does not.


## Track B: build the measures your team needs

You know the business logic, not the code. Describe the metrics and KPIs that you need and have the agent create and validate them.

Example prompts:

1. Add a measure "Actual vs Budget Variance" = total actuals minus total budget. Format as a whole number.
2. Add "Variance %" = variance over total budget, as a percentage.
3. Add "YoY Actuals %" comparing each period to the same period last year.
4. Add "YTD Actuals" and "Rolling 3-Month Actuals".
5. Validate every new measure with a small DAX query and show the top 5 cost centres. Put the measures in a "Finance" display folder.

Feel free to push the agent with complex logic, edge cases and more.

Deliverable suggestion: An extended semantic model with new measures.

## Track C: document and govern the model

You are responsible for audit-readiness and onboarding.
Use the agent to help you generate the necessary artifacts to smoothen that task. Ask it to:

1. Document every measure: name, what it calculates in business terms, and the DAX, as a markdown table.
2. Generate a Mermaid diagram of the table relationships.
3. List every relationship and the direction the filter flows.
4. Check the model against best practices and list issues by severity, with a one-line fix for each.
5. Find columns not used by any measure or relationship, as candidates to hide.

Deliverable suggestion: A documentation file in the repo created by the agent.


## Exercise 3: turn your model into a report

Now it's time to turn the model into something visual.

### Option 1: HTML report

Instruct your agent to create an HTML report based on the open semantic model.

1. Tell it about KPIs you want included. Comparisons, variances.
2. Tell it how you want it visualized. Big-Ass-Numbers at the top. Line charts. Bar Charts. DO-NOT charts.
3. Tell it how you want it formatted. What are your company colors and fonts? Font sizes? Annotations and tooltips?
4. Open the HTML file created by the agent and repeat the process.

The more specific you can be, the better. Obviously, iterations are likely needed.


### Option 2: native Power BI report 

Instruct the agent to use the `powerbi-report-authoring` skill to author a real `.pbir` file that you can open in Power BI Desktop.

1. Using the report-design skill, propose a one-page report based on your model.
2. Tell it about KPIs you want included. Comparisons, variances.
3. Tell it how you want it visualized. Big-Ass-Numbers at the top. Line charts. Bar Charts. DO-NOT charts.
4. Tell it how you want it formatted. What are your company colors and fonts? Font sizes? Annotations and tooltips?
5. Open the generated file in Power BI and repeat as necessary.



## Exercise 4: Share-back in plenary

- Show your work. What was cool? What was not?
- Say where it would help your real work, and what you would not trust the agent to do on its own.
- Share something that surprised you.
