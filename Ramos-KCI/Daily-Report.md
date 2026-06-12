# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-12

Current Task:
Fix the login page background image URL that returned 404 after publishing and deployment.

Status:
Done

Progress:
100%

Completed:
- Reviewed the solution structure, web project entry point, static file middleware, login view, shared authentication layout, and existing asset URL patterns before making changes.
- Identified the root cause: `wwwroot` is a physical publish directory and must not be included in the browser URL; `UseStaticFiles()` exposes its contents from the application web root.
- Updated only the background image URL in `web_app/Views/Account/Login.cshtml` to use `@Url.Content("~/assets/images/logos/entity/login_bg_optimized_3.svg")`.
- Confirmed the SVG exists under `web_app/wwwroot/assets/images/logos/entity/`.
- Preserved all other local changes and made no architecture or static-file configuration changes.
- Verified `dotnet build web_app_solution.sln --no-restore`: 0 errors.

Issue / Blocker:
- No active blocker.
- Build reports 3 NU1900 warnings because NuGet vulnerability data could not be retrieved from `https://api.nuget.org/v3/index.json`.

Need Decision:
- None.

Risk:
- Low. The change affects one Razor attribute and aligns the URL with ASP.NET Core static file conventions and application PathBase handling.

Next Action:
- Publish and deploy the updated application, then verify the login page requests `/assets/images/logos/entity/login_bg_optimized_3.svg` without a 404 response.

ETA:
2026-06-12

---

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
