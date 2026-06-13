# Pegangan Meeting Sesi 1 - Project KCI Add On Web

**Meeting:** Pak Hadi dan Tim  
**Jadwal:** Sabtu, 13 Juni 2026 - Pukul 09:00 WIB  
**Presenter:** Melkiandy Bessie  
**Agent:** Ramos KCI  
**Repository:** `melkiandy/web_app_solution`  
**Branch utama:** `master`  

## 1. Konteks yang Benar

Dokumen ini digunakan untuk **Sesi 1: Show Project KCI**, bukan untuk Visitor Management System.

Project yang akan ditunjukkan adalah **KCI Add On Web**, yaitu aplikasi web operasional yang dibuat untuk membantu proses produksi, gudang, handling unit, sales return, shipment, delivery, dan integrasi SAP Business One.

**Catatan presenter:** jangan menggunakan narasi VMS, visitor registration, face recognition, kamera, swing barrier, atau device visitor. Itu milik agent/project lain, bukan bahan Sesi 1 Ramos KCI.

## 2. Tujuan Presentasi

Tujuan sesi pertama adalah menunjukkan bahwa Project KCI sudah memiliki fondasi aplikasi operasional yang cukup luas, antara lain:

- Login, session, user management, menu management, dan menu access.
- Dashboard / home sebagai pintu masuk aplikasi.
- Modul proses produksi dan warehouse berbasis Handling Unit.
- Modul scan dari production, production confirmation, warehouse in, warehouse out, scan bin to bin, dan scan sales return.
- Modul shipment, pick list, plan delivery, delivery order, transfer cost, dan document print.
- Integrasi SAP Business One melalui SAP Service Layer dan SQL query pendukung.
- Real-time notification menggunakan SignalR Notification Hub.
- Logging, rate limiting, dan middleware token/session.

## 3. Narasi Pembuka untuk Pak Hadi

> Pada sesi pertama ini, saya akan menunjukkan Project KCI dari repository `melkiandy/web_app_solution`. Project ini bukan Visitor Management System, melainkan KCI Add On Web yang berfungsi sebagai aplikasi operasional pendukung SAP Business One. Fokusnya adalah bagaimana proses produksi, handling unit, warehouse, shipment, delivery, dan sales return dapat dibantu melalui aplikasi web yang terintegrasi dengan database dan SAP Service Layer.

## 4. Ringkasan Teknis Project

| Area | Penjelasan |
|---|---|
| Repository | `melkiandy/web_app_solution` |
| Deskripsi repository | KCI Add On Web |
| Branch utama | `master` |
| Solution | `web_app_solution.sln` |
| Project utama | `web_app` |
| Project pendukung | `web_app.Infrastructure`, `web_app.Domain` |
| Framework | ASP.NET Core MVC / .NET 8 |
| Database & ORM | Entity Framework Core, SQL Server, PostgreSQL/Npgsql support |
| Integrasi utama | SAP Business One Service Layer dan SAP SQL Queries |
| Frontend | Razor View, Bootstrap/Admin template, DataTables, Select2, SweetAlert |
| Realtime | SignalR Notification Hub `/hubs/notification` |
| Security | ASP.NET Identity, TokenAuthenticationMiddleware, authorization, rate limiting |
| Logging | Serilog file/console logging |
| QR / barcode support | ZXing.Net untuk kebutuhan generate/scan QR/handling unit |

## 5. Alur Sistem yang Perlu Dijelaskan

1. **User login ke KCI Admin.**  
   Sistem melakukan validasi username/password, rate limit login, dan mengembalikan redirect/token/session.

2. **User masuk ke Home/Dashboard.**  
   Home menjadi shell utama aplikasi. Konten dapat dimuat sebagai full view atau partial view untuk kebutuhan AJAX.

3. **Menu dan akses dibentuk berdasarkan hak user.**  
   Menu, submenu, dan permission dikontrol melalui Menu Management dan Menu Access.

4. **User menjalankan proses operasional.**  
   Contoh proses yang bisa ditunjukkan: Scan From Production, Production Confirmation, Scan Bin To Bin, Sales Return, Delivery Order.

5. **Aplikasi mengambil/mengirim data ke SAP.**  
   Beberapa proses memakai SAP Service Layer seperti Production Order, Inventory Gen Entry/Exit, Batch Master Data, Bin Locations, Stock Transfer, Sales Return, Delivery Notes, dan Sales Order.

