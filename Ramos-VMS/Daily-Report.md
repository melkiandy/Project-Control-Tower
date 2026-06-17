# CONTROL TOWER REPORT

Agent:
Ramos VMS

Project:
Application VMS HM

Date:
2026-06-11

Current Task:
Mengaktifkan SOP Ramos Control Tower dan melaporkan penyelesaian rollback perubahan base URL global pada frontend VMS.

Status:
Done

Progress:
100%

Completed:
- Membatalkan penambahan app-config.js.
- Menghapus referensi app-config.js dari pages/home.html dan pages/accounts/login.html.
- Memastikan tidak ada perubahan arsitektur atau source lain yang ikut terubah.
- Menjalankan dotnet build untuk seluruh solution dengan hasil 0 warning dan 0 error.
- Mengaktifkan alur pelaporan Ramos Control Tower untuk task berikutnya.

Issue / Blocker:
- Build awal di sandbox gagal karena akses tulis ke cache static web assets pada folder obj ditolak.
- Blocker terselesaikan dengan menjalankan build menggunakan izin yang sesuai.

Need Decision:
-

Risk:
- Issue URL absolut pada deployment IIS kembali ke kondisi sebelum perubahan dan belum memiliki solusi pengganti.

Next Action:
- Menunggu task VMS berikutnya dan memperbarui laporan ini pada setiap task selesai, blocker, root cause, atau issue resolution.

ETA:
2026-06-11

---

# CONTROL TOWER REPORT

Agent:
Ramos VMS

Project:
Application VMS HM

Date:
2026-06-18

Current Task:
Menghapus popup session timeout dan mengganti flow enroll ID visitor agar diambil dari endpoint device get_ladt_id lalu disimpan pada relasi VisitorDevice.

Status:
Done

Progress:
100%

Completed:
- Mengganti session timeout popup menjadi redirect langsung ke /pages/accounts/login.html.
- Mengubah DeviceRecognitionService.GetEnrollId menjadi request HTTP ke endpoint get_ladt_id dengan payload password dari device password dan cmd dari device username.
- Menambahkan model response enroll ID device dan konfigurasi GetEnrollIdEndpointPath.
- Menambahkan properti EnrollId nullable pada VisitorDevice dan mapping kolom EF.
- Menyimpan enroll ID hasil response device ke VisitorDevice saat create/update visitor.
- Menggunakan enroll ID tersimpan untuk request SaveVisitor dan DeleteVisitor ke device.
- Menjalankan dotnet build Application_VMS_HM.sln dengan hasil sukses, 0 warning, 0 error.

Issue / Blocker:
- Build pertama di sandbox gagal karena akses NuGet/network dan file obj dibatasi; build ulang dengan izin yang sesuai berhasil.

Need Decision:
- Konfirmasi deploy DB existing untuk menambahkan kolom nullable EnrollId pada tabel crm.Visitor_Device karena repo belum memiliki folder migrations existing.

Risk:
- Data visitor lama yang belum memiliki EnrollId tersimpan tidak akan dikirim delete ke device sampai visitor tersebut tersinkron ulang/update.
- Jika endpoint get_ladt_id pada device memakai path berbeda, nilai GetEnrollIdEndpointPath perlu disesuaikan di appsettings.

Next Action:
- Validasi integrasi dengan device fisik untuk memastikan endpoint get_ladt_id mengembalikan enrollid sesuai format response.

ETA:
2026-06-18

---

# CONTROL TOWER REPORT

Agent:
Ramos VMS

Project:
Application VMS HM

Date:
2026-06-18

Current Task:
Menghapus kolom username pada Device dan menyesuaikan flow frontend sampai backend.

Status:
Done

Progress:
100%

Completed:
- Menghapus properti Username dari entity Device.
- Menghapus field username dari DeviceModel dan payload login device.
- Menyesuaikan DeviceService agar CRUD device tidak membaca/menyimpan username.
- Menyesuaikan login/update sync device menjadi berbasis password karena username device sudah dihapus.
- Menyesuaikan DeviceRecognitionService agar request get_ladt_id tidak lagi memakai device.Username dan mengirim cmd kosong.
- Menghapus kolom/input username pada halaman device dan render table device di home.html.
- Menjalankan dotnet build Application_VMS_HM.sln dengan hasil sukses, 0 warning, 0 error.

Issue / Blocker:
- Tidak ada blocker pada implementasi.

Need Decision:
- Konfirmasi deploy DB existing untuk drop kolom Username pada tabel crm.Device karena repo belum memiliki folder migrations existing.

Risk:
- Integrasi device yang sebelumnya mengirim username pada LoginDevice perlu menyesuaikan payload menjadi password-only.
- Jika endpoint get_ladt_id ternyata tetap memerlukan cmd berisi username, perlu ada keputusan pengganti karena username device sudah dihapus.

Next Action:
- Validasi flow manajemen device di UI dan integrasi device fisik setelah perubahan schema database.

ETA:
2026-06-18

---

# CONTROL TOWER REPORT

Agent:
Ramos VMS

Project:
Application VMS HM

Date:
2026-06-18

Current Task:
Audit management user dan memperbaiki error save edit serta delete/cancel.

Status:
Done

Progress:
100%

Completed:
- Menghapus kewajiban password pada UserModel agar edit user tidak gagal validasi ketika password tidak diubah.
- Menambahkan validasi password khusus create user di UserService.
- Mengubah data table user agar tidak mengirim PasswordHash ke frontend.
- Memperbaiki update user agar username, email, nama, dan optional password diproses melalui UserManager serta error Identity dikembalikan ke response.
- Menambahkan fungsi soft-delete user dengan Active=false, Deleted_By, dan Deleted_Date.
- Menyesuaikan UserController agar status D diproses sebagai delete/cancel tanpa terganjal validasi field create/edit.
- Menambahkan tombol Delete pada form user edit dan mapping data edit di home.html.
- Menjalankan dotnet build Application_VMS_HM.sln dengan hasil sukses, 0 warning, 0 error.

Issue / Blocker:
- Tidak ada blocker pada implementasi.

Need Decision:
- Konfirmasi perilaku reset password saat edit: password kosong berarti tidak mengubah password, password terisi berarti reset password user tersebut.

Risk:
- Delete user bersifat soft-delete; data Identity user tetap ada untuk histori dan filter list/dropdown sudah disesuaikan ke Active dan belum deleted.

Next Action:
- Uji manual flow add, edit tanpa password, edit dengan password baru, dan delete/cancel user dari UI.

ETA:
2026-06-18
