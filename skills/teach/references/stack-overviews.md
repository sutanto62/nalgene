# Stack overviews

Detect the project's language/framework from files in the project root, same approach as `worktree-cleanup`'s cache reference. Check framework markers before language markers — a framework marker is more specific and usually implies the language anyway (Android implies Kotlin/Java, iOS implies Swift/Obj-C). A project can match more than one marker; when it does, or when nothing matches cleanly, say your best guess and ask before writing the overview (see `teach/SKILL.md` step 1).

For each stack: the marker(s) that identify it, and what the opening architecture overview should orient a new learner around. Keep the overview short — a map to place the rest of the explanation on, not the explanation itself.

## Odoo

Marker: `__manifest__.py`, `odoo-bin`, or a `requirements.txt` with `odoo` in it.

Orient around: the manifest (`__manifest__.py`) declares a module's dependencies and what loads; models (`models/*.py`) define data and business logic on top of the ORM; views (`views/*.xml`) define UI; security (`security/ir.model.access.csv`, record rules) gates access; controllers (`controllers/*.py`) expose HTTP routes. Say which layer this module lives in — Community, Enterprise, or a custom addon — since load order (Community → Enterprise → Custom) decides whose definition of a model or view wins, the same layering `how`'s Odoo track traces.

## Android

Marker: `AndroidManifest.xml`, or `build.gradle`/`build.gradle.kts` applying the `com.android.application` or `com.android.library` plugin.

Orient around: `AndroidManifest.xml` declares the app's components (Activities, Services, Receivers) and the launcher Activity is where the app starts; Gradle module graph (`settings.gradle[.kts]`) shows `:app` plus any feature/library modules; the architecture pattern in use (MVVM, MVI, Clean) and its layers (UI — Activities/Fragments/Composables, ViewModels, repositories/data sources); DI framework if any (Hilt, Dagger, Koin).

## iOS

Marker: `*.xcodeproj`, `*.xcworkspace`, `Podfile`, or a `Package.swift` whose platforms include iOS.

Orient around: where the app launches (`@main App` + `Scene` for SwiftUI, or `AppDelegate`/`SceneDelegate` for UIKit); whether the UI layer is SwiftUI or UIKit (storyboards vs programmatic); the module/target layout (`.xcodeproj`/`.xcworkspace`, any local Swift packages); dependency management (SPM, CocoaPods via `Podfile`).

## Node.js / JavaScript

Marker: `package.json` (and no Android/iOS marker alongside it).

Orient around: the entry point (`package.json`'s `main`, or the framework's convention — e.g. Next.js's `app/`/`pages/`, Express's `app.js`/`server.js`); the framework in use, read off `dependencies` (Express, Fastify, Next.js, NestJS, etc.); module system (CommonJS vs ESM, from `"type"` in `package.json`); monorepo layout if `workspaces` is set. For a frontend-only JS project (no server framework in `dependencies`), orient around the bundler/entry (`index.html`, `vite.config.*`, `webpack.config.*`) and the component tree's root instead.

## Go

Marker: `go.mod`.

Orient around: the module path and Go version in `go.mod`; which binary this is, if there's a `cmd/*` directory with multiple entry points; the split between `internal/` (unexported, project-private) and top-level packages (the module's public surface, if it's a library); where `main()` wires dependencies together versus where business logic actually lives.

## Kotlin (non-Android, JVM backend)

Marker: `build.gradle[.kts]` or `pom.xml` with Kotlin, no Android plugin applied.

Orient around: the framework in use (Ktor, Spring Boot) and its entry point (`main()` / `@SpringBootApplication` class); module layout under Gradle's multi-module conventions if present; routing/handler layer versus service/domain layer.

## Swift (non-iOS, server-side)

Marker: `Package.swift` with no iOS platform listed (or targeting Linux/macOS server use).

Orient around: the framework in use (Vapor, Hummingbird) and its entry point (`main.swift` / `configure.swift`); route registration; the package's target layout (`Sources/`, `Tests/`).

## General fallback

Nothing matches, or the match is ambiguous: say what you found (or didn't) and ask what language/framework this is rather than guessing an overview that might be wrong. Once confirmed, orient around whatever that stack's own convention is for "where does this start and how is it organized" — entry point, module/package layout, and the dominant architectural pattern if one is evident.

## Adding a new stack

Add a marker and its overview here. `teach/SKILL.md` step 1 already treats this list as open-ended — no change needed there.
