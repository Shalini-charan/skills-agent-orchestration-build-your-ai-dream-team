# Project Pulse dashboard implementation plan

## Summary

Build Mona's lightweight, static Project Pulse dashboard so contributors can scan active projects, owners, current status, recent activity, priority or risk, and a short contributor-friendly summary. The deliverable is plain HTML, CSS, and JSON, previewed through VS Code; it does not need a framework, build pipeline, or backend. Follow the exercise's Orchestrator → Planner → Designer/Coder workflow rather than treating the work as one undifferentiated implementation.

## Ordered implementation steps

1. **Confirm requirements and agree on the interface contract — Orchestrator and Planner.** Use `.github/project-pulse-brief.md`, the Step 3 exercise prompt, and `.github/agents/` as sources of truth. Fix the data shape, required visible information, shared HTML/CSS hooks (`.dashboard` and `.project-card`), and launch behavior before implementation. Keep the app a small static page.

2. **Implement the data, markup, styling, and preview configuration — Designer and Coder in parallel after Step 1.** Keep each agent within its file assignment below. The Coder should render the project data into cards; the Designer should style the agreed structure and selectors. Resolve any markup/style contract mismatch through the Orchestrator rather than having both agents edit the same file.

3. **Integrate and review — Orchestrator with Designer and Coder as needed.** Check that the data fields are rendered in the cards, CSS hooks match the HTML, local asset paths work, and the launch configuration serves the app directory and opens the page. Return any corrections to the owner of the affected file.

4. **Validate the static deliverable and preview.** Run the JSON and repository checks below, then use **Run Project Pulse Dashboard** and confirm the browser shows the dashboard rather than a directory listing. Stop the preview server after checking it.

## File assignments and agent responsibilities

| File | Owner | Assignment |
| --- | --- | --- |
| `app/index.html` | Coder | Create the page with the exact title **Project Pulse**; reference `styles.css` and `project-data.json`; provide accessible, semantic dashboard markup; load the data and render a visible `.project-card` for each project. Show its name, owner, status, recent activity, priority/risk, and a concise contributor-friendly summary. Include the `.dashboard` container and ensure data is presented as content rather than a directory listing. |
| `app/styles.css` | Designer | Create the polished dashboard presentation: clear information hierarchy, readable spacing and typography, project cards, visible status badges, distinct priority/risk treatment, sufficient contrast, and responsive layout. Style the agreed `.dashboard` and `.project-card` hooks; include rounded corners (`border-radius`) and card depth (`box-shadow`). |
| `app/project-data.json` | Coder | Supply representative static project records in a top-level `projects` array. Every record must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Add a concise `summary` value if needed to make the brief's contributor-friendly summary explicit. Keep the file strict, parseable JSON. |
| `.vscode/launch.json` | Coder | Create strict JSON (no comments) with a configuration named **Run Project Pulse Dashboard**. Serve from `app/` using `python3 -m http.server 5500`, set the working directory to `${workspaceFolder}/app`, and use `serverReadyAction` to open `http://localhost:%s/index.html`. Opening `index.html` is required so the browser displays the dashboard, not a directory listing. |

**Designer:** Owns visual and accessibility decisions and the stylesheet only. Keep status, activity, ownership, and risk easy to scan; use responsive behavior and accessible contrast/semantic affordances. Report design decisions and any markup assumptions to the Orchestrator.

**Coder:** Owns the static page markup, JSON data, and VS Code launch support. Keep implementation deterministic and easy to inspect; wire the page to the JSON and stylesheet, render the required project fields, use the shared selectors, and validate assigned files. Do not add frameworks or dependencies for this static exercise.

**Orchestrator:** Delegates using explicit file scopes, coordinates the agreed contract, integrates the separate contributions, and checks the end-to-end preview. The Planner creates this plan and identifies sequencing and validation; the Planner does not implement app files.

## Dependencies and parallel work decisions

- The brief and shared HTML/data/CSS contract must be agreed before implementation; both agents rely on it.
- `app/index.html` depends on the `projects` schema in `app/project-data.json` and the shared selectors expected by `app/styles.css`. The Coder should keep the field names aligned exactly.
- `app/styles.css` depends on the agreed dashboard/card structure, but not on final sample content. Once the selectors and card content roles are agreed, the Designer can work in parallel with the Coder.
- `.vscode/launch.json` depends on the known `app/` location and required page path; it can be created alongside the HTML and data files by the Coder.
- **Parallel:** Designer works only in `app/styles.css`; Coder works only in `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. This is safe after the contract is fixed because file ownership does not overlap.
- **Sequential:** Contract/review first; integration and final validation after both parallel contributions are complete. If the HTML structure changes after styling begins, the Orchestrator routes the change and asks the Designer to update the CSS.

## Validation expectations

- Confirm `app/index.html` has the exact **Project Pulse** title, references both `styles.css` and `project-data.json`, and renders visible `.project-card` elements with each project's status, `recentActivity`, and priority.
- Confirm `app/styles.css` contains `.dashboard` and `.project-card`, with `border-radius`, `box-shadow`, readable spacing, and responsive styling.
- Parse `app/project-data.json` and `.vscode/launch.json` with `python3 -m json.tool`. Check that the data uses the top-level `projects` key and each record has `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Run `bash scripts/validate-exercise.sh` to check the repository's exercise-level validation gates.
- In VS Code, launch **Run Project Pulse Dashboard**. Verify it serves from `app/`, opens `index.html`, displays project cards (not a directory listing), and remains readable at a narrow viewport; stop the server afterward.
