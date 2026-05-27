# P6 Cost Writer — How to Use This Every Time

A browser tool that loads **cost onto Primavera P6 activities** by Activity ID and produces a P6-importable XLSX. Runs entirely in your browser — no install, no data leaves your machine. Open `P6_Cost_Writer` in any browser.

Tested against Primavera **P6 Professional 23**. Currency in the sample project: **Saudi Riyal**.

---

## Part A — Load cost onto the schedule (one time, at the start)

**Step 1 — Export your project from P6**
File → Export → Spreadsheet (XLSX) → tick **Expenses** → export.
This gives you P6's own file (it contains the hidden `USERDATA` sheet). Using P6's own file is what avoids the *"user preferences could not be found"* import warning.

**Step 2 — Open the tool, load activities**
Open `P6_Cost_Writer.html`. In Step 1 of the tool, upload that P6 export and pick the **TASK** sheet (or paste your activity list). Map the **Activity ID** column.

**Step 3 — Paste your cost list**
In Step 2, paste your cost data — one row per activity: `Activity ID` then the amount. Tab- or comma-separated. Headerless is fine.

```
Hail-OHTL.GD-1010   3000
Hail-OHTL.GD-1005   30000
Hail-OHTL.GD-1006   2212220
```

**Step 4 — Match (the accuracy checkpoint)**
Click **Match**. Confirm **matched count, 0 unmatched, and the correct total** before going further.
If anything is unmatched → the Activity IDs don't line up exactly. The ID is the `GD-…` value, **not** the WBS code (`Hail OHTLu-2.2.2`). Fix the IDs and re-match.

**Step 5 — Inject (Method A, recommended)**
Under **Method A**, upload the *same* P6 export from Step 1. Click **Inject cost & download** → you get `P6_export_cost_loaded.xlsx`.
All original sheets (incl. `USERDATA`) are preserved → no import warning.

**Step 6 — Import into P6**
Always import into a **copy** of the project first.
File → Import → Spreadsheet → XLSX → select the file → map to **PROJCOST** → Finish.
P6 recognises its own file → no warning. Cost lands as **activity expenses** (visible on each activity's **Expenses** tab; rolls up into **Budgeted Expense Cost**).

**Verify:** the project-summary **Budgeted Expense Cost** equals your matched total.

---

## Part B — Weekly progress tracking & S-curve

> Weekly tracking happens **in P6**, not in the tool. The tool loads cost (Part A) and draws the curve from your weekly export (below).

**Each week, in P6:**
1. Set the **Data Date** to this week's cutoff.
2. Update **% complete** / actual dates / actual cost on progressed activities.
3. **Schedule** (F9) to recalculate.

**To pull the S-curve for the weekly report:**
1. Export the schedule to XLSX including: **Activity ID, Start, Finish, Baseline (BL) Start/Finish, Actual Start/Finish, Physical % Complete, Budgeted Total Cost, Actual Total Cost**.
2. Load that file into the tool — it plots the cumulative **planned vs actual** S-curve.

---

## Key column meanings (so the numbers aren't confusing)

| Column | What it means |
|---|---|
| **Budgeted Expense Cost** | The cost you loaded with this tool (PROJCOST / expenses) |
| **Actual Total Cost** | Cost recorded as *already spent* — grows as you record progress. NOT resource-vs-expense |
| **Total / At Completion Cost** | Budgeted + actual rollup (full planned cost) |

Resource cost ≠ expense cost. This tool loads **expenses** (no resource dictionary needed). To check for resource loading, open an activity → **Resources** tab. Empty = no resources assigned; all cost is expense-based.

---

## Quick rules

- **CSV cannot be imported into P6** — always use XLSX.
- **Method A (round-trip)** = inject into P6's own export → no warning. Preferred.
- **Method B (generate)** = tool builds the file from scratch → P6 may warn "user preferences not found"; click **Yes** on a project copy. Use only when you don't have a P6 export handy.
- Always load into a **project copy** first.
- Accuracy = exact **Activity ID** match. The tool never silently drops rows; it shows matched vs unmatched every time.
- First use needs internet (chart + spreadsheet libraries load from CDN).

---

*Loads cost. Tracking lives in P6. Bring real Activity IDs and matching costs, confirm the match count, import to a copy.*
