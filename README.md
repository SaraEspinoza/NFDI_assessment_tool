# NFDI service criteria: self-assessment tool (prototype)

A small, bilingual (English / Deutsch) web page that guides a Tool or service provider through the NFDI criteria for the service catalogue, one criterion at a time.

> **Status: draft.** The criteria and scoring forms follow the *Leitfaden für das NFDI-Dienstportfolio* (v0.9, draft) written by Stephan Ferenz, Julia Armin, André Giesler, Lukas Jansen, Denise Lego, Christian Schäfer- Neth, Sonja Schimmler, Cord Wiljes.  Example answers, indicators and scoring forms are proposals and still under discussion.


## What it does

1. The user describes the service and chooses the **type of offer** (software, storage or repository, training, consulting, process or workflow, other).
2. The user moves through the **15 criteria** one by one. Help texts adapt to the chosen type.
3. Criteria 1 to 12 are answered with a yes / no / not applicable checklist. **The result (for example Met, Partly met, Not met) is calculated automatically from the checklist.** Strategic criteria (13 to 15) are free text and not rated.
4. The tool works in **English and German**. A switch (EN / DE) in the header changes the interface, all criteria texts, example answers and the Markdown export. The first visit follows the browser language; the choice is remembered.
5. The user exports the result as **Markdown**, **JSON** (machine-readable, also to continue later) or **PDF** (a real PDF file, created in the browser).

Nothing is sent to a server. Answers are stored in the user's own browser only.

## Files

| File | Purpose |
|---|---|
| `index.html` | The whole application (page, style and logic). No build step, no libraries. It has comments at the top that explain its parts. |
| `criteria.json` | **All content**: criteria, questions, example texts, help, rating rules. Edit this file to change the tool. |

`index.html` also holds a built-in copy of the criteria. It is only a safety net for views that cannot load files (for example a preview) and may be outdated. Change content in `criteria.json` only.

**Colours** follow NFDI4Cat: `#ECF3F5` (background), `#005D7E` (blue), `#8FC73E` (green), `#000000`.

## Change the criteria

Edit `criteria.json` only. Do not edit the built-in copy inside `index.html`.

### Two languages in one file

Every text a user reads is written side by side:

```json
"text": { "en": "The service is available across Germany.", "de": "Der Dienst steht deutschlandweit zur Verfügung." }
```

- Change both `en` and `de` when you change the meaning of a text.
- A plain string (`"text": "..."`) is shown in both languages. Use this only for names that never change.
- If one language is missing, the page falls back to the other language and shows a note on the start page with the number of missing texts.
- Interface texts (buttons, headings) are in `index.html`, in the block `var UI`, with the keys `en` and `de`.

### The form of a criterion and the automatic result

Every criterion from 1 to 12 has `questions` (the checklist) and `levels` (the possible results, best first). The tool calculates the result when all questions are answered. A question answered "not applicable" counts as fulfilled.

**Example answers.** Each checklist question can have an `example` (`{"en":..., "de":...}`, a short sentence with [placeholders]). When the user answers that question with Yes, a text field with the example text appears directly below it. The user completes or edits it there; the text is saved per question. There is no separate block of complete example answers any more.

**Feedback questions.** After the result of every criterion the tool asks two fixed questions: an expert comment ("how do you rate the criterion, would you add or change something?") and the expected workload over time (heavy / medium / low). The texts are in the `UI` block of `index.html` (keys `fb.*`), not in `criteria.json`. The text box "Comments on the questions above" stays below as an open comment field.

**Exports.** On the summary page: **Markdown**, **JSON** and **PDF** (the full filled-in assessment as a real PDF file made in the browser; links are clickable). There is one JSON file only. It is machine-readable (`schema: nfdi-assessment-result/2`: service data, per criterion the status, the calculated rating, every checklist answer `yes`/`no`/`na`/`null` with its reason, the free-text answer and the evidence lines; titles and questions in both languages) and it also contains the raw answers (`state`), so the same file can be imported on the start page to continue later.

**Evidence and practical points.** The criterion's `evidence` and `tips` lists are shown in a collapsible block directly under the explanation of the criterion. They are only for reading.

**Not applicable per question.** When a question is answered "Not applicable", a text box "Why does this not apply? (optional)" appears below it. The reason is saved and appears in the Markdown export and the report. It needs no entry in `criteria.json`.

To make a criterion free text, change its `"form"` to `"freetext"`. To change the draft, also change `"version"` at the top of the file, so exports show which version was used.

If the file has a mistake, the page shows a boxed list of problems instead of the assessment.

