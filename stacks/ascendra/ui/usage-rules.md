# ascendra-ui: Usage Rules

Rules for using `ascendra-ui` learned while building with it, including places where the library's own docs are incomplete or wrong. Read in addition to the library's own docs (`../ascendra-ui/docs/ui-reference.md`, `showcase-reference.md`), last, before writing UI code. When a case isn't covered here, read the real pages in `../ascendra-ui/components/{forms,dialogs,sheets,drawers}/` and `../ascendra-ui/app/showcase/` and copy the closest one's structure; never invent a structure freehand. When the docs and the real pages disagree, the real pages win.

Paths below are inside `../ascendra-ui/`. A project's design rules may deviate from a rule here; it records the deviation in its `standards/conventions.md`.

**Adding a rule:** the next `AUI-NNN`, never reused or renumbered; the rule, stated so a diff can be checked against it; the real file that proves it (never the docs alone). A rule that narrows an earlier one marks the earlier one "superseded by".

---

## Forms and fields

**AUI-001 · Section headers inside a form or dialog use `FieldLegend`**, styled by its `variant` (`variant="label"` for a smaller inline label), never a hand-made heading. Ref: `components/forms/contact-inquiry-form.tsx`.

**AUI-002 · Under every control, `FieldHint`, never `FieldError`.** Pass `error` as one `{ message? }` object, plus `mandatory`, `optional` or `help` as needed. See "FieldHint in full" below. Ref: `components/forms/contact-inquiry-form.tsx`.

**AUI-003 · No `FieldContent` wrapper.** `Field > FieldLabel + control + FieldHint`, as siblings. (The showcase docs' templates show `FieldContent`; the real pages don't use it.) Ref: `components/forms/contact-inquiry-form.tsx`, `components/dialogs/invite-member-dialog.tsx`.

**AUI-004 · `DialogContent` has no `showCloseButton` prop**, though the docs list one. Before using an unfamiliar prop found only in the docs, check the component's source. Ref: `ascendra-ui/components/ui/dialog.tsx`.

**AUI-007 · Copy the structure of the closest real form or dialog, and don't add spacing.** Find the nearest match by field count in `components/{forms,dialogs,sheets,drawers}/`, copy its nesting exactly, change only content and validation. Spacing comes from the primitives; never add layout utility classes the reference doesn't have.

**AUI-010 · `FieldHint` ignores children.** Hint text goes in `description="…"`; children silently render nothing. Ref: `ascendra-ui/components/ui/field.tsx`.

**AUI-011 · A `FieldLegend` section header is followed by a `FieldDescription`**: one or two sentences on what the section is for, in the domain's own words. Ref: `components/forms/contact-inquiry-form.tsx`.

**AUI-012 · A form's success or error banner is `SimpleAlert`** with the matching `variant`, never a hand-styled element. Ref: `ascendra-ui/components/common-ui/simple-alert.tsx`.

**AUI-037 · Give `info` to every field in a grid row or to none.** `FieldInfo` reserves no height when empty, so a row where only some fields have `info` misaligns. For a hint on just some fields, use `description` (`FieldHint` does reserve height). Ref: `ascendra-ui/components/ui/field.tsx`.

**AUI-040 · A `Combobox` filters only what it renders itself.** Inside `ComboboxList`: `<ComboboxEmpty>` then `<ComboboxCollection>{(item) => <ComboboxItem …/>}</ComboboxCollection>`, never `.map()`. Give `itemToStringLabel` when the label isn't the item, and `filter` when more than the label should match. Ref: the showcase's combobox preview.

### FieldHint in full

| Prop | Does |
|---|---|
| `error` | `{ message? }`. Replaces the description with the message, destructive, with a warning icon. A react-hook-form `fieldState.error` fits as is |
| `description` | Hint text, shown only when there is no error. Defaults to a non-breaking space, so a bare `<FieldHint />` reserves its row's height |
| `mandatory` / `optional` | One small badge. One choice, never both (both shows only Mandatory) |
| `help` | A `?` badge with this text as a tooltip; can sit beside the other badge |

- **Every control is followed by a `FieldHint`, bare when nothing applies**, so every field's height is the same.
- **Badge the minority only.** If most fields are required, mark the few optional ones, and the reverse. Never badge every field. When all fields of a `QueryDef` are alike, drop the badges and say it once in `QueryDef.info` (AUI-038).

---

## Pages and shell

