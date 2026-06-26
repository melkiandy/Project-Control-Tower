# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-26

Current Task:
Fix build error in SAP Service Layer client after Step 4 HttpClient/session management changes.

Status:
Done

Progress:
100%

Completed:
- Reviewed build error attachment and confirmed all 25 compiler errors came from `SapServiceLayerClientService.cs` around the end of the class.
- Identified root cause: obsolete leftover code from the old manual HttpClient pattern remained after the new session-cache implementation, including an unnamed block with `await`, old `_http/_handler` references, `ClearCookies()`, and an extra `#endregion`.
- Removed only the obsolete trailing block and extra preprocessor directive from `SapServiceLayerClientService.cs`.
- Preserved the new Step 4 implementation: IHttpClientFactory, session cache, relogin lock, timeout, and duration logging.
- Verified `dotnet build`: build succeeded with 0 error and 112 existing warnings.

Issue / Blocker:
- No active blocker for the application build.
- Existing warnings are still mostly nullability/reference warnings outside this fix scope.

Need Decision:
- None.

Risk:
- Low. Change only removes unreachable/invalid leftover code that referenced removed fields and duplicated logout/dispose behavior.

Next Action:
- Continue smoke test for SAP GET/POST/PATCH flows after the Step 1/2/4 changes.

ETA:
2026-06-26

---

# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-26

Current Task:
Audit kesiapan operasional aplikasi KCI untuk berjalan 24 jam dalam 3 shift setelah Step 1, Step 2, dan Step 4.

Status:
Done

Progress:
100%

Completed:
- Reviewed current working tree, Program startup pipeline, SAP Service Layer client, common dependency injection, database service, background email worker, rate limiter, appsettings, and logging configuration.
- Confirmed Step 1/2/4 improvements are present in code: SAP duration logging, batching/chunking for large SAP lookups, IHttpClientFactory registration, SAP session cache, relogin lock, and request timeout default.
- Assessed that the application is technically more ready for 24-hour operation after Step 1/2/4, especially for reducing repeated SAP login, oversized SAP request filters, and request hanging risk.
- Identified remaining operational dependencies: IIS app pool configuration, server resource monitoring, PostgreSQL connection/pool health, SAP availability, and production smoke test evidence.
- Identified startup risk: application runs DB migration/seeding and SAP SQLQueries sync during startup, so restart/recycle depends on DB and SAP availability.
- Identified logging risk: Serilog rolls daily but no retention/file-size limit is configured in appsettings, so 24/7 operation needs log retention policy outside code or config adjustment later.
- Identified database risk: custom database service opens/disposes connections per operation, which is acceptable with provider pooling, but command timeout/pool sizing is not explicitly configured in connection strings.
- No application code was changed during this audit.

Issue / Blocker:
- No audit blocker.
- Runtime capability still needs confirmation through 24-hour or multi-shift smoke/soak testing on the target IIS/SAP/DB environment.

Need Decision:
- Decide whether IIS app pool idle timeout/recycle schedule, log retention, DB pool/command timeout, and SAP health monitoring should be formalized before go-live 24/7.

Risk:
- Medium if deployed 24/7 without IIS always-on/no-idle configuration, log retention, DB timeout/pool settings, SAP health monitoring, and smoke test evidence.
- Low to Medium after those operational controls are confirmed because Step 1/2/4 already reduced major SAP request/session bottlenecks.

Next Action:
- Run smoke test for one full shift, then soak test across shift handover, covering Generate QR, Production Confirmation, Positive Release, Warehouse In/Out, Delivery Order, Repack, Scan Bin-to-Bin, and Document Print.
- Verify IIS app pool settings: AlwaysRunning/start mode, idle timeout disabled, planned recycle window outside production peak, and rapid-fail protection policy.
- Verify log retention and disk monitoring for daily SAP duration logs.

ETA:
2026-06-26

---

# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-26

Current Task:
Step 4 - Perbaiki HttpClient/session management SAP Service Layer dengan IHttpClientFactory, session cache, relogin lock, dan timeout.

Status:
Done

Progress:
100%

