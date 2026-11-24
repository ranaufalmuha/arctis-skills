# Angular adapter

Official source, reviewed 2026-11-24: [Angular style guide](https://angular.dev/style-guide). It recommends feature-area organization and keeping related component files and tests together. Inspect workspace/project configuration and installed version; preserve existing standalone or NgModule architecture.

```text
src/
  main.ts
  app/
    app.ts
    app.html
    app.routes.ts
    auth/
      login.ts
      login.html
      login.css
      login.spec.ts
    shared/
      button/
```

File names are illustrative; follow the project's generated naming for its Angular version. Native feature areas such as auth implement the logical feature layer; an additional feat wrapper is optional, not required. Root app/router composition may register feature entry components or lazy feature routes. Feature-owned route declarations may register their own children; they must not reach into sibling features. Shared must not depend on feature services or components.

Keep injection/provider scopes deliberate. Dependency injection, templates, lazy imports, and NgModule imports are dependency edges, not exceptions to ownership. Related templates/styles remain alongside their component. Colocated tests are allowed here to follow Angular guidance; the React/Next tests-outside-src rule does not apply.

No bundled Angular architecture validator exists. Use the configured Angular compiler/build, tests, and template-aware lint, then review module/provider/template dependencies. Do not run the SPA layout validator against Angular and describe the resulting folder errors as architectural defects.
