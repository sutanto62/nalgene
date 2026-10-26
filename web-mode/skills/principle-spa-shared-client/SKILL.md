---
name: principle-spa-shared-client
description: "Apply when adding or touching a feature's data-fetching code, or anything auth/session-related, in a frontend SPA that talks to a backend through a generated or hand-rolled API client. Reuse the app's existing client and session wiring instead of instantiating another one or reading the session token from a new place."
disable-model-invocation: true
---

# SPA: share the client, don't duplicate it

A frontend SPA that talks to a backend through a generated (OpenAPI/GraphQL codegen) or hand-rolled API client only pays off the "one client" idea if every feature actually goes through it. Left unchecked, each feature's data-fetching code tends to instantiate its own client configuration instead, and each place that needs the session token tends to read it from storage directly instead of through whatever the app's single source of session truth is. In a monorepo with more than one such app sharing a backend, each app usually also carries its *own* copy of the generated client, which can drift from its siblings with nothing to catch it.

**Why:** every duplicate client instantiation is a place a backend contract change, a new auth header, or a changed base URL has to be applied by hand and can be missed. Every extra place that reads the session token from storage is a place a logout or token-refresh bug can hide. Registering HTTP interceptors somewhere other than app bootstrap (e.g. inside a component that re-renders) stacks duplicate interceptors silently. None of this shows up as a bug until the one copy that got missed is the one a user hits.

**Pattern:**
- Before instantiating a new API client/configuration object in a feature's data-fetching code (a hook, a service, a store — whatever this app's convention is), check whether a shared client module already exists. If it doesn't, building that is the fix to make *before* adding another duplicate instantiation — not a side project to defer.
- Don't add another place that reads the session token from `localStorage`/`sessionStorage`/cookies. Route all session reads through the app's single session/auth store or context.
- Don't register HTTP interceptors outside app bootstrap. A component that registers one on every render is the anti-pattern, not something to extend.
- A presentational or shared component that imports the HTTP client or a generated API class directly, instead of receiving data as props or calling into the feature's own data layer, is a layering violation — move the call where the rest of this app's data-fetching lives.
- Name what you found and which option you took, even when the answer is "no shared client exists yet, so I added the first one." That's a finding, not silence.

**Multi-app boundary** (monorepo with more than one SPA sharing a backend):
- The real contract lives in the backend's API spec, not in any one app's generated client — each app's copy is a snapshot and can be stale in a different way than its siblings.
- Before regenerating a generated client in one app, check whether sibling apps' copies need the same regeneration against the same spec revision, and say so explicitly even when the answer is "not this time." Skipping that check is exactly how sibling copies drift apart.
- If more than one app needs the same new endpoint, regenerate all of them from the same spec revision together — don't patch one app's generated file by hand to unblock it.
