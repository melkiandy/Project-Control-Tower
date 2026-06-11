# BERITA ACARA DOKUMENTASI
## Kesepakatan Awal Implementasi Phase 1 - Foundation System

**Project:** Sistem Distribusi dan Sales Management  
**Klien:** PT Reski Laifasto (PT Resto) dan PT Prima Larsa Mitra Niaga (PKMN)  
**Penyedia:** PT Pilar Sinergi Integrasi  
**Dokumen:** Draft untuk validasi dan diskusi  
**Tanggal dokumentasi:** 11 Juni 2026  

## 1. Latar Belakang

Berita Acara Dokumentasi ini dibuat untuk mencatat pemahaman awal mengenai pilihan komersial dan ruang lingkup implementasi Phase 1 Project Sistem Distribusi dan Sales Management.

Berdasarkan informasi yang disampaikan, Pak Hadi telah menyetujui **Option A - 6 Months Development** dari dokumen *Recommended Commercial Proposal Strategy*. Dokumen ini menjadi dasar pembahasan lebih lanjut sebelum kickoff, finalisasi kebutuhan, dan pengesahan dokumen kontraktual.

## 2. Referensi Dokumen

1. *Application Blueprint & Development Plan - Sistem Distribusi dan Sales Management*.
2. *Commercial Proposal - System Distribution & Sales Management System*.
3. *Scope of Work (SOW) & Initial Scope Discussion - Phase 1*.
4. *Recommended Commercial Proposal Strategy*.
5. Repository `melkiandy/project-saas`, branch utama `main`.

## 3. Kesepakatan yang Didokumentasikan

| Item | Hasil dokumentasi |
|---|---|
| Strategi implementasi | Bertahap berdasarkan phase |
| Phase yang disetujui | Phase 1 - Foundation System |
| Opsi komersial | Option A - 6 Months Development |
| Durasi target | 24 minggu |
| Estimasi investasi Option A | Rp158.000.000,00-Rp162.000.000,00, belum termasuk PPN |
| Nilai kontrak final | Menunggu konfirmasi tertulis |
| Skema pembayaran rekomendasi | 20% initiation, 30% UI/UX & foundation, 30% development progress, 20% go-live & handover |
| Tanggal kickoff | Menunggu penetapan |
| Model pengerjaan | Hybrid: online discussion, remote development, dan workshop operasional |

Persetujuan Option A tidak dengan sendirinya menetapkan nominal final di dalam rentang investasi, tanggal kickoff, biaya onsite, atau perubahan ruang lingkup. Item tersebut perlu dikonfirmasi dalam dokumen komersial/kontrak final.

## 4. Ruang Lingkup Phase 1

### 4.1 Authentication dan Security

- Login dan logout.
- Session management.
- Password management.
- User authentication.

### 4.2 User dan Access Management

- User management.
- Role dan permission.
- Division access.
- Menu access configuration.

### 4.3 Tenant dan Company Setup

- Tenant management.
- Tenant type.
- Branch setup.
- Company profile.
- Struktur organisasi fleksibel.

### 4.4 Master Data Foundation

- Customer.
- Supplier.
- Product.
- Warehouse.
- Product category.
- Unit of measurement.
- Price list.

### 4.5 Approval Foundation

- Approval configuration dasar.
- Approval workflow dasar.
- Notifikasi approval.

Multi-level approval dan workflow orchestration kompleks ditunda berdasarkan strategi optimasi Option A.

### 4.6 Notification Foundation

- Realtime popup notification.
- Activity notification.
- Approval notification.
- Basic mobile push notification sebagai pondasi integrasi, dengan kanal dan target perangkat dikonfirmasi saat elaborasi.

### 4.7 System Foundation

- Auto numbering document.
- Audit log dasar.
- Activity history.
- Basic dashboard foundation.

### 4.8 Finance Foundation

- Chart of account preparation.
- Finance account category.
- Payment method setup.
- Tax configuration.
- Finance approval setup dasar.

Finance foundation hanya mencakup konfigurasi awal. Phase 1 tidak mencakup full accounting, general ledger, financial statement, automatic journal posting, closing period, advanced tax reporting, costing, atau advanced financial analytics.

## 5. Ruang Lingkup yang Ditunda

- Purchase, sales, warehouse transaction, stock transaction, delivery, invoice, dan payment.
- Full finance dan accounting.
- Mobile Sales Force Automation.
- GPS tracking.
- KPI dan executive dashboard.
- Advanced analytics dan reporting.
- Advanced audit visualization.
- Multi-level approval.
- Complex workflow automation dan enterprise operational monitoring.

Fitur di atas menjadi kandidat Phase 2-4 atau change request terpisah.

## 6. Pemetaan terhadap Repository

Repository saat ini telah menyediakan fondasi teknis yang relevan:

