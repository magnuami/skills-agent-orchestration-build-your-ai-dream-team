# Project Pulse Dashboard Implementation Plan

## Summary

Build a small, static Project Pulse dashboard that helps contributors scan active projects, owners, status, recent activity, priority or risk, and a short contributor-friendly summary. The page should present a polished, accessible, responsive card-based UI and run from the **Run Project Pulse Dashboard** VS Code launch configuration, opening `app/index.html` rather than a directory listing. Keep the implementation deterministic and dependency-free.

## Ordered work

1. **Agree on the display contract.** Designer proposes information hierarchy, card layout, status/priority treatments, responsive behavior, and accessibility requirements. Confirm the shared `.dashboard` and `.project-card` hooks and the project-data fields before implementation.
2. **Create the project dataset.** Coder creates representative, contributor-friendly project entries in `app/project-data.json` using a top-level `projects` array. Every item has `name`, `owner`, `status`, `recentActivity`, and `priority`; include a short summary if the agreed card design uses it.
3. **Implement page structure and rendering.** Coder creates `app/index.html`, references `styles.css` and `project-data.json`, and renders visible project cards from the data. Use semantic, accessible markup and show each project's required fields, with clear loading/error behavior if data cannot be loaded.
4. **Implement the visual system.** Designer owns `app/styles.css`: style the agreed page/card structure with the `.dashboard` and `.project-card` selectors, clear status and priority badges, readable spacing/contrast, rounded corners and shadows, and a responsive layout. Coordinate selectors with the page markup.
5. **Add the runnable preview.** Coder creates `.vscode/launch.json` as strict JSON. Add a configuration named **Run Project Pulse Dashboard**, run `python3 -m http.server 5500` with `cwd` set to `${workspaceFolder}/app`, and configure `serverReadyAction` to open `http://localhost:%s/index.html`.
6. **Integrate and verify.** Orchestrator checks that file contracts agree, runs the checks below, and opens the launch configuration when available to confirm the visible dashboard experience. Resolve any integration or accessibility issues before handoff.

## File assignments

| File | Owner | Scope |
| --- | --- | --- |
| `app/index.html` | Coder | Page title and semantic dashboard structure; link `styles.css`; load `project-data.json`; render each item as a `.project-card` and expose name, owner, status, recent activity, priority, and any agreed summary. Use accessible labels/landmarks and communicate loading or fetch errors. |
| `app/styles.css` | Designer | Polished responsive visual design for the dashboard and cards; provide `.dashboard` and `.project-card` hooks, status/priority distinction, readable typography and spacing, adequate contrast, focus visibility where interactive, `border-radius`, and `box-shadow`. |
| `app/project-data.json` | Coder | Valid JSON with a top-level `projects` array and realistic sample records containing `name`, `owner`, `status`, `recentActivity`, and `priority`; keep values consistent with the UI's labels and badge styles. |
| `.vscode/launch.json` | Coder | Strict JSON; **Run Project Pulse Dashboard** launch configuration serving from `app/` using `python3 -m http.server 5500`, with the working directory `${workspaceFolder}/app` and browser URL `http://localhost:%s/index.html`. |

## Agent responsibilities

- **Orchestrator:** Coordinate handoffs, keep shared data/markup/style/launch contracts aligned, integrate the Designer and Coder outputs, and verify the finished experience.
- **Designer:** Own visual and accessibility decisions and the stylesheet. Deliver a clear information hierarchy, responsive card design, visible status/priority treatment, readable spacing and contrast, and design guidance for matching markup.
- **Coder:** Own page structure/rendering, sample data, and launch configuration. Implement the agreed design hooks, keep the page/data contract consistent, and validate the runnable static app.

## Dependencies and parallel work decisions

- The data schema and agreed field labels precede the renderer: `index.html` must consume the top-level `projects` array and render its defined fields.
- The Designer's layout and class hooks must be shared with the Coder before finalizing markup; CSS and HTML integrate through `.dashboard`, `.project-card`, and badge-related classes.
- After the display contract is agreed, data authoring (`project-data.json`) and stylesheet work (`styles.css`) can proceed in parallel because they have distinct file ownership. The Coder can also create the launch configuration in parallel with page/data work; it only depends on the known app path and agreed server command/port.
- Do not have Designer and Coder edit the same file concurrently. The Coder completes/adjusts markup against the Designer's CSS hooks during integration; the Orchestrator resolves mismatches.
- Final integration and browser verification depend on all four files being present and mutually consistent.

## Validation expectations

- Parse `app/project-data.json` and `.vscode/launch.json` with `python3 -m json.tool`.
- Confirm the data has a top-level `projects` array and that each item includes `name`, `owner`, `status`, `recentActivity`, and `priority`.
- Inspect `app/index.html` for the exact **Project Pulse** title, stylesheet and JSON references, visible data-driven project cards with the `project-card` class, and required displayed fields.
- Inspect `app/styles.css` for `.dashboard`, `.project-card`, responsive behavior, `border-radius`, and `box-shadow`; review keyboard/focus behavior where relevant, semantic/accessibility structure, and text/background contrast.
- Inspect `.vscode/launch.json` for strict JSON, the exact **Run Project Pulse Dashboard** name, `cwd` targeting `app/`, the required `python3 -m http.server 5500` command, and the browser URL ending in `/index.html`.
- Run `bash scripts/validate-exercise.sh` for repository-level validation. Then use **Run Project Pulse Dashboard** to confirm the browser opens the dashboard (not a directory listing), cards and fields are visible, and the layout remains usable at narrow and wide viewport sizes. Stop the preview server after the smoke test.

## Risks and open questions

- Fetching `project-data.json` may fail when opening the HTML directly via `file://`; use the planned local HTTP launch configuration and verify the browser preview rather than relying on direct-file opening.
- Agree on the permitted status and priority vocabulary before styling badges; keep sample values consistent so the visual treatment remains understandable.
- The brief requests a short contributor-friendly summary but the required data schema does not mandate a `summary` field. Decide during design whether to add it consistently to all sample records or compose a concise summary from required fields.
- Confirm the Codespace/VS Code environment supports the configured Python HTTP server and browser-opening action; if not, retain the deterministic launch target and report the environment limitation.
