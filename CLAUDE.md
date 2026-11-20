# Math Practice site

Short practice quizzes for sixth-grade students. Plain static HTML hosted on **GitHub Pages** (`Connection-Denied/math-help`). There is no build step, no package manager and no shared assets.

## Layout
- `index.html`: the landing page. It has one `<h2>` section per topic, each with a `<ul class="quiz-list">` of quiz cards.
- `<topic>_quiz<N>/index.html`: one self-contained quiz per folder, with its own CSS and JavaScript. Current quizzes:
  - `integer_quiz1/`, `integer_quiz2/`: integer addition and subtraction
  - `fraction_decimal_quiz1/`, `fraction_decimal_quiz2/`: multiplying decimals and fractions (choose all that apply), decimal patterns, and fruit price word problems. Same format, different numbers.

## Preview locally
Opening files directly (`file://`) breaks the folder links, because only a web server loads `index.html` for a folder. Run this from the repo root and open http://localhost:8000:
```
python3 -m http.server 8000
```
The server shows whatever branch is checked out.

## Adding a quiz
1. Copy the **newest** quiz folder as the template. Right now that's `fraction_decimal_quiz2/`. The fraction quizzes are the only ones that support the word-problem `text` field and the `select` type.
2. Replace the `QUIZ_QUESTIONS` array. The comment block above it documents the format.
3. Update the topic-specific text: `<title>`, `<h1>`, the key rule box and intro sentence in `buildIntro()`, `KEY_RULE`, `MATCH_GOAL` and `resultMessage()`.
4. On the landing page, add a card to the topic's existing section, or add a new `<h2>` and `<ul class="quiz-list">` for a new topic. CSS already spaces consecutive sections.

### Question format
- `mc`: `expression` (shown as "What is …?"), optional `text` (a word problem shown instead), 4 `choices`, `answer` (0-based index), `hint` (shown after the first miss) and `explanation`. Worth 1 point.
- `select` ("choose all that apply", only in the `fraction_decimal_quiz*/` folders): `prompt`, `choices`, `answers` (array of 0-based indexes), `hint` and `explanation`. Worth 1 point per correct box. After the last try, the score is correct boxes checked minus wrong boxes checked, never below 0.
- `match`: `left` and `right` item lists, `answers` (leftId → rightId) and `explanations` per left item. Worth 1 point per pair.
- Students get two tries per question (`MAX_ATTEMPTS`), and results show a full review.
- `fmt()` displays every `-` as a minus sign, so don't use hyphenated words in any text.
- Write fractions as `3/4`, mixed numbers as `2 1/2`, and operators as `×` and `÷`.

### Content conventions
- Sixth-grade level. Match each topic's constraints. For example, the fraction/decimal quiz uses positive values only.
- Wrong choices should be common mistakes, such as multiplying instead of dividing, flipping the wrong fraction, misplacing the decimal point, or giving the amount used instead of the amount left.
- No wrong choice may equal the correct value. Vary which letter holds the correct answer.
- Every question has a hint and an explanation that walks through the steps.

## The engine is copied into every quiz
Each quiz carries its own copy of the quiz code, so an engine change or bug fix must be repeated in every quiz folder that needs it. The integer quizzes predate the `text` field.

## Before finishing a change
- Check every answer with exact math, for example Python's `fractions.Fraction` run against the `QUIZ_QUESTIONS` data. Confirm the marked answer is correct and no wrong choice equals it.
- Preview the quiz in a browser, including a phone-width check (about 375px).

## Git workflow
- Do all work on a feature branch. **Never merge into `main`** unless the user explicitly asks.
- Commit or push only when the user asks.
