# Project Pulse Dashboard Implementation Plan

## Summary

Build a small, static Project Pulse dashboard for contributors. The first view should clearly present active projects, ownership, current status, recent activity, priority or risk, and concise contributor-facing context through polished project cards, status badges, readable spacing, and responsive behavior.

The implementation should remain dependency-light and use the existing repository's static-app conventions. The dashboard must load from `app/index.html`, read project records from `app/project-data.json`, use `app/styles.css` for presentation, and be launchable through a VS Code configuration that serves the `app/` directory and opens `index.html`.

## Ordered implementation steps

### 1. Establish the data contract and sample content

**Owner:** Coder  
**File assignment:** `app/project-data.json`

- Create a strict JSON document with a top-level `projects` array.
- Include representative projects with the required fields:
  - `name`
  - `owner`
  - `status`
  - `recentActivity`
  - `priority`
- Use a small, readable dataset that exercises multiple statuses and priority levels.
- Keep values suitable for direct rendering in the dashboard and avoid relying on undefined fields.

### 2. Define the information hierarchy and visual direction

**Owner:** Designer  
**File assignment:** Design guidance for `app/index.html` and `app/styles.css`; implementation may be made directly in `app/styles.css` only if explicitly assigned during execution.

- Establish a clear page title and supporting dashboard context.
- Define the project-card structure: project name, owner, status badge, priority/risk treatment, recent activity, and contributor summary.
- Specify accessible color and contrast choices for status and priority states.
- Define responsive behavior for narrow and wide viewports, including card stacking and readable spacing.
- Reserve deterministic hooks such as `.dashboard` and `.project-card` for validation and future iteration.

### 3. Implement the dashboard document structure

**Owner:** Coder  
**File assignment:** `app/index.html`

- Create a semantic static page that presents the Project Pulse heading and project-card region.
- Load `styles.css` and the project data needed by the page.
- Render the required project information without exposing a server directory listing.
- Use accessible headings, landmarks, labels, and status text; do not communicate status through color alone.
- Keep the markup compatible with the data contract from step 1.

### 4. Implement the visual system and responsive layout

**Owner:** Designer, with Coder integration  
**File assignment:** `app/styles.css`

- Implement the approved hierarchy using polished typography, spacing, rounded cards, shadows, status badges, and priority indicators.
- Add responsive layout rules so cards remain readable on small screens and make effective use of larger screens.
- Include visible focus states and sufficient contrast.
- Keep selectors and class names aligned with the structure in `app/index.html`.

### 5. Configure the runnable preview

**Owner:** Coder  
**File assignment:** `.vscode/launch.json`

- Create strict JSON with a deterministic launch configuration named **Run Project Pulse Dashboard**.
- Serve from `${workspaceFolder}/app`.
- Set the launch working directory (`cwd`) to `${workspaceFolder}/app`.
- Open `index.html` directly so the dashboard appears instead of a directory listing.
- Use the repository's available local serving/debugging approach rather than introducing unnecessary dependencies.

### 6. Integrate and verify the complete dashboard

**Owner:** Orchestrator, with Designer and Coder reports

- Confirm all assigned files exist and agree on paths, field names, selectors, and launch behavior.
- Resolve integration issues without expanding either specialist's file scope unnecessarily.
- Confirm the finished result satisfies the Project Pulse brief before handing it back to the learner.

## File assignments

| File | Primary owner | Responsibility |
| --- | --- | --- |
| `app/index.html` | Coder | Semantic dashboard shell, project-card markup, data/style loading, accessible content structure |
| `app/styles.css` | Designer | Visual hierarchy, card styling, badges, spacing, responsive layout, focus and contrast states |
| `app/project-data.json` | Coder | Top-level `projects` data contract and representative project records |
| `.vscode/launch.json` | Coder | **Run Project Pulse Dashboard** configuration, `app/` working directory, direct `index.html` launch |

The Designer should provide layout and accessibility decisions before the Coder finalizes the HTML structure when those decisions affect markup. The Designer should not modify the data or launch configuration. The Coder should not change design-only behavior outside the assigned implementation files.

## Dependencies

- `app/index.html` depends on the field names and value conventions established in `app/project-data.json`.
- `app/index.html` and `app/styles.css` depend on an agreed information hierarchy and shared class names.
- `.vscode/launch.json` depends on `app/index.html` existing at the configured path and on the chosen local server/debug command being available in the environment.
- Integration validation depends on all four files being present.
- No external package installation should be required unless the repository's existing preview tooling proves insufficient.

## Parallel work decisions

### Work that can run in parallel

- The Designer can define the visual hierarchy, accessibility requirements, responsive rules, and CSS naming conventions while the Coder creates the data contract.
- After the data contract and shared structure are agreed, the Designer can work on `app/styles.css` while the Coder works on `.vscode/launch.json`.
- Repository inspection and validation preparation can occur in parallel with specialist implementation when they do not modify the same files.

### Work that must be sequential

- The data shape must be settled before the Coder finalizes data-driven markup in `app/index.html`.
- The Designer's structural decisions and required hooks must be agreed before final HTML/CSS integration, because both files share class names and layout assumptions.
- `.vscode/launch.json` finalization must follow confirmation of the app path and entrypoint so it opens `index.html` rather than a directory.
- End-to-end validation must follow completion of all four files.

The Orchestrator should keep overlapping edits sequential: no two agents should modify the same file in the same phase.

## Edge cases and risks

- Missing or misspelled required JSON fields can produce incomplete cards; validate every record against the contract.
- Unknown status or priority values should have a readable fallback treatment rather than disappearing or becoming inaccessible.
- Long project names, owner names, or activity text should wrap without breaking the card grid.
- Empty project data should produce a useful empty state instead of a blank page if the implementation renders data dynamically.
- The dashboard must remain understandable without color vision and usable with keyboard navigation.
- A launch configuration that serves the repository root may show a directory listing; explicitly verify its working directory and URL.
- Strict JSON files must not contain comments or trailing commas.
- If the chosen browser/preview command is unavailable, surface the limitation explicitly rather than silently weakening the launch configuration.

## Validation expectations

1. Run the repository's existing validation script, `scripts/validate-exercise.sh`, and address any Project Pulse-related failures.
2. Parse `app/project-data.json` with a strict JSON parser and verify it contains a non-empty top-level `projects` array whose records include all five required fields.
3. Inspect `app/index.html` to confirm it references `styles.css`, uses the expected dashboard/card hooks, and presents semantic headings and accessible status/priority text.
4. Inspect `app/styles.css` to confirm `.dashboard` and `.project-card` exist, project cards have clear spacing and visual treatment, and responsive rules cover narrow viewports.
5. Parse `.vscode/launch.json` as strict JSON and verify it includes the exact launch name **Run Project Pulse Dashboard**, uses `${workspaceFolder}/app` as `cwd`, and opens `index.html`.
6. Start the configured preview or equivalent local server and verify the root dashboard URL renders `app/index.html`, not a directory listing.
7. Perform a browser-level smoke check at desktop and narrow viewport widths: cards render, status and priority remain legible, long text wraps, and keyboard focus is visible.

## Open questions

- Which existing local server/debug adapter is available for the VS Code launch configuration should be confirmed before implementation. The plan assumes the repository or environment already provides one.
- If the dashboard is intended to be purely static, data rendering can be implemented with static markup plus the JSON as the source fixture; if live JSON loading is required, the Coder should use a small explicit script while preserving the four-file scope.