**AUI-008 · The app shell has a `Header` always, and a `Nav` only when the screen map has top-level sections.** `PageLayout > SideBarOverlay + Header + (Nav) + MainContainer > SideBarToggle + SideBar + ContentArea`. The `Header` holds the brand (`HeaderLinks`) and `HeaderActions` (theme, avatar, sign-out). Ref: `app/showcase/layout.tsx`.

**AUI-009 · A flat sidebar group is `SideBarMenuSet > SideBarMenuSetTitle + SideBarMenuItemGroup > SideBarMenuItem`.** Only a group that really collapses uses `SideBarMenu(basePath) > SideBarMenuHeader + SideBarMenuContent`, never without its header. Ref: `app/showcase/layout.tsx`.

**AUI-023** · Superseded by AUI-033.

**AUI-033 · Every page is one of four recipes, chosen before writing its JSX:**
1. **Dashboard:** `PageHeader variant="dashboard"` + `DashboardContent`. No `PageMain`.
2. **Report:** `(BackLink) + ReportDocumentWrapper`. No `PageHeader`, no `PageMain`.
3. **Tabbed:** plain `PageHeader` → `PageMain` → `Tabs > TabList + TabContent`, each `TabContent` holding `MainContent` (+ `AsideContent`). No `PageWrapper` or `PageContent`.
4. **Standard** (list, detail, settings): plain `PageHeader` → `PageMain` → `PageWrapper` → `PageContent` → `MainContent` (+ `AsideContent` only for a real side panel).

`variant="dashboard"` belongs to recipe 1 only; recipes 3 and 4 get their divider from `TabList` or `PageWrapper`. `PageWrapper` and `PageContent` go together. Ref: `app/showcase/layout-guide/page.tsx`, `data-table-lab/page.tsx`, `dashboards/saas-revenue/page.tsx`, `reports/annual-financial-statement/page.tsx`.

---

## Lists and tables

**AUI-005 · A list page's main action ("Add …") goes in the table's `DataTableBarAction`**, not in `PageHeaderAction`, which the showcase uses only on dashboards. Ref: `app/showcase/data-table-lab/page.tsx`.

**AUI-006 · Before building a list, look at everything the DataTable system offers** (sorting, selection and bulk actions, row menus, search with highlight, filters, column order and visibility, saved preferences, saved queries, server batches), then use only what the requirements need. A search-only table is a fine choice when it's a choice. Ref: `app/showcase/data-table-lab/page.tsx`; the docs' "DataTable System".

**AUI-016 · A screen's tables mirror its design or API.** Don't split one table into two, or merge two, without a stated reason.

**AUI-019 · `DataTableLoadingBody`, `DataTableErrorBody` and `DataTableEmptyBody` work standalone** with `isLoading`, `isEmpty`, `isError`/`error`/`onRetry`. Use them for a simple query-driven table; never hand-roll `Empty` markup or adopt `DataTableProvider` just for them. With a fixed-height table, give all three a matching height `className`. Ref: `components/data-table/data-table-{loading,error,empty}-body.tsx`.

**AUI-024 · Table headers are sentence case** ("Occurred at"). Headers don't wrap by default; don't add `whitespace-nowrap`. Ref: `components/data-table/data-table-head.tsx`.

**AUI-025 · Clicking a row opens a `Sheet`, never a plain page.** A record that needs its own URL stays a route but renders `Sheet` inside it, closing by navigating back. Ref: `ascendra-ui/components/ui/sheet.tsx`, `.claude/commands/create-sheet.md`.

**AUI-026 · Every searchable text cell is wrapped in `DataTableHighlight`**, including the text part of a composed cell. Ref: `components/data-table/data-table-highlight.tsx`.

**AUI-027** · Superseded by AUI-042.

**AUI-028 · Combinable filters are one "Advanced Filter" `QueryDef`**, with `columns` for its grid, when the API ANDs them. Separate named queries only for genuinely different requests. Ref: `providers/data-table-query/data-table-query.types.ts`.

**AUI-029 · A sample batch smaller than every page size makes paging look broken.** Check the sample size before calling the pager broken. Ref: `providers/data-table/use-pagination.hook.ts`.

**AUI-035 · A column with a custom-formatted cell sets `ColumnDef.searchValue`** with the identical formatter the cell uses, or search and highlight disagree with what is shown. Ref: `providers/data-table/data-table.types.ts`.

**AUI-036 · Plain search matches word by word**, so multi-word display strings ("Sep 04, 2026") are fine. Ref: `providers/data-table/use-search.hook.ts`.

