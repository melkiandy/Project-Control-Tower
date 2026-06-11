# Timeline Implementasi Phase 1 - Option A

**Durasi:** 24 minggu (6 bulan)  
**Basis jadwal:** Minggu relatif sejak kickoff  
**Status:** Draft baseline untuk validasi  

## 1. Prinsip Penjadwalan

- Foundation yang sudah tersedia di repository digunakan kembali dan diperkuat.
- Modul domain baru mengikuti pola project `Model -> Service -> API -> Web`.
- Validasi user dilakukan per increment, bukan hanya pada akhir project.
- Advanced enterprise complexity yang dinyatakan deferred tidak dimasukkan ke baseline.
- Tanggal kalender ditetapkan setelah tanggal kickoff disepakati.

## 2. Baseline 24 Minggu

| Minggu | Workstream | Aktivitas utama | Output/milestone |
|---|---|---|---|
| 1-2 | Initiation dan discovery | Kickoff, stakeholder mapping, konfirmasi scope, current-process workshop, inventaris branch/warehouse/user/role, acceptance criteria | Scope baseline, RACI, backlog awal, decision log |
| 2-3 | Architecture dan DevOps baseline | Review repository, branching strategy, environment matrix, secret handling, CI/CD design, database convention, Nginx/topology, backup plan | Architecture baseline dan development environment |
| 3-5 | UI/UX foundation | Information architecture, navigation, design system, prototype administration, review key user | UI/UX foundation approved |
| 3-6 | Auth dan security hardening | Login/session/password policy, user lifecycle, JWT/cookie flow, rate limit/security review, audit event contract | Authentication ready for integration test |
| 4-8 | Organization dan access | User, role, division, menu permission, tenant, tenant type, branch, company profile, access enforcement | Milestone 1: organization and access foundation |
| 7-9 | Shared system services | Error handling, validation, logging, file/config convention, tenant/branch context, reusable table/form pattern | Shared development foundation |
| 8-13 | Master data wave 1 | Product category, unit of measurement, warehouse, customer, supplier | Master data wave 1 ready for SIT |
| 11-15 | Master data wave 2 | Product, price list, relation/validation, import template and initial-data preparation | Milestone 2: master data foundation |
| 13-17 | Approval foundation | Approval configuration dasar, approver assignment, status transition, approval inbox/history | Basic approval ready |
| 14-18 | Notification foundation | SignalR event integration, popup notification, approval/activity notification, unread/read flow, push-provider contract dasar | Realtime notification ready |
| 16-19 | System foundation | Auto numbering, audit log dasar, activity history, basic dashboard cards | Milestone 3: system foundation |
| 17-20 | Finance configuration | COA preparation, account category, payment method, tax config, finance approval setup dasar | Finance configuration foundation |
| 19-21 | Integration dan hardening | Cross-module SIT, authorization matrix test, tenant isolation test, performance/security baseline, defect fixing | Release candidate for UAT |
| 21-22 | UAT cycle 1 | Training key user, scenario execution, issue triage, requirement clarification | UAT findings and fixes |
| 22-23 | UAT cycle 2 dan readiness | Regression, data verification, deployment rehearsal, user guide, admin guide, runbook | Go-live readiness approval |
| 24 | Go-live dan handover | Production deployment, smoke test, hypercare start, source/document handover, retrospective | Phase 1 handover |

## 3. Pemetaan Workstream ke Solution

| Workstream | Project existing yang dipakai | Project/module yang direkomendasikan |
|---|---|---|
| Auth, user, tenant | `Application.API.Account`, `Application.Service.Account`, `Application.Model.Account`, `Application.Web.Admin` | Lanjutkan dan lengkapi modul existing |
| Master data | `Infrastructure.Data`, `Core.Common`, reusable admin components | `Application.Model.MasterData`, `Application.Service.MasterData`, `Application.API.MasterData`, area Web Admin |
| Approval | Identity/tenant context, notification infrastructure | `Application.Model.Workflow`, `Application.Service.Workflow`, `Application.API.Workflow`, area Web Admin |
| Notification | `Application.API.Notification`, SignalR hub, Redis | Integrasi event dari MasterData/Workflow; rapikan duplikasi notification implementation |
| Audit/activity | Logging dan current-user context | Shared audit contract di `Core.Common`, persistence di `Infrastructure.Data` |
| Auto numbering | Database abstraction dan tenant context | Shared numbering service dengan konfigurasi per tenant/branch/document |
| Finance foundation | Database abstraction dan access foundation | Module configuration terpisah agar tidak dianggap full accounting |
| Dashboard foundation | `Application.Web.Admin` | Query/read model Phase 1 dan dashboard cards dasar |
| Deployment | Docker Compose existing | Nginx, health checks, secrets, backup, CI/CD, staging/production profiles |

## 4. Milestone Komersial yang Disarankan

| Milestone | Target | Bukti penerimaan | Payment reference |
|---|---|---|---|
| Project initiation | Minggu 1 | Kickoff dan scope baseline | 20% |
| UI/UX dan foundation progress | Minggu 6-8 | Prototype, auth, organization/access demo | 30% |
| Development progress | Minggu 16-18 | Master data, approval, notification demo | 30% |
| Go-live dan handover | Minggu 24 | UAT sign-off, production release, handover | 20% |

Payment milestone final tetap mengikuti dokumen kontrak yang disepakati.

## 5. Critical Path

1. Scope dan organization model harus disepakati sebelum permission matrix dikunci.
2. Tenant, branch, dan user context harus stabil sebelum master data dibangun penuh.
3. Approval scenarios harus disepakati sebelum approval dan notification diintegrasikan.
4. Data template harus tersedia sebelum UAT.
5. Infrastruktur staging harus siap sebelum integration hardening.
6. UAT sign-off menjadi prasyarat go-live.

## 6. Risiko terhadap Target 24 Minggu

| Risiko | Dampak | Mitigasi |
|---|---|---|
| Requirement bisnis terlambat/frequently changing | Rework dan pergeseran milestone | Weekly decision forum dan change-control |
| Jumlah branch/role/approval belum jelas | Desain access dan data model berubah | Discovery wajib selesai pada Minggu 2 |
| Multi-level approval masuk kembali | Melebihi optimasi Option A | Jadikan change request atau tukar prioritas |
| Basic mobile push belum memiliki provider | Integrasi tidak dapat divalidasi end-to-end | Tetapkan provider dan credential sebelum Minggu 14 |
| Data master tidak siap | UAT tertunda | Template dan owner data ditetapkan Minggu 8 |
| Repository belum memiliki automated tests | Regression risk tinggi | Tambah test project sejak Minggu 3 |
| Docker config masih development-oriented | Risiko staging/production | Pisahkan configuration dan secret management |

## 7. Definition of Done per Modul

- Requirement dan acceptance criteria disetujui.
- Code review selesai.
- Unit/integration test relevan lulus.
- Authorization dan tenant isolation tervalidasi.
- Logging dan audit event tersedia.
- Functional QA lulus.
- Dokumentasi pengguna dan teknis diperbarui.
- Demo/validasi key user selesai.

## 8. Keputusan yang Diperlukan sebelum Baseline Dikunci

- Tanggal kickoff.
- Nominal final dan payment trigger.
- Daftar key user dan jalur eskalasi.
- Organization/tenant/branch model.
- Permission matrix awal.
- Prioritas master data.
- Daftar approval dasar.
- Provider push notification.
- Environment dan tanggung jawab infrastruktur.
- UAT signatory dan target response time untuk issue.

