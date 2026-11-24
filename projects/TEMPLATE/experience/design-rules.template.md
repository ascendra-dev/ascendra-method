# Design Rules: [Project code]

> **Guide.** Living (METHOD.md section 6). Every settled rule for how this product's screens look and behave, in one place (METHOD.md section 7.3). `/design-group` reads it in full before drawing; `/plan-unit` copies what a unit needs; a built screen follows it.
>
> - **One place.** A rule lives only here. A review finding becomes a rule here and is applied to every affected artboard. A new rule that contradicts one is a decision, and the old text is replaced, not kept beside it.
> - **Rules, not tokens.** The theme's values (colours, type sizes, radii) are in the design system; a rule here says how to use them. How the UI library itself is used on any project is in the stack profile; this file holds this product's rules on top.
> - **Each rule has an ID** (`DR-NNN`, never reused) and, where one exists, the decision that set it. A pattern in `standards/patterns.md` that builds a rule names it.
>
> Write each rule as one short statement a reviewer can check on an artboard. Add an example only where the rule is easy to misread.

| Field | Value |
|---|---|
| Version | [1.0] |
| Design system | [Its name and where it is] |
| Stack profile | [`stacks/{name}/`] |

---

## 1. Principles

- **DR-001** [Principle, e.g. "Unbuilt is absent, not grey: navigation shows only what works for the role; no disabled 'coming soon' items."] ([DEC-NNN])

## 2. Page recipes and layout

> **Guide.** The recipes every page uses (for example dashboard, report, tabbed, standard, wizard), what each is for, and the layout rules inside them: back links, headers, sections, notices, the aside, pages outside the app shell, emphasis.

- **DR-0NN** [Rule]

## 3. Shell and navigation

- **DR-0NN** [Rule: header, navigation, what shows for each role]

## 4. Lists, records and states

> **Guide.** When a list loads whole and when from the server; what a row click opens; selection, paging, search and filters; the states every list and panel has (loading, empty, error, normal) and what each shows.

- **DR-0NN** [Rule]

## 5. Forms and overlays

> **Guide.** The form rule (which surface a form uses: dialog, sheet, page or wizard, and why), fields and hints, validation messages, leaving with changes, confirmations, outcomes after saving.

- **DR-0NN** [Rule]

## 6. Statuses and figures

> **Guide.** One mapping from every status to its badge, for every record; how figures are shown (formats, alignment, what is kept apart and never summed). A design never invents a status or merges figures this section keeps apart.

| Record | Status | Badge | Means |
|---|---|---|---|
| [Record] | [Status] | [Variant or colour] | [One line] |

- **DR-0NN** [Rule on figures: format, alignment, what is never netted]

## 7. Words

> **Guide.** Voice, the product's own terms on screen (from Requirements' language, with any everyday screen word), button verbs, dialog titles, errors, empty states, names and pronouns.

- **DR-0NN** [Rule]

## 8. Outputs

> **Guide.** Printed documents and emails: templates, what leads (the customer's brand or the product's), what each must and must never carry.

- **DR-0NN** [Rule]

## 9. Responsive and themes

- **DR-0NN** [Rule: what is drawn at phone width and dark mode, and when]

## 10. Library gaps

> **Guide.** Parts this product needs that the UI library doesn't have, or has wrong, and the agreed composition or workaround. Each is also reported to the library's owner; remove a row when the library fixes it.

| Gap | Composed as | Reported |
|---|---|---|
| [Missing or wrong part] | [What is used instead] | [Where, date] |

---

## Change log

| Version | Date | Change | Decision |
|---|---|---|---|
| 1.0 | [YYYY-MM-DD] | [First version] | [DEC-NNN] |
