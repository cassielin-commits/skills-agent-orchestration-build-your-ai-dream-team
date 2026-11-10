# Project Pulse Dashboard Implementation Plan

## Summary

Build Mona’s lightweight, static Project Pulse dashboard so contributors can scan projects, owners, status, recent activity, priority or risk, and a short summary. Use plain HTML, CSS, and JSON in the existing `app/` directory, with a VS Code launch configuration that serves that directory and opens `index.html`.

The repository has no existing app implementation, package manifest, or launch configuration. The brief and Step 3 workflow define the expected files and checks; avoid adding a framework or build system.

## Ordered implementation steps

### 1. Confirm requirements and agree on the UI/data contract

- **Orchestrator:** Use this plan to delegate work and maintain non-overlapping file ownership.
- **Designer:** Define the page hierarchy, responsive card layout, typography, status/priority badge treatment, accessibility approach, and the CSS hooks the markup will use. At minimum, coordinate `.dashboard` and `.project-card`.
- **Coder:** Confirm the data contract from the brief: a top-level `projects` array with `name`, `owner`, `status`, `recentActivity`, and `priority`. Include a short contributor-friendly `summary` field as well, because the brief asks for one.
- **Files:** No file changes in this step.
- **Dependency:** Complete the shared structure/class-hook agreement before implementing the HTML and stylesheet in parallel.

### 2. Implement the data and launch configuration

- **Coder owns `app/project-data.json`:** Add multiple clearly identified example projects with all required fields and a short summary. Keep field names consistent and values human-readable. Do not imply that example data is confirmed real team data.
- **Coder owns `.vscode/launch.json`:** Create strict JSON with no comments and a configuration named **Run Project Pulse Dashboard**. Use `python3 -m http.server 5500`, set `cwd` to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`. Choose a VS Code-supported launch type for running that command; verify its required debugger support is available in this repository’s Codespace.
- **Dependency:** The data shape is determined in Step 1. The launch configuration can be implemented independently of the UI files.

### 3. Implement the frontend in parallel

Once Step 1 establishes the markup and CSS hook contract:

- **Coder owns `app/index.html`:** Set the exact page title to “Project Pulse”; reference `styles.css` and `project-data.json`; provide semantic page structure and an accessible loading, error, and empty-project state; fetch and render the project data as visible `.project-card` elements. Display each project’s name, owner, status, recent activity, priority, and summary.
- **Designer owns `app/styles.css`:** Implement the agreed visual hierarchy and responsive layout, including `.dashboard` and `.project-card`. Use readable spacing, contrast, status/priority treatments, `border-radius`, and `box-shadow`. Ensure badge meaning is conveyed by text, not color alone, and preserve readable layouts on narrow screens.
- **Dependency:** HTML and CSS are parallel after the class contract is agreed; they must be integrated against the same hooks.
- **Security/robustness:** Render JSON values as text rather than injecting them as HTML.

### 4. Integrate and validate

- **Orchestrator:** Review all four deliverables together, confirm the frontend loads the local JSON over HTTP, and resolve any mismatch between the markup, CSS hooks, and launch settings.
- **Coder and Designer:** Fix issues only within their assigned files. Coordinate any change that affects the shared markup/CSS contract.
- **Dependency:** Run this step after Steps 2 and 3 are complete.

## File assignments

| File | Owner | Responsibility |
|---|---|---|
| `app/index.html` | Coder | Semantic dashboard structure, exact title, stylesheet/data references, JSON loading, project-card rendering, and loading/error/empty states. |
| `app/styles.css` | Designer | Responsive visual design, accessibility-oriented contrast and spacing, status/priority treatment, and agreed CSS hooks. |
| `app/project-data.json` | Coder | Top-level `projects` array; required project fields and contributor-friendly `summary` values. |
| `.vscode/launch.json` | Coder | Strict JSON launch configuration named **Run Project Pulse Dashboard** that serves `app/` and opens `index.html`. |

The Designer provides visual and accessibility guidance and owns the stylesheet. The Coder owns markup, data, and runnable-app configuration. The Orchestrator coordinates delegation and integration; the Planner’s role is to define this plan and its dependencies.

## Dependencies and parallel work

- **Must be sequential:** Agree on the UI/class-hook contract before HTML and CSS implementation; integrate and run the complete dashboard after all assigned files exist.
- **Can run in parallel:** After the shared contract is agreed, Designer can implement `app/styles.css` while Coder implements `app/index.html`. Coder can also create the data and launch files independently of CSS; JSON field names are already specified by the brief.
- Keep file ownership separate to avoid simultaneous edits to the same file. Coordinate any changes to the agreed HTML/CSS interface.

## Edge cases and risks

- Empty `projects` array: show a useful no-projects message rather than a blank dashboard.
- Fetch failure or invalid JSON: show a visible, understandable error rather than silently rendering nothing.
- Missing or empty field values: avoid broken markup; display a neutral fallback where appropriate.
- Untrusted text values: insert as text, not executable HTML.
- Narrow viewport and long names/activity: cards should reflow without horizontal page overflow and text should remain readable.
- Launch port `5500` already occupied: report the startup failure and stop or resolve the conflicting server before retrying.
- Launch/debugger type unavailable: the devcontainer currently lists Copilot and GitHub Actions extensions, not a Python debugger. Do not assume a debugger extension exists; verify the selected launch type in Codespaces and surface any required extension/setup decision.
- `python3 -m http.server` is for local preview, not production hosting.

## Validation expectations

1. Confirm all four assigned files exist.
2. Parse `app/project-data.json` and `.vscode/launch.json` as JSON (for example, `python3 -m json.tool <file>`).
3. Confirm the JSON has a top-level `projects` array and each example includes `name`, `owner`, `status`, `recentActivity`, `priority`, and `summary`.
4. Confirm the HTML references `styles.css` and `project-data.json`, and renders visible project cards with status, recent activity, and priority.
5. Confirm the stylesheet defines `.dashboard` and `.project-card`, includes `border-radius` and `box-shadow`, and supports a narrow viewport.
6. In VS Code, run **Run Project Pulse Dashboard**. Verify that it serves from `app/`, opens `http://localhost:<port>/index.html`, and displays the dashboard rather than a directory listing. Stop the server after testing.
7. Treat the Step 3 GitHub workflow checks as baseline file/content/JSON checks; they do not prove the browser preview works. Perform the launch and visual checks manually.

## Open questions

- What are the team’s real project names, owners, statuses, activity updates, priorities, and summaries? Use clearly labeled examples until Mona supplies confirmed values.
- Which launch configuration type is supported in the target Codespace without installing an additional extension? Verify this during implementation; the required command, working directory, and browser URL are fixed by the brief.
- Are status and priority labels governed by an existing team vocabulary? If not, choose a small, consistent set and keep labels explicit in the UI.