| Area SOW | Kondisi repository | Tindak lanjut |
|---|---|---|
| Authentication | API login/logout/refresh JWT dan cookie session web tersedia | Hardening password lifecycle, session policy, dan test |
| User/Role | ASP.NET Core Identity dan role seeding tersedia | Tambah UI/API user, role, division, dan permission |
| Tenant | Model, seeder, service, API, dan halaman tenant/tenant type tersedia sebagian | Selesaikan branch/company profile dan validasi hierarchy |
| Menu access | Struktur tabel/menu access tersedia | Implementasikan konfigurasi dan enforcement permission |
| Notification | SignalR API/hub dan Redis tersedia | Hubungkan ke event approval/activity dan notification center |
| Database | PostgreSQL dan SQL Server abstraction tersedia | PostgreSQL menjadi target utama; tambah schema domain Phase 1 |
| Master data | Belum tersedia sebagai modul bisnis | Bangun model, service, API, UI, validation, dan seeder |
| Approval | Belum tersedia sebagai modul | Bangun approval dasar yang dapat diperluas pada phase berikut |
| Audit/activity | Belum tersedia secara lengkap | Tambah audit trail lintas modul dan activity history |
| Auto numbering | Belum tersedia | Tambah konfigurasi sequence per tenant/branch/document |
| Dashboard | Halaman template tersedia, belum menjadi dashboard operasional | Bangun dashboard foundation berbasis data Phase 1 |
| Deployment | Docker Compose, PostgreSQL, Redis, API, dan web tersedia | Tambah Nginx, environment hardening, backup, CI/CD, dan observability |
| Mobile push | Belum ada mobile project/provider push | Tetapkan provider dan buat integration contract dasar |

Struktur solution yang akan dipertahankan:

- `Application.API.Account`: endpoint account dan foundation administration.
- `Application.Service.Account`: business service account/tenant.
- `Application.Model.Account`: request, response, dan model account.
- `Application.API.Notification`: realtime notification.
- `Application.Web.Admin`: web administration.
- `Infrastructure.Data`: database, identity, seeder, notification, dan multi-provider access.
- `Core.Common`: shared response, helper, logging, dan configuration.

Untuk menjaga batas domain, master data dan approval sebaiknya dibuat sebagai project/module baru mengikuti pola `Model -> Service -> API -> Web`, bukan menumpuk seluruh fitur ke Account.

## 7. Kriteria Penyelesaian Phase 1

Phase 1 dinyatakan siap untuk UAT apabila:

- Scope prioritas telah melewati functional test dan integration test.
- Hak akses tervalidasi berdasarkan user, role, division, tenant, dan branch.
- Master data utama dapat dikelola melalui web.
- Approval dasar, notification, audit, dan auto numbering berjalan pada skenario yang disepakati.
- Data antar-tenant/branch terisolasi sesuai desain.
- Deployment staging tersedia dan dapat direproduksi.
- UAT issue kritis telah ditutup.
- Dokumen teknis, user guide, deployment guide, dan handover tersedia.

## 8. Asumsi dan Ketergantungan

- Klien menyediakan PIC bisnis dan key user tiap area.
- Keputusan requirement diberikan sesuai jadwal validasi.
- Data awal dan format migrasi disediakan sebelum tahap data preparation.
- Infrastruktur VPS/domain/SSL dan akses terkait tersedia sebelum staging/go-live.
- Integrasi pihak ketiga di luar dokumen menjadi change request.
- Perubahan flow bisnis yang memengaruhi desain atau estimasi diproses melalui change request.

## 9. Poin yang Wajib Dikonfirmasi

1. Entitas legal klien yang menjadi pihak kontrak.
2. Nominal final Option A dalam rentang Rp158-162 juta.
3. PPN dan ketentuan invoice.
4. Tanggal kickoff dan target go-live.
5. Nama lengkap dan jabatan Pak Hadi serta para penandatangan.
6. Jumlah tenant, company, branch, warehouse, division, role, dan user awal.
7. Prioritas master data dan urutan UAT.
8. Approval dasar yang benar-benar masuk Phase 1.
9. Provider basic mobile push notification.
10. Biaya serta mekanisme onsite visit.
11. Masa warranty, SLA support, source-code handover, dan ownership.

## 10. Persetujuan

Dokumen ini merupakan catatan pemahaman dan bahan diskusi. Dokumen menjadi berita acara final setelah poin konfirmasi dilengkapi dan disetujui para pihak.

| Pihak Klien | Pihak Penyedia |
|---|---|
| Nama: ____________________ | Nama: ____________________ |
| Jabatan: __________________ | Jabatan: __________________ |
| Tanggal: __________________ | Tanggal: __________________ |
| Tanda tangan: _____________ | Tanda tangan: _____________ |