6. **Aplikasi mencatat transaksi dan memberi notifikasi.**  
   Proses tertentu dapat mengirim notification payload ke Notification Hub agar user terkait mendapat informasi lanjutan.

## 6. Flow Demo yang Disarankan

Gunakan flow yang aman dan mudah dipahami oleh Pak Hadi:

1. **Login** ke aplikasi KCI Admin.
2. Tunjukkan **Home/Dashboard** dan jelaskan bahwa ini adalah pusat operasional.
3. Tunjukkan **Menu Management** dan **Menu Access** untuk menjelaskan foundation security.
4. Tunjukkan **User Management** untuk menjelaskan pengelolaan user internal.
5. Tunjukkan salah satu proses inti produksi: **Scan From Production**.
6. Lanjutkan ke **Production Confirmation** untuk menjelaskan proses lanjutan setelah scan produksi.
7. Tunjukkan **Scan Bin To Bin** sebagai contoh perpindahan barang/bin/handling unit.
8. Tunjukkan **Scan Sales Return** sebagai contoh proses retur yang terhubung ke SAP Return Request/Return.
9. Tunjukkan **Plan Delivery / Delivery Order** untuk menjelaskan alur distribusi/shipment.
10. Tutup dengan penjelasan value bisnis dan risiko teknis yang perlu dikontrol sebelum demo live.

## 7. Penjelasan View / Modul untuk Presentasi

| View / Modul | Fungsi Utama | Narasi Presentasi |
|---|---|---|
| Login | Autentikasi user, validasi form, rate limit login, redirect ke Home. | Ini pintu masuk aplikasi. Setelah login sukses, user diarahkan ke Home dan mendapatkan session/token untuk akses berikutnya. |
| Home / Dashboard | Halaman utama aplikasi dan ringkasan Plan Delivery. | Ini landing page setelah login. Dari sini user masuk ke modul-modul operasional. |
| Menu Management | Mengelola menu, parent menu, icon, document type, dan struktur navigasi. | Menu aplikasi tidak hardcode penuh; dapat dikelola agar modul muncul sesuai kebutuhan. |
| Menu Access | Mengatur permission per user/menu/action. | Ini fondasi kontrol akses: siapa boleh melihat, membuat, mengubah, atau menjalankan proses tertentu. |
| User Management | List user, create/edit/cancel user, aktivasi password via email. | Admin bisa mengelola user internal dan mengirim aktivasi password. |
| Document Number | Mengatur format dan nomor dokumen internal. | Modul ini penting agar dokumen operasional memiliki nomor yang konsisten dan bisa ditelusuri. |
| Generate QR | Membantu pembuatan QR/label untuk proses handling unit. | QR dipakai agar proses scan lebih cepat dan mengurangi input manual. |
| Handling Unit Tracking | Melihat dan menelusuri pergerakan Handling Unit. | View ini menjawab pertanyaan: HU ini sekarang berada di mana dan status prosesnya apa. |
| Scan From Production | Scan hasil produksi dan validasi production order/batch/bin melalui SAP. | Ini contoh proses lantai produksi: hasil produksi discan lalu disiapkan untuk proses confirmation. |
| Production Confirmation | Konfirmasi produksi dengan data production order, receipt order, shift, group, HU detail. | Modul ini menjadi checkpoint penting sebelum transaksi produksi dilanjutkan ke SAP. |
| Positive Released | Mengelola status released/blocked serta reason yang relevan. | View ini membantu quality/warehouse memastikan item bisa diproses atau perlu ditahan. |
| Warehouse In | Proses penerimaan/masuk gudang. | Digunakan untuk memastikan barang/HU masuk ke lokasi yang benar. |
| Repack Order | Proses repack handling unit. | Digunakan ketika barang perlu dikemas ulang atau diproses ulang sebelum lanjut. |
| Scan Sales Return | Proses scan retur penjualan dan integrasi ke SAP Return Request/Return. | Ini menunjukkan bagaimana retur dapat divalidasi dan diproses melalui aplikasi, bukan manual penuh. |
| Scan Bin To Bin | Perpindahan barang/HU antar bin menggunakan SAP Bin Locations dan Stock Transfer. | Ini contoh proses warehouse yang paling mudah dipahami: barang dipindahkan dari bin asal ke bin tujuan. |
| Warehouse Out | Proses keluar gudang. | Digunakan ketika barang/HU sudah siap dikirim atau dipindahkan keluar dari warehouse. |
| Transfer Cost | Pengelolaan biaya transfer. | Modul ini mendukung kontrol biaya atas proses perpindahan/logistik. |
| Shipment Complete | Menandai shipment selesai. | Menjadi checkpoint akhir bahwa pengiriman sudah lengkap dan siap ditindaklanjuti. |
| Pick List | Menyiapkan daftar picking. | Membantu gudang mengambil item sesuai kebutuhan shipment/delivery. |
| Plan Delivery | Perencanaan delivery dan summary delivery. | Ini modul yang baik untuk menjelaskan hubungan antara rencana pengiriman dan realisasi. |
| Shipment Delivery | Eksekusi pengiriman berdasarkan rencana. | Menghubungkan proses shipment dengan data delivery operasional. |
| Delivery Order | Membuat delivery order, unassign HU, dan integrasi SAP Delivery Notes/Sales Order. | Ini modul penting untuk menjelaskan output akhir proses distribusi. |
| Transfer Cost Delivery | Biaya transfer terkait delivery. | Dipakai untuk kontrol cost pada delivery/shipment. |
| Document Print | Cetak dokumen operasional. | Memudahkan user menghasilkan dokumen pendukung setelah proses selesai. |
| Notification | Notifikasi real-time untuk proses lanjutan. | Beberapa proses dapat mengirim notifikasi agar user terkait tahu ada pekerjaan berikutnya. |

