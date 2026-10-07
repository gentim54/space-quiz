# Copilot instructions

## Commands

- Run the app by opening `index.html` in a browser. From PowerShell: `Start-Process .\index.html`.
- No build, test, or lint commands are configured in this repository, and there is no automated test suite or individual test command.

## Architecture

- The project is a self-contained static quiz. Markup, styling, question data, and application logic all live in `index.html`; there are no dependencies or separate application modules.
- The inline script owns the question data and the quiz state (`current`, `score`, and `answered`). `renderQuestion` updates the quiz view, `selectAnswer` handles scoring and feedback, `showResults` switches to the report, and `restart` resets the state.
- The quiz and results are separate sections in the same page. Navigation hides and reveals these sections rather than loading another page.

## Conventions

- Keep each question as an object in the `questions` array. Its `correct` value is a zero-based index into that question's `answers` array; keep the answer and index aligned when editing question content.
- Keep state transitions in the existing quiz functions. Selecting an answer is guarded so it scores only once, disables all answer buttons, marks the correct answer, and presents feedback before the next-question action.
- Keep presentation in the inline stylesheet and use the existing CSS custom properties for theme colors. Dark mode is handled with `prefers-color-scheme`, the narrow-screen layout uses a 480px breakpoint, and reduced-motion preferences disable animations and transitions.
- Preserve the existing accessibility and keyboard behavior: use semantic buttons, keep progress and feedback announcements accessible, update the progress bar's `aria-valuenow`, and retain explicit focus movement when answers, navigation, and results change.

## Browser smoke checks

- Open `index.html` in the integrated browser for an end-to-end smoke check; no server or install step is required.
- Start at the initial screen and use Tab/Enter to select an answer and advance. Confirm that the score and progress update, correct answers show “Correct!” feedback, incorrect answers show “Not quite!” feedback without increasing the score, and answer buttons cannot be selected twice.
- Complete all 10 questions and verify the results screen reports the accumulated score. Use Enter on “Play again” and confirm the quiz returns to question 1 with score 0.
