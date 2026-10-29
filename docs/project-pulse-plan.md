# Project Pulse implementation plan

## Summary

Build Mona's lightweight, static Project Pulse dashboard so contributors can scan active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The repository brief names three app files and a VS Code launch configuration; the exercise outline and build step further require project cards, accessible and responsive presentation, JSON-backed project data, and a launch target that opens the dashboard rather than a directory listing.

Repository evidence: `.github/project-pulse-brief.md` defines the required fields and files; `.github/steps/3-step.md` specifies visible project cards, CSS hooks, the launch command and URL; `.github/agents/designer.agent.md`, `coder.agent.md`, and `orchestrator.agent.md` define agent scopes and coordination. There is no existing `app/` implementation or app framework, package manifest, or app-specific test suite in the repository. Keep the implementation static and avoid adding dependencies or files outside the assigned scope unless the Orchestrator explicitly approves a change.

## Ordered phases and file assignments

| Phase | Owner and responsibilities | Assigned files | Output |
|---|---|---|---|
| 1. Agree on the experience and data assumptions | **Designer** proposes information hierarchy, card layout, status/priority treatment, responsive behavior, readable spacing, and accessible semantics. **Coder** confirms a feasible static implementation and identifies how the page will consume the JSON without adding an unapproved file. Orchestrator resolves scope questions and records decisions for both agents. | No implementation files; design direction is a handoff to Coder. | Shared decisions for the page structure, responsive/accessibility expectations, and data rendering. |
| 2. Create the page and its styling | **Coder** creates the semantic page structure and connects it to the stylesheet and project data. **Designer** guides and reviews visual hierarchy, accessible presentation, and responsive styling; Designer owns design decisions, not the app's data or launch configuration. | `app/index.html` — Coder; `app/styles.css` — Coder implementing Designer's direction. | A clearly titled Project Pulse dashboard with a `.dashboard` container and visible `.project-card` elements, styled as a polished, readable, responsive UI. |
| 3. Supply the project data and render it | **Coder** defines the documented JSON shape and ensures the page displays the supplied project fields. Coordinate with Designer on how longer summaries, activity, and priority/status labels remain scannable. | `app/project-data.json` — Coder; `app/index.html` — Coder for loading/rendering; `app/styles.css` — Coder for field presentation. | Valid JSON with a top-level `projects` array and the required fields `name`, `owner`, `status`, `recentActivity`, and `priority`; rendered project cards show at least status, activity, and priority. |
| 4. Add the runnable preview | **Coder** creates strict JSON launch configuration and checks the requested working directory, command, and browser target. | `.vscode/launch.json` — Coder. | A **Run Project Pulse Dashboard** configuration serving from `${workspaceFolder}/app` with `python3 -m http.server 5500` and opening `http://localhost:%s/index.html` once the server is ready. |
| 5. Integrate and validate | **Coder** runs structural checks and the preview; **Designer** reviews the rendered hierarchy, accessibility, and responsive behavior; **Orchestrator** checks that the files agree, dependencies are satisfied, and unresolved assumptions are reported. | All four implementation files, read-only during review except by their assigned owner if fixes are needed. | A coherent dashboard that launches to `index.html`, with validation results and limitations reported. |

## Agent responsibilities and file ownership

- **Orchestrator:** Use this plan to set phases, hand out the scopes above, keep overlapping page/data work coordinated, and review the integrated output. Do not have Designer and Coder edit the same file concurrently.
- **Designer:** Define and review visual and interaction guidance: content hierarchy, accessible semantics/contrast, readable spacing, project-card/status/priority affordances, and responsive layout. Designer does not own the data schema, implementation, or launch file.
- **Coder:** Owns all four requested implementation files and their integration. Implement the agreed design, connect the page to the JSON, keep launch JSON strict and deterministic, and validate the result. The Coder agent specifically calls for `cwd` `${workspaceFolder}/app`, opening `index.html`, and no JSON comments.

## Dependencies and execution order

- Phase 1 precedes implementation: Coder needs a stable design direction and decisions about the content/data presentation.
- In Phase 2, the HTML structure and stylesheet have a design dependency but can be drafted in parallel only after Designer's direction is explicit and file ownership is non-overlapping. Coder should coordinate class names and semantic structure across the two files.
- Phase 3 depends on the agreed data shape. The JSON file and the page's rendering can be authored in parallel after the shape and rendering approach are agreed, but must be integrated and tested together. CSS can be refined alongside those tasks once the field presentation is known.
- Phase 4 is independent of the dashboard's visual implementation and can be prepared in parallel after the launch requirements are fixed. Final launch verification depends on `app/index.html` existing at the configured target.
- Phase 5 is sequential after all files are integrated; visual review and runtime validation should be performed against the same final state. Any fixes return to the owning phase/file, followed by the affected checks again.
- **Parallel work:** Designer's design guidance/review can run alongside Coder's data and launch setup after scope is agreed; Coder can also develop separate assigned files in parallel if their interfaces (HTML hooks, JSON keys, launch target) are first agreed.
- **Sequential work:** Design/data contract before integration; page and data integration before end-to-end checks; all implementation before final runtime and review. Avoid concurrent edits to `app/index.html` or `app/styles.css`.

## Validation expectations

1. Parse `app/project-data.json` and confirm `projects` is an array whose project records include `name`, `owner`, `status`, `recentActivity`, and `priority`. Check that the values used in the page correspond to this data.
2. Parse `.vscode/launch.json` as JSON with no comments. Confirm the configuration is named **Run Project Pulse Dashboard**, serves from `app/`, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html` after server readiness.
3. Inspect `app/index.html` for the exact **Project Pulse** title, references to `styles.css` and `project-data.json`, and visible `.project-card` elements that display project name, owner, status, recent activity, priority, and a short contributor-friendly summary where available.
4. Inspect `app/styles.css` for `.dashboard` and `.project-card`, clear status/priority treatments, readable contrast and spacing, rounded card treatment (`border-radius`, `box-shadow`), and responsive layout. Review keyboard/screen-reader semantics and narrow viewport behavior.
5. In VS Code, run **Run Project Pulse Dashboard** and verify the browser shows the dashboard at the `index.html` URL rather than a server directory listing. Stop the preview server afterward. If browser-based validation is unavailable, report that limitation rather than claiming the runtime check passed.
6. The repository's Step 3 workflow checks required files, phrases, and JSON syntax; it does not prove that the page renders or the launch configuration runs. Use the manual preview check in addition to those static checks. `scripts/validate-exercise.sh` validates template/exercise consistency and does not replace app-specific checks.

## Assumptions and open questions

- The brief does not supply real project records. Do not present invented owners, activity, or risk as Mona's real project data; confirm the sample-data source or label any examples clearly as illustrative.
- The brief requires a “short contributor-friendly summary” for the dashboard, but the specified JSON fields do not include a summary field. Confirm whether to add a `summary` property or derive the summary from existing fields; do not silently treat activity as a summary.
- Status and priority vocabularies, ordering/filtering, and empty/error/loading states are not specified. Keep labels and behavior simple and consistent with approved data; clarify whether filtering or interactive controls are in scope before adding them.
- The page must show cards from JSON and reference `project-data.json`, but no separate JavaScript file is among the requested deliverables. Coder should use an implementation approach contained in the assigned files unless the Orchestrator approves an additional file.
- The launch details above follow `.github/steps/3-step.md`. Confirm Python 3 and the chosen VS Code launch/debug support are available in the target Codespace when testing; no additional server or package dependency is specified.
