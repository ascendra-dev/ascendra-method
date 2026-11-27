# The manual test page

How the local-only test page of METHOD.md section 8.5 is built in an Ascendra web app (Next.js with `ascendra-ui`). What the page shows and the files it reads and writes are in `guides/manual-testing.md`; this file says only how it is built here.

## Rules

1. **Local development only.** The page and its route handler use the file names `page.dev.tsx` and `route.dev.ts`. `next.config.ts` adds `dev.tsx` and `dev.ts` to `pageExtensions` only when `NODE_ENV` is `development`, so `next build` never includes them: staging and production have no such route. As a second guard, both answer `notFound()` unless `NODE_ENV` is `development` and the path setting is present.
2. **Where it lives.** `app/dev/manual-tests/page.dev.tsx` and `app/dev/manual-tests/api/route.dev.ts`, outside the `(workspace)` and `(auth)` groups: no sign-in, no shell. Its components in `app/dev/manual-tests/_parts/`. No other code imports them.
3. **The path setting.** `MANUAL_TESTS_DIR` in `.env.local`, relative to the web app's folder, naming the project's `delivery/testing/`, for example `../ascendra-method/projects/{CODE}/delivery/testing`. Listed, commented out, in `.env.local.example`.
4. **The route handler** runs on the Node runtime and is the only code touching the files:
   - `GET` returns `people.json`, `issues.json`, and for the requested unit its `registry.json`, every `round.json` and every results file.
   - `PUT` saves one results file: the body is the whole file for one person and one round. It refuses a person not in `people.json`, a round that doesn't exist or has `ended`, and any path that resolves outside `MANUAL_TESTS_DIR`. It writes to a temporary file and renames it.
   - It never writes the registry, a round or the issues.
5. **The tester** is chosen from `people.json` in the header and kept in `localStorage`; it names the results file the page saves.
6. **Saving** happens when the tester presses Save, in the case Sheet's footer, which stays visible: one press writes the whole results file, with every case changed since the last save. The state shows beside it and in the header ("Saved", "Unsaved changes", "Saving", "Not saved: retry"); reloading or closing the page with unsaved changes asks first. Clear result takes a case back to Not run by removing its entry.
7. **Built with `ascendra-ui` parts at their defaults** (`ui/usage-rules.md`), as any other page: `Tabs` for Cases, Issues, Coverage; a list of units and chunks with counts; a `DataTable` of cases; the open case in a `Sheet`. No styling of its own beyond the library's.
8. **No tests of its own beyond type-checking and lint.** It is a development tool; it is proved by the first round run with it.
9. **Built once per project,** on its own branch from the main line, merged by the PO with `--no-ff`. A change to it later is a small change outside a unit.