## 8. Value Bisnis yang Harus Ditekankan

- Mengurangi proses manual antara operasional lapangan dan SAP.
- Mempercepat validasi data produksi, warehouse, shipment, delivery, dan return.
- Mengurangi risiko salah input karena penggunaan scan/QR/handling unit.
- Meningkatkan traceability transaksi dan status handling unit.
- Menyediakan fondasi user access, menu access, logging, dan notification.
- Membuat proses SAP lebih mudah digunakan oleh user operasional melalui tampilan web.

## 9. Batasan yang Perlu Disampaikan dengan Hati-Hati

Sampaikan sebagai catatan teknis, bukan sebagai kelemahan project:

- Demo live bergantung pada koneksi ke database, SAP Service Layer, dan konfigurasi environment.
- Jika Service Layer atau firewall/network belum stabil, siapkan data/screenshot cadangan.
- Beberapa fitur membutuhkan master data SAP dan data transaksi yang valid.
- Role user demo harus memiliki menu access yang sesuai agar semua menu tampil.
- Pastikan browser, IIS, certificate, dan URL API sudah sesuai environment demo.

## 10. Checklist Sebelum Meeting

| Checklist | Status |
|---|---|
| Pastikan repo yang dipakai adalah `melkiandy/web_app_solution`. | Wajib |
| Pastikan tidak menyebut VMS/Visitor Management System. | Wajib |
| Pastikan aplikasi bisa login. | Wajib |
| Pastikan user demo punya menu access yang cukup. | Wajib |
| Pastikan Service Layer SAP bisa diakses dari environment demo. | Wajib |
| Siapkan screenshot cadangan untuk modul penting. | Disarankan |
| Siapkan contoh HU / batch / production order / sales return / delivery data. | Disarankan |
| Pastikan SignalR notification tidak error di browser console. | Disarankan |

## 11. Narasi Penutup

> Kesimpulannya, Project KCI ini adalah add-on web untuk membantu proses operasional yang berhubungan dengan SAP Business One. Nilainya bukan hanya di tampilan web, tetapi di integrasi proses: mulai dari production, handling unit, warehouse, sales return, shipment, delivery, sampai notifikasi dan kontrol akses. Untuk meeting ini saya akan fokus menunjukkan alur yang sudah siap didemokan dan menjelaskan bagian mana yang masih perlu validasi data, environment, dan koneksi SAP sebelum dipakai lebih luas.

## 12. Next Action Setelah Meeting

- Catat feedback Pak Hadi dan tim untuk modul yang diprioritaskan.
- Tentukan modul mana yang masuk demo lanjutan atau UAT.
- Validasi koneksi SAP Service Layer dan SQL query di environment target.
- Rapikan role/menu access untuk user demo dan user operasional.
- Buat daftar issue teknis dari browser console, IIS, Cato/firewall, atau SAP response bila muncul saat demo.
- Update Daily Report Ramos KCI/Ramos Makasar sesuai pembagian agent bila diperlukan.
