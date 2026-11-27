# Stack Profile: Ascendra

The standards shared by every project built on the Ascendra stack (METHOD.md section 11). A project names this profile in its `CLAUDE.md`; its own choices and deviations are in its `standards/conventions.md`. A convention proven in a second project is proposed here by a method decision.

## The stack

| Layer | Choice | Its own documentation |
|---|---|---|
| API | NestJS, Drizzle, PostgreSQL | — |
| Web | Next.js with `ascendra-ui` | `../ascendra-ui/docs/ui-reference.md` (components, props), `../ascendra-ui/docs/showcase-reference.md` (patterns, templates), the showcase pages in `../ascendra-ui/app/showcase/` |
| Shared modules | `ascendra-commons`, copied in | `../ascendra-commons/` (module catalogue, `MODULE-STANDARD.md`, each module's `README.md`) |
| Language | TypeScript throughout | — |

The library's own documents are read where they live, never copied here.

## The profile

| File | Holds | Read by |
|---|---|---|
| [`api-contract.md`](api-contract.md) | The API's wire rules: error shape (Rule 1), 400 vs 422 (2), success shape (3), lists (4), methods (5), query parameters (6), headers (7), optimistic concurrency (8), exports (9) | `/plan-unit` (A0, endpoint blocks), `/build-chunk` |
| [`validation-messages.md`](validation-messages.md) | How validation messages are written | `/plan-unit` (A3 field validation) |
| [`ui/usage-rules.md`](ui/usage-rules.md) | How `ascendra-ui` is used, learned in projects (`AUI-NNN`) | `/plan-unit`, `/build-chunk`, `/design-group` |
| [`ui/code-style.md`](ui/code-style.md) | How the JSX and TypeScript around components is written | `/build-chunk` |
| [`commons.md`](commons.md) | Adopting a commons module; `.core`-only imports; logging changes to copies | `/plan-unit` (A6), `/build-chunk` |
| [`canvas.md`](canvas.md) | Drawing with the real components on a design canvas | `/design-group` |
| [`testing-page.md`](testing-page.md) | The local-only manual test page in the web app (METHOD.md §8.5) | No command; manual testing |

## What a plan takes from this profile

**Considered for every unit** (added to the method's list in the unit template's A1):

- A large list from the server: `DataTableQuery`, its saved queries and server half
- A small list loaded whole: `DataTable`
- A commons module copied, or a copy changed
- Row security on a new or changed table
- An optimistic-concurrency `version` on a record people change (contract Rule 8)
- An idempotency key on a command (contract Rule 7)

**Standard behaviour** (the unit's A0 copies these, with the project's own additions):

- Errors are the flat shape of Rule 1; a screen reads only an error's `code` and `message`. Data a refusal needs comes from a query the screen already makes, never from the error.
- A 400 lists the API's messages in the form's alert (the contract's 400 carries no field names, and the form checks the same rules first). A 422 shows by its code, under its field or in the form alert.
- Lists follow Rule 4: a bare array only for a collection bounded by nature; otherwise the paged shape.
- Every field has a `FieldHint` (AUI-002); every searchable cell a `DataTableHighlight` (AUI-026); every date or time `formatDateTime` (AUI-034).
- Every page is one of the four recipes (AUI-033).
