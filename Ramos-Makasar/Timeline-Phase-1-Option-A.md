# Timeline Implementasi Phase 1 - Option A

**Durasi:** 24 minggu (6 bulan)  
**Basis jadwal:** Minggu relatif sejak kickoff  
**Status dokumen:** Draft baseline untuk validasi  
**Status pengerjaan:** Planning / Pre-Kickoff  
**Progress implementasi:** 0%  
**Terakhir diperbarui:** 12 Juni 2026  

## 1. Ringkasan Status

| Area | Status | Progress | Catatan |
|---|---|---:|---|
| Dokumen scope dan timeline | In Review | 80% | Menunggu validasi stakeholder dan keputusan final |
| Project initiation | Planning | 0% | Kickoff belum ditetapkan |
| Development Phase 1 | Not Started | 0% | Dimulai setelah scope baseline disetujui |
| UAT dan go-live | Not Started | 0% | Mengikuti hasil development dan readiness review |
| Overall Phase 1 | Planning / Pre-Kickoff | 0% | Progress implementasi dihitung mulai kickoff resmi |

Fondasi teknis yang sudah tersedia di repository merupakan aset awal dan belum dihitung sebagai progress implementasi Phase 1 sampai scope, acceptance criteria, serta baseline backlog disetujui.

## 2. Definisi Status

| Status | Arti |
|---|---|
| Not Started | Aktivitas belum dimulai |
| Planning | Requirement, dependency, atau rencana kerja sedang disiapkan |
| In Progress | Aktivitas sedang dikerjakan |
| In Review | Output sedang diperiksa atau menunggu persetujuan |
| Testing | Aktivitas berada pada tahap SIT, QA, atau UAT |
| Blocked | Aktivitas tidak dapat dilanjutkan karena dependency atau keputusan |
| Done | Acceptance criteria aktivitas telah terpenuhi |
| On Hold | Aktivitas dihentikan sementara berdasarkan keputusan project |

## 3. Prinsip Penjadwalan

- Foundation yang sudah tersedia di repository digunakan kembali dan diperkuat.
- Modul domain baru mengikuti pola project `Model -> Service -> API -> Web`.
- Validasi user dilakukan per increment, bukan hanya pada akhir project.
- Advanced enterprise complexity yang dinyatakan deferred tidak dimasukkan ke baseline.
- Tanggal kalender ditetapkan setelah tanggal kickoff disepakati.
- Progress hanya diperbarui berdasarkan bukti penyelesaian dan acceptance criteria.

## 4. Baseline dan Status 24 Minggu

| Minggu | Workstream | Aktivitas utama | Output/milestone | Status | Progress |
|---|---|---|---|---|---:|
| 1-2 | Initiation dan discovery | Kickoff, stakeholder mapping, konfirmasi scope, current-process workshop, inventaris branch/warehouse/user/role, acceptance criteria | Scope baseline, RACI, backlog awal, decision log | Planning | 0% |
| 2-3 | Architecture dan DevOps baseline | Review repository, branching strategy, environment matrix, secret handling, CI/CD design, database convention, Nginx/topology, backup plan | Architecture baseline dan development environment | Not Started | 0% |
| 3-5 | UI/UX foundation | Information architecture, navigation, design system, prototype administration, review key user | UI/UX foundation approved | Not Started | 0% |
| 3-6 | Auth dan security hardening | Login/session/password policy, user lifecycle, JWT/cookie flow, rate limit/security review, audit event contract | Authentication ready for integration test | Not Started | 0% |
| 4-8 | Organization dan access | User, role, division, menu permission, tenant, tenant type, branch, company profile, access enforcement | Milestone 1: organization and access foundation | Not Started | 0% |
| 7-9 | Shared system services | Error handling, validation, logging, file/config convention, tenant/branch context, reusable table/form pattern | Shared development foundation | Not Started | 0% |
| 8-13 | Master data wave 1 | Product category, unit of measurement, warehouse, customer, supplier | Master data wave 1 ready for SIT | Not Started | 0% |
| 11-15 | Master data wave 2 | Product, price list, relation/validation, import template and initial-data preparation | Milestone 2: master data foundation | Not Started | 0% |
| 13-17 | Approval foundation | Approval configuration dasar, approver assignment, status transition, approval inbox/history | Basic approval ready | Not Started | 0% |
| 14-18 | Notification foundation | SignalR event integration, popup notification, approval/activity notification, unread/read flow, push-provider contract dasar | Realtime notification ready | Not Started | 0% |
| 16-19 | System foundation | Auto numbering, audit log dasar, activity history, basic dashboard cards | Milestone 3: system foundation | Not Started | 0% |
| 17-20 | Finance configuration | COA preparation, account category, payment method, tax config, finance approval setup dasar | Finance configuration foundation | Not Started | 0% |
| 19-21 | Integration dan hardening | Cross-module SIT, authorization matrix test, tenant isolation test, performance/security baseline, defect fixing | Release candidate for UAT | Not Started | 0% |
| 21-22 | UAT cycle 1 | Training key user, scenario execution, issue triage, requirement clarification | UAT findings and fixes | Not Started | 0% |
| 22-23 | UAT cycle 2 dan readiness | Regression, data verification, deployment rehearsal, user guide, admin guide, runbook | Go-live readiness approval | Not Started | 0% |
| 24 | Go-live dan handover | Production deployment, smoke test, hypercare start, source/document handover, retrospective | Phase 1 handover | Not Started | 0% |

