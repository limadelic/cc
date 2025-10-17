---
description: Launch an exploratory test voyage with elkano at the helm
argument-hint: "[file-name]"
scope: project
---

Open with “Preparing for the `$1` voyage…”, announce each `Destination:` before executing it, keep Elkano at the helm for every browser maneuver, and narrate progress in a pilot’s voice.

**Ground rules**
- You MUST invoke `/a elkano …` for every browser-related action. No direct Playwright tool usage.
- Do not attempt to "sniff" page state by yourself—ask `elkano` to gather snapshots or data.
- Keep a running Todo list so the user can track the scenario.
- Never show file operations (reading maps.yaml, chart.yaml, etc.) to the user. Work with these files silently in the background.

**Workflow**
1. Compute `voyage_path = voyages/$1` if the argument ends with `.md`, otherwise `voyages/$1.md`. If it doesn't exist, ask for a valid voyage file name.
2. Read `voyage_path` and split on lines that start with `Destination:`; bullets belong to the current destination, or to a single `Destination: Default` if none exist. Ignore everything else.
3. Load vars from `voyages/chart.yaml` for substitutions. Load (or initialize if missing) `voyages/maps.yaml` silently - never mention file errors to the user.
4. Tell the user "Preparing for the `$1` voyage…".
5. Create a parent todo `Voyage $1` and child todos for each destination with their stored steps.

**Voyage execution**
6. For each destination in order:
   - Announce it (e.g., "Destination: Warmup—setting course now.") and mark the todo `in_progress`.
   - For each step in that plan:
     1. Expand chart variables in the step text (supports nested keys like `$env.user`).
     2. Look up any element references in `voyages/maps.yaml` and expand them inline (e.g., if step says "Click applicants link" and maps.yaml has `applicants_link`, expand to the actual locator). Never mention maps.yaml in the elkano prompt.
     3. Issue the browser action via `/a elkano …` with the expanded locator.
     4. If Elkano returns new or updated mapping entries, write them to `voyages/maps.yaml` immediately.
     5. On success continue; on failure mark the destination todo `blocked`, report the issue, and end the voyage.
   - When the steps complete, mark the destination todo `completed` and move on.

**Wrap-up**
- If every destination completed, mark the parent todo `completed` and summarize the voyage (destinations visited, notable outputs, new refs stored).
- If the voyage stopped early, leave remaining destination todos `pending`, mark the parent todo `blocked`, and surface the failure reason plus recommended next action.
