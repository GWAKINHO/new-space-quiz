# Copilot instructions

Single-file Korean space-exploration quiz: `index.html` holds all HTML, CSS, and JS. No build, tests, linter, package manager, or server. Open the file directly (`file:///D:/space-quiz/index.html`) to run it.

## Architecture

- **Data:** the `Q` array in the script. Each entry is `[question, [4 options], correctIndex, explanation]`. Quiz length derives from `Q.length`, but the header text "문제 1 / 10", the progress `aria-valuemax="10"`, and the result tiers assume 10 questions. Update them if the count changes.
- **State:** module-level `i` (question index), `score`, `locked` (blocks double answers).
- **Screens:** `#quiz` and `#res` sections toggled via `style.display`. `finish()` picks a result tier from a descending-threshold list (10/8/5/0) via `.find(x => score >= x[0])`, and adds `.great` plus confetti at 8+.
- **Flow:** `show()` renders a question, `pick()` grades and applies feedback, `#next` advances, and `#again` resets.

## Conventions

- All user-visible text is Korean, and `<html lang="ko">`. Keep new strings in Korean.
- Theming is CSS variables in `:root`, overridden in `@media (prefers-color-scheme: dark)`. Use the variables (`--accent`, `--ok`, `--bad`, `--card`, ...), not hard-coded colors, so both themes keep working.
- All animations (pop, shake, twinkle, bounce, confetti, blink) must stay disabled under `prefers-reduced-motion`. `celebrate()` also checks `matchMedia` before creating confetti.
- Accessibility: progress bar has `role="progressbar"` and `aria-valuenow` (updated in `setBar`), feedback is in an `aria-live` region, and the focus ring uses `:focus-visible`. Number keys 1–4 select answers, and focus moves to `#next` after answering and to `#again` on the results screen.
- Body font stays at 15px sans-serif (system font stack). The layout is a single column capped at `max-width:440px`.
- Feedback classes: `.correct` (green pop) and `.wrong` (red shake). On a wrong answer both the chosen and the correct option are marked.

## Verifying changes

There are no automated tests. Verify in a browser: reload the page, then drive it with key presses (1–4) and `#next` clicks through all 10 questions. Check score, progress bar, feedback classes, and the results tiers (including 8+ for the `.great` styling and confetti) in both light and dark schemes.
