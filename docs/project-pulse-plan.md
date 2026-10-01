# Project Pulse Dashboard Plan

## Goal and context

Build a polished, accessible, responsive static Project Pulse dashboard that gives contributors a quick, useful view of project name, owner, status, recent activity, priority or risk, and a concise contributor-friendly summary. Keep the implementation browser-native HTML, CSS, and JavaScript with no new dependencies. Serve the static files over HTTP so the page can fetch its JSON data.

The data contract is a JSON object with a top-level `projects` array. Each project must include `name`, `owner`, `status`, `recentActivity`, and `priority`; provide `summary` where useful to help contributors understand the project and how to engage.

## File assignments and responsibilities

| Owner | File or scope | Responsibilities |
| --- | --- | --- |
| Planner | This plan and shared contract | Define the data shape, DOM/CSS hooks, acceptance criteria, dependencies, and work order. |
| Designer | `app/styles.css` | Establish information hierarchy and responsive project cards. Style the `.dashboard` and `.project-card` hooks, including distinct status and priority treatments, readable contrast, visible keyboard focus, considered `border-radius`, and `box-shadow`. Ensure status and priority are not communicated by color alone. |
| Coder | `app/project-data.json` | Supply valid, representative data under the top-level `projects` array, with all required fields and useful summaries. |
| Coder | `app/index.html` | Create the page with the exact title `Project Pulse`, link `app/styles.css` relative to the page, fetch `project-data.json`, and render visible `.project-card` elements containing every required field and available summary. Use semantic markup and safe text insertion (for example, `textContent` rather than interpolating data as HTML). Provide a visible, understandable fetch/data error state. |
| Coder | `.vscode/launch.json` | Add strict, comment-free JSON with a configuration named exactly `Run Project Pulse Dashboard`. Serve `${workspaceFolder}/app` using `python3 -m http.server 5500` and configure `serverReadyAction` to open `http://localhost:%s/index.html`. |
| Orchestrator | Coordination and integration | Coordinate the handoffs, keep assigned file scopes distinct, confirm the shared contract is implemented consistently, and lead final integration and checks. Do not modify unrelated `.vscode/tasks.json`. |

## Dependencies and work order

1. **Phase 1 — sequential contract and design agreement:** The Planner and Orchestrator agree on the JSON fields and the corresponding semantic HTML structure and CSS hooks. In particular, the Coder and Designer must share the `.dashboard` and `.project-card` class names and agree how status and priority values will be represented.
2. **Phase 2 — parallel implementation:** Once the contract and hooks are fixed, the Designer can implement `app/styles.css` while the Coder implements `app/project-data.json` and `app/index.html` in separate file scopes. The Coder’s HTML depends on the agreed data schema and CSS hooks; the stylesheet depends on those same hooks. The launch configuration can also be drafted in parallel because its command and URL are fixed by the plan, but it does not prove the app works until the app files are integrated.
3. **Phase 3 — sequential integration and verification:** The Orchestrator checks that data fields map to rendered content, HTML class hooks match the stylesheet, and the launch configuration serves the correct directory and page. Run the checks below and complete browser inspection only after all assigned files are integrated.

## Edge cases and behavior

- Keep the dashboard usable when `projects` is an empty array, with a visible empty-state message instead of a blank content area.
- Handle a failed fetch, invalid JSON, a missing or wrongly typed `projects` array, and projects with missing required fields explicitly. Show a visible error state rather than silently rendering incomplete cards or failing without explanation.
- Fetch requires HTTP serving; opening `index.html` directly with a `file://` URL is not a supported data-loading path.
- Preserve accessible semantic structure, keyboard operation, visible focus, and sufficient text/background contrast. Never rely on color alone to communicate status or priority.
- Keep `.vscode/launch.json` valid strict JSON with no comments, use the exact configuration name, port `5500`, and URL pattern; do not change `.vscode/tasks.json`.

## Validation and acceptance expectations

Use `.github/workflows/2-step.yml` and `.github/workflows/3-step.yml` as repository-specific acceptance references. Step 2 expects this plan to identify Project Pulse, Designer and Coder responsibilities, all four assigned implementation files, dependencies, parallel ordering, and validation. Step 3 checks the app and launch files, dashboard title and references, rendered card fields, CSS hooks and treatments, required data keys, JSON validity, and launch configuration name and target.

After implementation:

1. Check both JSON files:

   ```sh
   python3 -m json.tool app/project-data.json >/dev/null
   python3 -m json.tool .vscode/launch.json >/dev/null
   ```

2. Serve the app from its required directory and verify both resources respond:

   ```sh
   python3 -m http.server 5500 --directory app
   ```

   In another terminal, run:

   ```sh
   curl --fail http://localhost:5500/index.html
   curl --fail http://localhost:5500/project-data.json
   ```

   Stop the server after these checks.

3. Start the **Run Project Pulse Dashboard** configuration in VS Code and confirm it serves `app` on port `5500` and opens `http://localhost:5500/index.html`.
4. Inspect the rendered dashboard at narrow and wide viewport sizes; verify cards remain readable and responsive, all required fields appear, and empty/error states are visible when exercised.
5. Navigate by keyboard and inspect focus visibility, semantic reading order, contrast, and non-color status/priority cues.
