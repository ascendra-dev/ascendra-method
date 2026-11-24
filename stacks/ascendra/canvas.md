# ascendra-ui on a Design Canvas

How screens are drawn with the real `ascendra-ui` components on a Claude Design canvas, so a design is the library itself, not a picture of it. A project's design runbook holds its own pipeline (folders, scripts, publishing, its canvas's limits); this file holds what is true for any project on this stack.

## The approach

- The product's **design system** is `ascendra-ui` with the product's theme, compiled into one bundle that assigns every component to a global (`window.{DS}`, for example `window.AscendraPay`). It is installed on the canvas, and rebuilt and reinstalled after a library, theme or card change.
- **Screens are React**, written with the real components from that global, compiled into one screens bundle. Each artboard is a small file that mounts one compiled screen with its props. Never compose a look-alike by hand: use the whole library part, at its defaults.
- **Render before publishing:** every changed artboard is rendered locally in a headless browser at its own size, with no console errors and no sideways overflow, and the screenshot is looked at. A card can render without errors and still be wrong.

## Rules on a canvas

- Load in order: `tokens.css`, the bundle's stylesheet, then the bundle's script. The bundle reads `window.React` and `window.ReactDOM` (React 18 or 19) and carries no React of its own.
- Mount a component: `<x-import component-from-global-scope="{DS}.Button" variant="secondary">Print</x-import>`.
- **Dark mode** is `data-theme="dark"` on `<html>`, because dialogs, sheets and menus portal to `<body>`.
- **Providers:** wrap a screen in `TooltipProvider` when it uses tooltips or `WithCopyText` with a tooltip, and in `QueryProvider` when it uses `TableLookup` or a query-driven DataTable.
- **Icons:** `{DS}.Icon` takes any Lucide name (`name="Banknote"`); a prop that wants an icon component takes `{DS}.Lucide.Banknote`.
- **Charts:** `ChartContainer` with Recharts parts from `{DS}.Recharts`.
- **No router:** set `window.__dsPath` to the path before rendering to show a sidebar item or nav link as active.
- **A DataTable with queries reads the address bar:** call `history.replaceState(null, '', '?' + {DS}.buildQuerySearch(queryId, params))` before rendering to open a chosen query.
- **No page-wide link colour.** Never style `a` in a screen's own CSS; primary-coloured text only where a component asks for it.
- **Card separators:** the library draws them with CSS on a card's direct children, which a canvas slot hides, so set them: a `CardFooter` over a `CardPanel` or `CardTable` gets `class-name="border-t-0"`; a `CardHeader` over a `CardTable` gets `class-name="border-b"`.
- **`style` on an `x-import` styles its slot**, not the component: size, font and colour go in as `class-name`, using only classes the bundle's CSS already has.
- **Body text:** the library app sets `text-sm` (14/20) on `<body>`; a compiled screen outside it sets the same on its root, or text in tables inherits 16px.
- **Stand-ins:** `next/link` renders a plain `<a>`, `next/navigation` returns `window.__dsPath`, `next-themes` reads and sets `data-theme` on `<html>`.