Completed:
- Reviewed struktur solution, Program registration, common DI, SAP Service Layer client, option model, dan konfigurasi SapServiceLayer sebelum editing.
- Mengganti pembuatan `HttpClient` manual di SAP common client menjadi named `HttpClient` melalui `IHttpClientFactory`.
- Menambahkan timeout request SAP dari `SapServiceLayerOptions.RequestTimeoutSeconds` dengan default 180 detik agar environment existing tetap kompatibel tanpa wajib edit appsettings.
- Mengubah session SAP menjadi cache eksplisit di client umum: `B1SESSION`, `ROUTEID`, dan expiry session disimpan di memory service, lalu dikirim lewat header `Cookie`.
- Menonaktifkan automatic cookie handler pada named HttpClient agar session SAP tidak tersembunyi di pooled handler dan dapat dikontrol oleh session cache aplikasi.
- Menambahkan relogin lock menggunakan `SemaphoreSlim` agar request paralel tidak melakukan login ulang bersamaan saat session belum ada/expired/401.
- Mengubah flow relogin: request memakai session cache, hanya relogin ketika cache expired atau SAP mengembalikan 401, lalu retry sekali dengan session baru.
- Mempertahankan logging durasi request SAP dari Step 1 pada semua request SAP common client.
- Verified `dotnet build`: build succeeded dengan 0 error dan 112 warning existing/nullability.

Issue / Blocker:
- No active blocker.
- Build masih memiliki 112 warning existing/nullability di area existing yang tidak menjadi scope Step 4.
- Validasi runtime ke SAP belum dilakukan dari environment ini karena tidak ada smoke test koneksi SAP/UI dalam task ini.

Need Decision:
- Tentukan apakah `RequestTimeoutSeconds` dan `SessionTimeoutMinutes` perlu dioverride per environment di appsettings Development/Staging/Production atau cukup memakai default code-level saat ini.

Risk:
- Medium sampai dilakukan smoke test runtime. Perubahan menyentuh client umum SAP sehingga semua request SAP akan melewati session cache dan relogin flow baru.
- Risiko teknis utama yang dikurangi: socket exhaustion karena manual HttpClient, login ulang berlebihan, relogin paralel, dan timeout request SAP yang tidak eksplisit.

Next Action:
- Lakukan smoke test login SAP dan flow GET/POST/PATCH utama: Production Confirmation, Positive Release, Generate QR, Warehouse Out/In, Delivery Order, Repack, dan Scan Bin-to-Bin.
- Monitor log durasi SAP setelah deploy untuk memastikan jumlah login turun dan request besar tidak menggantung tanpa timeout.

ETA:
2026-06-26

---

# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-26

Current Task:
Step 2.2 - Tambahkan batching/chunking untuk batch, production order, sales order, dan HU pada proses SAP Service Layer.

Status:
Done

Progress:
100%

Completed:
- Reviewed struktur solution dan file SAP Service Layer terkait sebelum editing.
- Menambahkan chunking filter SAP untuk batch di `BatchMasterDataService` pada lookup by item code, batch number, dan HU warehouse-out, termasuk normalisasi nilai, escape OData value, dan follow `ODataNextLink`.
- Menambahkan chunking production order di `ProductionOrderService` untuk lookup by document number dan item number, dengan order type tetap dipertahankan sebagai filter tambahan.
- Menambahkan chunking sales order di `SalesOrderService` untuk lookup by DocEntry tanpa mengubah `$select` existing.
- Menambahkan bulk HU lookup di `SQLQueriesService` melalui `GetHUStockLocationsAsync` dengan chunk 20 dan concurrency maksimal 5 karena endpoint SAP SQLQueries `GetHUStockLocation` masih single-HU.
- Mengganti caller HU massal di DeliveryOrder, WarehouseOut, WarehouseIn, RepackOrder, dan ScanBinToBin agar memakai bulk HU lookup terkontrol, bukan `Task.WhenAll` langsung tanpa batas.
- Verified `dotnet build`: build succeeded dengan 0 error dan 112 warning existing/nullability.

Issue / Blocker:
- No active blocker.
- Build masih memiliki 112 warning existing/nullability di beberapa service/model yang tidak menjadi scope Step 2.2.
- HU endpoint SAP saat ini tetap single-HU, sehingga batching dilakukan client-side dengan concurrency limit, bukan satu request SAP berisi banyak HU.