**AUI-038 · A bounded range is one `daterange` field**, not two `date` fields; uniform optional or mandatory fields get no badges (see FieldHint in full). Ref: `providers/data-table-query/data-table-query.types.ts`.

**AUI-041 · A row that can hold a `TableRowLink` carries `rowLinkRowClasses`** (from `ui/table`). Never build Tailwind class names by interpolation: an interpolated class is never generated. After adding a row link, click the first row and check the first row opens.

---

## Cards, panels and states

**AUI-015 · Choose each panel's header by what it needs:** a full `CardHeader` (title and subtitle) when the content needs a sentence of explanation; an inline title for a small self-explanatory panel; a bare `CardHeader` over `TableWrapper` for a full-bleed table (see AUI-043). Ref: `app/showcase/dashboards/saas-revenue/page.tsx`.

**AUI-043 · A table that belongs in a card uses `CardTable`:** `Card > CardHeader > CardTable > Table`, then `CardFooter`. A one-list section uses `CardList` with `CardListItem`. A full-bleed table uses `visibleRows={N}`, not `height`, and no `CardFooter` inside `TableWrapper`. Ref: `components/previews/card-preview.tsx`, `components/card/card-table.tsx`.

**AUI-021 · A state replaces only the fetched content inside a section's shell.** The card, its header and a table's header render always; only the values, series or rows swap for a placeholder of the same size.

**AUI-022 · Outside a DataTable, branch states with `WithState`** (`NormalState`, `LoadingState`, `ErrorState`, `EmptyState`) **or `WithSkeleton`**, never a hand-made ternary. Give the branches non-overlapping `if` conditions; reuse a query's own `isLoading` and `isError`. Ref: `components/common-ui/with-state.tsx`, `with-skeleton.tsx`.

**AUI-030 · `Empty` has no visible border by itself.** Wrap it in `Card`/`CardPanel`, or add `border` beside its `border-dashed`. Ref: `ascendra-ui/components/ui/empty.tsx`.

**AUI-031 · To nest a state's default content in your own wrapper, use its `*Message` sibling** (`LoadingMessage`, `ErrorMessage`, `EmptyMessage`), never a `*State` inside a `*State`. Ref: `components/common-ui/with-state.tsx`.

**AUI-039 · A decorative absolutely-positioned element over interactive content carries `pointer-events-none`.** When a click silently does nothing, look for such an overlay first.

---

## Dashboards and charts

**AUI-013 · A row of several panels is `grid grid-cols-12 gap-4`, each panel `col-span-12 md:col-span-N`** (8/4, 7/5), never `grid-cols-2`. Ref: `app/showcase/dashboards/saas-revenue/page.tsx`.

**AUI-014 · A KPI for a period shows its change against the previous period** as a trend badge, and says which period when tiles differ. Ref: `app/showcase/dashboards/saas-revenue/page.tsx`.

**AUI-017 · Sample data behind a dashboard is realistic in scale and deterministic** (a seeded generator); counts on a `YAxis` use `allowDecimals={false}`; large values use thousands separators, then compact notation. A fixture that can't produce realistic ticks is fixed at the fixture.

**AUI-018 · Labels spell out time windows** ("Past 30 days"), never a parameter ("30d"); abbreviations only on chart ticks.

**AUI-020 · Extract a shared part only when real use has proved its shape**, as composable sub-components (`KpiTile`, `KpiLabel`, `KpiValue`, `KpiTrend`, `KpiCaption`), not one component with many props. Ref: `components/common-ui/kpi-tile.tsx`.

**AUI-032 · A chart's loading state is the real chart** with its real axes and formatters, muted flat placeholder data, no tooltip, pulsing, gated by `WithSkeleton`. Each chart supplies its own placeholder domain and series; there is no automatic chart skeleton. Show it only after 150–300 ms.

---

## Values and identifiers

**AUI-034 · Every date or time a person reads goes through `formatDateTime`**, never `toLocaleString`. A precise record uses `{ time: true }`; a recency indicator `{ style: 'relative' }`; a month `{ style: 'month' }`, passing a mid-month date for a `YYYY-MM` value. `formatDate`, `formatShortDate` and `formatDateRange` are deprecated. Ref: `utils/common.util.ts`.

**AUI-042 · Every identifier a person might copy uses `WithCopyText`** (event, entity, correlation or actor IDs, reference numbers). When the value is also a link, give the link and the copy separate click targets. Ref: `components/util/with-copy-text.tsx`.
