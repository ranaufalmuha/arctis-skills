# Testing and delivery

Follow the selected adapter's native test convention. React SPA and Next.js retain external project-root tests and reject test/spec files in application source. Angular permits colocated tests following its style guide. For other adapters use their documented default or the project's explicit test convention. Do not apply React test placement universally.

Tests exercise feature behavior, UI integration, and real routing flows. Native component/template checks matter for Vue, Svelte, and Angular. For server-capable frameworks verify runtime boundaries and relevant authorization behavior. A static import check is not a framework build or an end-to-end test.

Run the architecture validator only for a supported profile, then relevant native lint/type/build/tests. With guidance-only adapters, review ownership explicitly and report the lack of automated architecture coverage. See [enforcement](enforcement.md). Do not claim a check passed when it was not run.
