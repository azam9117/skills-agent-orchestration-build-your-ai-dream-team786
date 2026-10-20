# Project Pulse implementation plan

## Goal

Build Mona’s Project Pulse as a small, static dashboard that helps contributors quickly understand project ownership, status, recent activity, priority or risk, and a short project summary. The dashboard should use accessible, responsive project cards and be previewable from VS Code.

## File ownership

| File | Owner | Responsibilities |
| --- | --- | --- |
| `app/index.html` | **Coder** | Create the page with the exact title **Project Pulse**. Link `styles.css`, load `project-data.json`, and render each project as a `.project-card` showing its name, owner, status, recent activity, priority, and summary. Use semantic, accessible markup. |
| `app/styles.css` | **Designer** | Define the visual hierarchy, typography, spacing, responsive layout, card and badge treatments, and accessible contrast. Include `.dashboard` and `.project-card`; use rounded corners and shadows as appropriate. Return design decisions and validation recommendations to the Orchestrator. |
| `app/project-data.json` | **Coder** | Add a top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a concise contributor-friendly `summary` to satisfy the brief. |
| `.vscode/launch.json` | **Coder** | Add strict JSON for **Run Project Pulse Dashboard**. Serve from `${workspaceFolder}/app` with `python3 -m http.server 5500`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. |

## Responsibilities

- **Designer:** Owns the visual and accessibility decisions and the implementation of `app/styles.css`. Coordinate the CSS hooks and interaction expectations with the Coder before work begins; do not edit the Coder-owned files.
- **Coder:** Owns `app/index.html`, `app/project-data.json`, and `.vscode/launch.json`. Implements the agreed page/data interface, connects the assets, and makes the dashboard runnable through the specified launch configuration.
- **Orchestrator:** Assigns these bounded scopes, resolves questions about the shared interface before implementation, integrates the results, and reports validation outcomes.
- **Planner:** Provides this implementation plan; does not implement the dashboard.

## Ordered work and dependencies

1. **Agree on the interface before implementation.** Confirm the JSON field names, the required `.dashboard` and `.project-card` hooks, and the launch configuration’s name, working directory, command, and target URL. The brief and exercise steps already specify these core requirements.
2. **Work in parallel after the interface is agreed.**
   - Designer implements `app/styles.css` using the agreed hooks.
   - Coder creates `app/project-data.json`, `app/index.html`, and `.vscode/launch.json`.
   - These assignments are file-disjoint. The shared contract lets the page and stylesheet integrate without either agent editing the other’s files.
3. **Integrate and review sequentially.** The Orchestrator checks that the HTML’s selectors and displayed fields match the stylesheet and JSON, then reviews the launch configuration and resolves any mismatches with the relevant owner.
4. **Validate the integrated dashboard.** Run the checks below, then launch the preview and confirm it opens the page rather than a directory listing.

## Parallel and sequential decisions

- **Parallel:** Designer’s stylesheet work and Coder’s HTML, data, and launch-file work can proceed together once the interface contract is clear. Each agent has exclusive ownership of its assigned files.
- **Sequential:** Contract-setting precedes implementation; integration review and end-to-end preview follow both workstreams. Any change that alters shared field names or CSS hooks must be communicated before the affected files are revised.
- **Avoid:** Assigning both agents the same file, or changing the data schema or CSS hooks during integration without coordinating the corresponding owner.

## Validation expectations

- Parse the data and launch files as JSON, for example:
  - `python3 -m json.tool app/project-data.json`
  - `python3 -m json.tool .vscode/launch.json`
- Confirm the data has a top-level `projects` array and every project has `name`, `owner`, `status`, `recentActivity`, and `priority`. Confirm summaries are present if following the brief’s contributor-friendly summary requirement.
- Review `app/index.html` for the exact title **Project Pulse**, stylesheet and data references, and visible project cards that show the required project details.
- Review `app/styles.css` for `.dashboard`, `.project-card`, readable contrast, and responsive behavior; confirm rounded-card and shadow treatments are present.
- Confirm `.vscode/launch.json` is strict JSON, names the configuration **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app` as its working directory, runs `python3 -m http.server 5500`, and opens `http://localhost:%s/index.html` through `serverReadyAction`.
- Start **Run Project Pulse Dashboard** in VS Code and verify that the browser displays the Project Pulse UI with project data, not a directory listing. Stop the preview server after checking it.
- The repository has no app-specific test suite. `scripts/validate-exercise.sh` is aimed at validating the starter template and expects learner output files to be absent, so it is not an appropriate post-implementation acceptance test once those files exist.

## Assumptions and open questions

- The dashboard is a static frontend with no backend, external service, or new dependency.
- The brief does not supply real project records or define status and priority vocabularies. Use clearly illustrative sample data and consistent, readable labels until Mona provides the real dataset and terminology.
- The brief asks for a short summary, while its required JSON-field list does not include one. This plan includes `summary` as an additional field to meet both expectations.
- The preview assumes Python 3 is available in the Codespace and VS Code can open the configured local URL externally.