Need Decision:
- Tentukan apakah chunk size 20 dan HU max concurrency 5 akan dijadikan konfigurasi appsettings pada improvement berikutnya atau tetap konstanta internal service.

Risk:
- Low to Medium. Perubahan kecil dan build berhasil, tetapi tetap perlu validasi runtime dengan data SAP besar untuk memastikan response paging/nextlink SAP sesuai ekspektasi di environment KCI.

Next Action:
- Lakukan smoke test UI untuk flow production confirmation/positive release/generate QR, warehouse out/in, delivery order, repack, dan scan bin-to-bin dengan data HU/item/order yang besar.
- Setelah validasi, lanjutkan Step berikutnya untuk `$select`, timeout/retry, atau pembatasan paralel request SAP lain yang masih di luar scope HU.

ETA:
2026-06-26

---

# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-25

Current Task:
Audit frontend and backend GenerateQR for bulk opening-balance upload from legacy production orders, and define a proposed Excel/CSV template without adding application code.

Status:
Done

Progress:
100%

Completed:
- Pulled the latest `main` branch of Project-Control-Tower before preparing this report.
- Reviewed the KCI solution structure and traced GenerateQR across Razor view, page JavaScript, controller, service/model layer, SAP Service Layer clients, database seeders, document/batch/HU sequences, handling-unit history, and recent runtime logs.
- Confirmed the current frontend flow: select Document Type `2` (Production Order), search a released SAP production order, derive machine/item/description and HU count, preview batch/HU numbers, submit header plus one detail row per HU, then print QR labels.
- Confirmed the current backend flow: validate header/detail count, generate document number, generate/reuse batch for the current day, allocate HU numbers, insert `trx.generateqr` and `trx.generateqr_handlingunitorder`, reload saved data, and create `trx.handlingunithistory` receipt records.
- Confirmed legacy/opening-balance production orders cannot reliably use the current search flow because it only lists SAP orders with `ProductionOrderStatus = boposReleased`, order type `ZP03`, and open production UDF status.
- Confirmed direct lookup by production-order document number does not apply the released-status filter, so a dedicated import-validation path can still validate old/closed SAP production orders without changing their SAP status.
- Identified that GenerateHandlingUnit limits quantity against SAP planned quantity minus quantities already stored in active GenerateQR records. This is suitable for normal operation but can reject opening-balance stock that represents historical production already completed in SAP.
- Identified that the UI HU grid is read-only. Quantity distribution is calculated from SAP planned quantity and item BPP, so the existing frontend cannot represent arbitrary legacy per-pallet quantities without an import-specific staging/preview step.
- Identified that preview values are not authoritative: Save regenerates document, batch, and HU numbers. Import must therefore reserve sequences only during final commit, not during file preview.
- Identified a historical runtime failure caused by raw SQL string construction when a production description contained an apostrophe. Bulk import must use parameterized/bulk database operations and must not reuse interpolated SQL construction.
- Identified scalability and integrity risks for large opening balances: one HU sequence update per row, a large `UNION ALL` insert statement, no uniqueness constraint on HU number, no explicit duplicate-file/idempotency key, and HU history being written after the GenerateQR transaction in separate calls.
- Defined the recommended import flow: upload -> parse -> normalize -> validate every row -> group by `ImportGroupKey` -> preview errors and calculated totals -> explicit commit -> reserve document/batch/HU sequences -> atomic header/detail/history write -> return generated numbers -> reuse existing GenerateQR label preview/print rendering.
- Defined the recommended flat Excel/CSV format as one row per HU. Rows sharing the same `ImportGroupKey` become one GenerateQR header.
- Proposed required columns: `ImportGroupKey`, `ProductionOrderNumber`, `ProductionDate`, `ShiftCode`, `GroupCode`, `MachineNumber`, `ProductionItemNumber`, `ProductionDescription`, `WarehouseCode`, `BinCode`, `Quantity`.
- Proposed optional audit columns: `LegacyReference`, `LegacyBatchNumber`, and `Notes`. `LegacyBatchNumber` must remain informational until the batch-number policy is decided.
- Proposed CSV example:

```csv
ImportGroupKey,ProductionOrderNumber,ProductionDate,ShiftCode,GroupCode,MachineNumber,ProductionItemNumber,ProductionDescription,WarehouseCode,BinCode,Quantity,LegacyReference,LegacyBatchNumber,Notes
OB-26000004-01,26000004,2025-12-31,1,4,11,12033151,30ML EAGLIN PHARM FLAT DF4B 00 LA00,3000,,12800,COUNT-2025-001,LEGACY-BATCH-001,Full pallet
OB-26000004-01,26000004,2025-12-31,1,4,11,12033151,30ML EAGLIN PHARM FLAT DF4B 00 LA00,3000,,12800,COUNT-2025-002,LEGACY-BATCH-001,Full pallet
OB-26000004-01,26000004,2025-12-31,1,4,11,12033151,30ML EAGLIN PHARM FLAT DF4B 00 LA00,3000,,6400,COUNT-2025-003,LEGACY-BATCH-001,Partial pallet
```

- Defined core validations: required values, date and numeric formats, allowed shift/group, positive quantity, consistent header fields inside one group, SAP PO existence, PO item/machine/warehouse reconciliation, item-master existence, duplicate rows, duplicate import group, duplicate legacy reference, optional quantity reconciliation, and generated HU uniqueness.
- Kept the KCI application working tree unchanged; no frontend, backend, database, or configuration code was added or modified.

Issue / Blocker:
- No technical blocker for the audit.
- Implementation should not begin until the business rules below are decided because they change data identity and stock-history behavior.

Need Decision:
- Batch policy: generate a new current-date batch exactly like GenerateQR today, derive a batch from the historical production date, or preserve `LegacyBatchNumber`.
- HU policy: always generate new HU numbers, or preserve existing legacy HU numbers when labels already exist.
- Quantity policy: validate total import quantity against SAP planned/completed quantity, allow an authorized opening-balance override, or use a separate approved stock-count reference as the source of truth.
- Location policy: derive warehouse from the SAP production order with blank bin like the current flow, or require actual opening-balance warehouse/bin per HU.
- Posting scope: import only creates local GenerateQR/HU/history records, or it must also trigger/represent an SAP inventory transaction.

Risk:
- High if import directly reuses the current Save method: historical orders may be rejected, generated batch dates may not represent legacy stock, large files may be slow, apostrophes can break interpolated SQL, and partial processing can leave sequence gaps or GenerateQR records without complete HU history.
- Medium after introducing staged validation, idempotency, parameterized bulk writes, sequence reservation, and one atomic commit.

Next Action:
- Confirm the five business decisions, then prepare a technical implementation plan for an upload button and validation preview in GenerateQR, an import endpoint/service, staged row results, atomic bulk persistence, downloadable error file, and regression tests. No implementation has been made in this audit.

ETA:
2026-06-25

---

# CONTROL TOWER REPORT

Agent:
Ramos KCI

Project:
KCI Web App

Date:
2026-06-23

Current Task:
Audit and refine GenerateQR print label layout so Machine Number appears to the right of HU Number without broad architecture changes.

Status:
Done

Progress:
100%

Completed:
- Reviewed solution structure, GenerateQR Razor partial, inline print-template CSS, existing local git state, and relevant label markup before editing.
- Updated only `web_app/Views/GenerateQR/_IndexPartial.cshtml` CSS for `.label-body-pallete` and `.label-body-machine`.
- Kept existing HTML structure intact during this update and used a small CSS-only layout adjustment.
- Made HU Number and Machine Number render side-by-side using inline-flex panels, proportional widths, consistent border/padding, aligned titles, and stable print-friendly height.
- Cleaned the affected selector block so label text styling is scoped correctly and easier to review.
- Verified `dotnet build web_app_solution.sln`: Build succeeded with 0 errors.

Issue / Blocker:
- No active blocker.
- Build still reports 109 existing warnings, mainly nullability/reference warnings in infrastructure services and models.
- Working tree also contained pre-existing local changes in `web_app.Infrastructure/Services/ProductionConfirmation/ProductionConfirmationService.cs`; this task did not modify that file.

Need Decision:
- None.

Risk:
- Low. Change is limited to print-template CSS in one Razor partial and does not affect controllers, services, database, routing, or business logic.

Next Action:
- Validate print preview output with real GenerateQR data to confirm the HU Number and Machine Number panels are visually balanced on A4 print.

ETA:
2026-06-23

---

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