## 5. Pemetaan Workstream ke Solution

| Workstream | Project existing yang dipakai | Project/module yang direkomendasikan | Readiness |
|---|---|---|---|
| Auth, user, tenant | `Application.API.Account`, `Application.Service.Account`, `Application.Model.Account`, `Application.Web.Admin` | Lanjutkan dan lengkapi modul existing | Foundation Available |
| Master data | `Infrastructure.Data`, `Core.Common`, reusable admin components | `Application.Model.MasterData`, `Application.Service.MasterData`, `Application.API.MasterData`, area Web Admin | New Module Required |
| Approval | Identity/tenant context, notification infrastructure | `Application.Model.Workflow`, `Application.Service.Workflow`, `Application.API.Workflow`, area Web Admin | New Module Required |
| Notification | `Application.API.Notification`, SignalR hub, Redis | Integrasi event dari MasterData/Workflow; rapikan duplikasi notification implementation | Foundation Available |
| Audit/activity | Logging dan current-user context | Shared audit contract di `Core.Common`, persistence di `Infrastructure.Data` | Partial Foundation |
| Auto numbering | Database abstraction dan tenant context | Shared numbering service dengan konfigurasi per tenant/branch/document | New Service Required |
| Finance foundation | Database abstraction dan access foundation | Module configuration terpisah agar tidak dianggap full accounting | New Module Required |
| Dashboard foundation | `Application.Web.Admin` | Query/read model Phase 1 dan dashboard cards dasar | UI Foundation Available |
| Deployment | Docker Compose existing | Nginx, health checks, secrets, backup, CI/CD, staging/production profiles | Partial Foundation |

## 6. Milestone Komersial yang Disarankan

| Milestone | Target | Bukti penerimaan | Payment reference | Status |
|---|---|---|---|---|
| Project initiation | Minggu 1 | Kickoff dan scope baseline | 20% | Planning |
| UI/UX dan foundation progress | Minggu 6-8 | Prototype, auth, organization/access demo | 30% | Not Started |
| Development progress | Minggu 16-18 | Master data, approval, notification demo | 30% | Not Started |
| Go-live dan handover | Minggu 24 | UAT sign-off, production release, handover | 20% | Not Started |

Payment milestone final tetap mengikuti dokumen kontrak yang disepakati.

## 7. Critical Path

1. Scope dan organization model harus disepakati sebelum permission matrix dikunci.
2. Tenant, branch, dan user context harus stabil sebelum master data dibangun penuh.
3. Approval scenarios harus disepakati sebelum approval dan notification diintegrasikan.
4. Data template harus tersedia sebelum UAT.
5. Infrastruktur staging harus siap sebelum integration hardening.
6. UAT sign-off menjadi prasyarat go-live.

## 8. Risiko terhadap Target 24 Minggu

| Risiko | Dampak | Mitigasi | Status |
|---|---|---|---|
| Requirement bisnis terlambat/frequently changing | Rework dan pergeseran milestone | Weekly decision forum dan change-control | Open |
| Jumlah branch/role/approval belum jelas | Desain access dan data model berubah | Discovery wajib selesai pada Minggu 2 | Open |
| Multi-level approval masuk kembali | Melebihi optimasi Option A | Jadikan change request atau tukar prioritas | Open |
| Basic mobile push belum memiliki provider | Integrasi tidak dapat divalidasi end-to-end | Tetapkan provider dan credential sebelum Minggu 14 | Open |
| Data master tidak siap | UAT tertunda | Template dan owner data ditetapkan Minggu 8 | Open |
| Repository belum memiliki automated tests | Regression risk tinggi | Tambah test project sejak Minggu 3 | Open |
| Docker config masih development-oriented | Risiko staging/production | Pisahkan configuration dan secret management | Open |

## 9. Definition of Done per Modul

- Requirement dan acceptance criteria disetujui.
- Code review selesai.
- Unit/integration test relevan lulus.
- Authorization dan tenant isolation tervalidasi.
- Logging dan audit event tersedia.
- Functional QA lulus.
- Dokumentasi pengguna dan teknis diperbarui.
- Demo/validasi key user selesai.

## 10. Mekanisme Update Status

- Status diperbarui minimal satu kali setiap minggu.
- Progress workstream dihitung berdasarkan acceptance criteria yang selesai, bukan berdasarkan waktu yang telah digunakan.
- Status `Blocked` wajib mencantumkan blocker, owner keputusan, dan target penyelesaian.
- Perubahan baseline minggu atau scope harus tercatat dalam decision log/change request.
- Overall progress dihitung setelah kickoff resmi dan menggunakan bobot workstream yang disepakati.

## 11. Keputusan yang Diperlukan sebelum Baseline Dikunci

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

