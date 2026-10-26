# Web Feature Reference

Build order for a new feature in a frontend SPA that talks to a backend through a generated or hand-rolled API client (React/Vue/Angular/Svelte, in a single app or one of several apps in a monorepo). Stack-agnostic within that shape — skip entirely on a backend-only or non-SPA project. Each step assumes the ones above it are settled.

1. **Shared-client check** — apply **principle-spa-shared-client** before writing a single API call: reuse the app's existing client wiring rather than another client/configuration instantiation. In a monorepo with sibling apps sharing a backend, check their generated clients for the same drift before touching any of them.
2. **Place the feature** — identify which app (if more than one) and which role, permission, or access scope the feature belongs to. This decides where the route or screen gets registered in whatever routing scheme the app already uses (role-keyed route tables, guarded routes, nested layouts — match what's there, don't introduce a second scheme).
3. **Confirm the contract exists** — check whether the generated/typed API client already exposes the needed types or endpoint. If not, that's a backend change first — not something to stub client-side.
4. **Split data from presentation** — follow the app's existing convention for separating data-fetching/state/business logic (a hook, a service, a store) from the component that renders it. A page/component file should stay mostly composition.
5. **Match the existing UI shape** — list+detail, table+modal, wizard, whatever this kind of screen already uses elsewhere in the app. Don't invent a new shape for a feature that fits an existing one.
6. **Forms** — use the app's existing form and validation library and shared schemas, if it has one. Don't start a second validation convention.
7. **Session/auth** — read the current user or token from the app's single session/auth store or context, never a fresh storage read or a new interceptor registration.
8. **Wire the route or entry point, fully** — register it everywhere it's needed and nowhere it isn't. Don't leave a stub with no real logic, a screen unreachable from any route, or the same route/entry registered twice.
9. **Test the data layer, not just the component it feeds** — the hook/service/store is usually where the logic and the risk live. A component- or table-only test suite can pass while the thing doing the real work (auth, pagination, filters, branching) is barely covered.

"SPA project" means the touched paths are inside a frontend app directory built on a SPA framework (package.json naming React/Vue/Angular/Svelte or similar) with a typed or generated client layer over a backend API, as opposed to a backend/API repo — same detection style `how` uses for Odoo.
