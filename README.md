# Authorization Units Calculator

A small, accessible, single-file web app to calculate authorization units and visits used in clinical or billing workflows.

The app provides two modes:
- **Units from POC Date Range** — calculate total weeks, visits and units given a Plan of Care (POC) start/end date, visits per week and visit duration.
- **Visits from Units** — calculate the total number of visits that can be scheduled given a number of units and visit duration.

Developed by Luis R.

---

## Files
- `index.html` — the entire app (HTML, CSS, and JavaScript) in one file. Open it in a browser; no build step required.

---

## Quick start

1. Clone or download the repository.
2. Open `index.html` in any modern browser (Chrome, Firefox, Edge, Safari).

No server or dependencies required — the app is static.

---

## Usage

There are two modes (use the segmented control at the top to switch):

1. Units from POC Date Range
   - Start Date and End Date: enter dates in **MMDDYY** format (six digits). The input shows a friendly mask as you type (e.g., `081825` → `08/18/25`).
   - Visits per Week: integer between 1 and 5.
   - Duration per Visit: choose 30, 45, or 60 minutes.
   - Click **Calculate** to see:
     - Total weeks (rounded)
     - Total visits
     - Total units

2. Visits from Units
   - Number of Units Given: numeric value (e.g., `36`).
   - Duration per Visit: choose 30, 45, or 60 minutes.
   - Click **Calculate** to see the total number of visits that the units represent.

---

## How calculations work

- Weeks:
  - Computed as Math.round(abs(endDate - startDate) / (7 days in milliseconds))
  - Note: this rounds to the nearest week. See Limitations for alternatives.

- Units:
  - Units are calculated on a 15-minute unit basis.
  - Units per visit = duration (minutes) / 15
  - Total units = visits_per_week × units per visit × weeks

- Visits from units:
  - Visits = round(units / (duration / 15))

- Two-digit year conversion:
  - The app converts `YY` to a full year using:
    - `YY ≤ 49` → `2000 + YY`
    - `YY > 49` → `1900 + YY`
  - Examples:
    - `25` → `2025`
    - `85` → `1985`

---

## Input validation & UX behavior

- Date format: accepts only 6 digits (MMDDYY). The UI masks input to `MM/DD/YY`.
- Month must be between `01` and `12`.
- Day must be valid for the chosen month (including February and leap-year checks).
- Leap year handling: February 29 is only allowed when the computed full year is a leap year.
- Visits per week must be an integer between `1` and `5`.
- Duration options are limited to `30`, `45`, and `60` minutes.
- Error messages are shown inline and fields are highlighted when invalid.
- Pressing Enter while inside a date field will trigger the active mode's calculation.

---

## Accessibility

- Segmented control supports keyboard navigation (ArrowLeft / ArrowRight).
- Buttons use appropriate `role` and `aria-selected` attributes.
- Inline error messages are placed in elements with `aria-live="polite"` so screen readers are notified.
- Inputs are focusable and show visible focus styles.

---

## Styling & Customization

- Styles are defined using CSS variables at the top of the file, making quick theme adjustments easy:
  - Primary color: `--primary`
  - Card/background colors: `--card`, `--bg`
  - Border radius: `--radius`, `--radius-sm`
- The segmented control uses an animated track for the active state.

---

## Examples

Example 1 — POC mode
- Start Date: `080125` (08/01/25)
- End Date: `090125` (09/01/25)
- Visits per week: `3`
- Duration: `45` minutes

Calculation (as the app does):
- weeks = Math.round(31 days / 7 days) ⇒ Math.round(4.428...) ⇒ 4
- visits = 3 × 4 = 12
- units per visit = 45 / 15 = 3
- total units = 3 × 3 × 4 = 36

Example 2 — Units mode
- Units given: `36`
- Duration: `45` minutes

Calculation:
- visits = round(36 / (45 / 15)) = round(36 / 3) = 12

---

## Limitations & suggested improvements

- Rounding weeks with Math.round can under- or over-count depending on intended behavior. Depending on your policy, you may prefer:
  - Math.ceil to always round up partial weeks,
  - Math.floor to drop partial weeks,
  - or compute partial-week prorated units instead of rounding.
- Date calculations use the browser's local timezone via JavaScript Date objects. For strict cross-timezone consistency, convert dates to UTC or use a date library (e.g., Luxon, date-fns).
- Year pivot (YY → YYYY) follows a simple rule (≤49 → 20YY, >49 → 19YY). Consider switching to a configurable pivot or accepting 4-digit years.
- Durations are limited to 30/45/60. You may allow arbitrary durations and compute fractional 15-minute units if needed.

---

## Contributing

- Suggestions, issues, and pull requests are welcome.
- For UI/styling changes: prefer edits to the top CSS variables.
- For logic changes: update the validation and calculation functions in `index.html`.

If you want, I can:
- Add automated tests for the calculation functions,
- Convert the app into a small npm-based project (React/Vite) for easier development,
- Add precise week calculations or alternative rounding strategies,
- Or create a LICENSE file (MIT is a common choice).

---

## License

No license is included in the repository. If you'd like a permissive license, I recommend adding an MIT license. Tell me if you'd like me to add a LICENSE file.

---

## Contact / Credits

- Developer: Luis R. (displayed in the app footer)
- Repository: robles1999/Authorization-Units-Calculator
