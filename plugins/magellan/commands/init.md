---
description: Bootstrap voyage structure with example files
scope: project
---

Initialize the voyage directory structure with example files.

1. Check if `voyages/` directory exists. If it does, ask the user if they want to overwrite existing files.
2. Create `voyages/` directory.
3. Write `voyages/chart.yaml`:
   ```yaml
   wikipedia: https://en.wikipedia.org
   ```
4. Write empty `voyages/maps.yaml` (or initialize with empty object `{}`).
5. Write `voyages/getaria.md`:
   ```markdown
   # Destination: Getaria

   ## Plan

   - Navigate to $wikipedia
   - Search for Magellan
   - Click Elcano link
   - Click Getaria link
   - Click first image
   ```
6. Tell the user "Voyage structure initialized. Run `/voyage getaria` to test."
