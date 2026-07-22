# @planoda/templates

> Free, open-source project templates for [Planoda](https://planoda.com) — the
> AI-native work platform that replaces **Linear, ClickUp, Monday, and Trello**.
> One schema, one login, an AI agent on every surface.

10 realistic, opinionated, ready-to-use templates as plain JSON — sprint
boards, bug triage, OKRs, incident response, and more. Copy the fields you
need into any tool that speaks issues/workflow-states/labels/views (Linear,
Jira, ClickUp, Plane, your own tracker), or one-click import straight into a
Planoda workspace.

No build step, no dependencies, no lock-in: it's just data.

## Templates

| Template | Slug | Category | Import |
| --- | --- | --- | --- |
| 🏃 Sprint Board | `sprint-board` | Engineering / Agile | [Import →](https://planoda.com/templates/sprint-board?utm_source=templates&utm_medium=github) |
| 🐛 Bug Triage & Support Queue | `bug-triage` | Support / QA | [Import →](https://planoda.com/templates/bug-triage?utm_source=templates&utm_medium=github) |
| 🗺️ Product Roadmap | `product-roadmap` | Product | [Import →](https://planoda.com/templates/product-roadmap?utm_source=templates&utm_medium=github) |
| 🎯 OKR / Goals Tracker | `okr-goals` | Strategy / Leadership | [Import →](https://planoda.com/templates/okr-goals?utm_source=templates&utm_medium=github) |
| 💬 Customer Feedback Intake | `customer-feedback` | Product / Customer Success | [Import →](https://planoda.com/templates/customer-feedback?utm_source=templates&utm_medium=github) |
| 🤖 AI Agent Governance (Propose / Approve) | `agent-governance` | AI / Operations | [Import →](https://planoda.com/templates/agent-governance?utm_source=templates&utm_medium=github) |
| 🚨 Incident Response | `incident-response` | Engineering / SRE | [Import →](https://planoda.com/templates/incident-response?utm_source=templates&utm_medium=github) |
| 📅 Content Calendar | `content-calendar` | Marketing | [Import →](https://planoda.com/templates/content-calendar?utm_source=templates&utm_medium=github) |
| 🧑‍💼 Hiring Pipeline | `hiring-pipeline` | People / Recruiting | [Import →](https://planoda.com/templates/hiring-pipeline?utm_source=templates&utm_medium=github) |
| 🚀 Launch Checklist | `launch-checklist` | Product / Go-to-Market | [Import →](https://planoda.com/templates/launch-checklist?utm_source=templates&utm_medium=github) |

Each import link opens `https://planoda.com/templates/<slug>`, which deep-links
into the Planoda app: sign in (or create a free workspace in under a minute)
and the template's teams, workflow states, labels, issues, and views are
materialized for you — no manual setup.

Looking for a **Linear alternative**, a **ClickUp alternative**, or a **Jira
alternative** with these workflows built in? This is the fastest way to try
one.

## Use it your way

### 1. One-click import into Planoda

Click any "Import →" link above, or construct your own:

```
https://planoda.com/templates/<slug>?utm_source=templates&utm_medium=github
```

### 2. Copy/paste into any tool

Every template is a single self-contained JSON file under
[`templates/`](./templates). Open the one you want, and hand-translate the
`workflowStates`, `labels`, and `issueTemplates` into your tool of choice — the
shapes are intentionally close to how Linear/Jira/ClickUp/Plane model the same
concepts, so the mapping is usually 1:1.

### 3. Install as a package

```bash
npm i @planoda/templates
# or
pnpm add @planoda/templates
```

```ts
import sprintBoard from "@planoda/templates/templates/sprint-board.json" with { type: "json" };

console.log(sprintBoard.name); // "Sprint Board"
console.log(sprintBoard.issueTemplates.length);
```

(Or read the file straight off disk from `node_modules/@planoda/templates/templates/*.json` in any language — it's plain JSON.)

## Template JSON shape

Every file in [`templates/`](./templates) follows this shape:

```jsonc
{
  "slug": "sprint-board",       // unique id, used in the import URL
  "name": "Sprint Board",
  "description": "…",
  "category": "Engineering / Agile",
  "icon": "🏃",                  // a single emoji

  "teams": [
    {
      "key": "ENG",             // short team key (issues are numbered ENG-123)
      "name": "Engineering",
      "description": "…",
      "icon": "💻",
      "color": "#6366f1",       // hex
      // optional, agile-flavored templates only:
      "estimateScale": "points",
      "estimatePoints": [0, 1, 2, 3, 5, 8, 13],
      "cycleEnabled": true,
      "cycleDurationDays": 14,
      "cycleStartDay": 1
    }
  ],

  "workflowStates": [
    {
      "name": "In Progress",
      // one of: backlog | unstarted | started | completed | canceled | triage
      "type": "started",
      "color": "#3b82f6",
      "position": 2,
      "description": "…"
    }
  ],

  "labels": [
    { "name": "bug", "color": "#ef4444", "emoji": "🐞", "description": "…" }
  ],

  "issueTemplates": [
    {
      "ref": "sprint-goal",      // optional local id, for parent/child links
      "parent": null,            // optional — the `ref` of a parent item, for
                                  // epics/objectives/requisitions with children
      "team": "ENG",             // which team key this issue belongs to
      "issueType": "Bug",        // free-text work-item type label
      "title": "…",
      "descriptionMd": "…",      // markdown body
      "priority": 2,             // 0 none · 1 urgent · 2 high · 3 medium · 4 low
      "estimate": 3,             // optional, points/hours per the team's scale
      "workflowState": "Todo",   // must match a `name` in this template's `workflowStates`
      "labels": ["bug"],         // must match `name`s in this template's `labels`
      "dueDateOffsetDays": 5     // optional — days from the import date, for
                                 // date-sensitive templates (content calendars,
                                 // launch checklists); omit for date-agnostic work
    }
  ],

  "views": [
    {
      "name": "Sprint Board",
      // one of: board | list | table | timeline | calendar | roadmap | dashboard | map | gallery | form
      "layout": "board",
      "description": "…",
      "scope": "team",           // team | workspace | private
      "groupBy": "status",       // convenience hint for the primary grouping
      "filter": [{ "field": "cycle", "op": "in", "values": ["current"] }],
      "display": { "swimlaneBy": "priority" }
    }
  ]
}
```

The field names deliberately mirror Planoda's own `teams` / `workflow_states` /
`labels` / `issues` / `views` tables (see
[`src/db/schema/`](https://github.com/idimetrix/team-task-manager/tree/main/src/db/schema)
in the main repo) so an import maps almost 1:1 into real rows — but the format
has no Planoda-specific dependency and is plain, portable JSON any tracker can
consume.

## Why these 10

Picked to cover the workflows real teams actually run, not generic busywork:

- **Sprint Board** — day-to-day engineering, Fibonacci points, 2-week cycles.
- **Bug Triage & Support Queue** — severity-driven intake with an SLA-shaped funnel.
- **Product Roadmap** — Now/Next/Later prioritization with a timeline view.
- **OKR / Goals Tracker** — Objectives as epics, Key Results as linked children.
- **Customer Feedback Intake** — one queue for requests/bugs/praise, closes the loop when shipped.
- **AI Agent Governance** — the propose/approve pattern for treating agents as teammates, safely.
- **Incident Response** — detection → mitigation → postmortem, with action items tracked as follow-ups.
- **Content Calendar** — editorial pipeline across blog/video/social/newsletter.
- **Hiring Pipeline** — requisitions as epics, candidates as linked children.
- **Launch Checklist** — one cross-functional checklist for engineering + marketing + support.

## Contributing

Templates are plain JSON — PRs adding a new template or improving an existing
one are welcome. Keep to the shape documented above, keep the content
genuinely useful (not filler), and validate your JSON before opening a PR:

```bash
node -e "JSON.parse(require('fs').readFileSync('templates/your-template.json', 'utf8'))"
```

## License

MIT © Planoda — see [`LICENSE`](./LICENSE).

---

<sub>⚡ Powered by [Planoda](https://planoda.com) — the AI-native Linear,
ClickUp, Monday & Trello alternative. 100x better, 100x more features, 100%
AI-powered.</sub>
