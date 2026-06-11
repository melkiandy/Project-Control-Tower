# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-11

Current Task:
Implement global base URL handling for frontend fetch requests when deployed under an IIS virtual application.

Status:
Done

Progress:
100%

Completed:
- Reviewed the solution structure, shared layouts, script loading order, and 136 fetch calls across 25 page scripts.
- Identified the root cause: URLs beginning with `/` resolve from the domain root instead of the IIS virtual application root.
- Added `window.AppConfig.baseUrl` to `_Layout.cshtml` and `_LayoutAuth.cshtml` using Razor `@Url.Content("~/")`.
- Added `App.BaseUrl`, `App.ResolveUrl()`, and a compatible global fetch wrapper in `core.js`.
- Preserved absolute URLs, protocol-relative URLs, data URLs, blob URLs, and Request objects.
- Verified JavaScript syntax with `node --check`.
- Verified the solution with `dotnet build web_app_solution.sln --no-restore`: 0 errors.

Issue / Blocker:
- No active blocker.
- Build reports 104 existing nullability warnings in the infrastructure project.

Need Decision:
- None.

Risk:
- Low. The change is limited to shared frontend URL resolution and does not alter controllers, routes, services, or business processes.

Next Action:
- Deploy the updated publish output to IIS and validate fetch requests under the configured virtual application path.

ETA:
2026-06-11
