### SESSION NOTE (CATATAN EKSEKUSI SESI) - CHR-002

* **Tester:** Sultan Badra Maulana (240209062)
* **Peran:** R1 (Test Lead / Exploratory Tester)
* **Durasi Sesi:** 30 Menit (Full Session)
* **Waktu Pelaksanaan:** 23:21 - 23:51 WIB
* **Status Sesi:** [X] Selesai Penuh (Full)
* **Test Charter:** [CHR-002] Eksplorasi Autentikasi, Manajemen Sesi, dan Navigasi Akun Customer dan Admin pada platform Practice Software Testing untuk menemukan masalah terkait endpoint API, data-binding, dan konsistensi status sesi dalam waktu 30 menit.

#### DATA UJI & AREA PENJELAJAHAN
* **Target URL:** https://with-bugs.practicesoftwaretesting.com
* **Akun Uji 1:** customer@practicesoftwaretesting.com / welcome01 (Baseline / Akun Customer Default)
* **Akun Uji 2:** test-uts@gmail.com / Password123! (Akun Uji Utama - Customer)
* **Akun Uji 3:** admin@practicesoftwaretesting.com / welcome01 (Akun Administrator / Verifikasi Back-office)
* **Rute Navigasi:** Login Customer -> Account Dashboard -> Profile (/#/account/profile) -> Form Edit Profile -> Save -> Verifikasi API & Persistence -> Login Admin -> Users Management (/#/admin/users) -> Detail User ID 4 -> Form Tambah Brand (/#/admin/brands/add) -> Menu Laporan Statistik (/#/admin/reports/statistics) -> Logout.

#### ANOMALI / BUG YANG DITEMUKAN
1. **[BUG-01] [CRITICAL] Update Profile Gagal Total Akibat Kesalahan Endpoint (HTTP 404)**
    * **Deskripsi:** Tombol Update Profile mengirim request ke endpoint keliru PUT /user/ID alih-alih /users/ID, sehingga server merespons HTTP 404 dengan banner merah "Resource not found". Fungsi inti pembaruan profil lumpuh total tanpa jalan keluar (no workaround).
    * **Tautan:** https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/13
2. **[BUG-02] [HIGH] Inversi Posisi Nama Depan & Belakang pada Form Profil Pelanggan**
    * **Deskripsi:** Posisi First Name dan Last Name tertukar secara sistemik saat data dimuat di form profil pelanggan (/#/account/profile). Nama belakang terisi ke kolom nama depan, dan sebaliknya.
    * **Tautan:** https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/14
3. **[BUG-03] [HIGH] Inversi Data Negara & Provinsi (Country & State) pada Profil**
    * **Deskripsi:** Field Country tertukar dengan State pada form profil customer, di mana kolom State terisi dengan kode ISO negara (seperti "AL") alih-alih nama provinsi sebenarnya.
    * **Tautan:** https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/14
4. **[BUG-04] [HIGH] Kerusakan Field Kota (City) Menampilkan String Mentah "City not found"**
    * **Deskripsi:** Field kota pada antarmuka profil customer menampilkan teks mentah hardcoded "City not found" alih-alih data nama kota sebenarnya yang valid tersimpan di basis data.
    * **Tautan:** https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/14
5. **[BUG-05] [MEDIUM] Tombol Back Macet (Dead Button) di Formulir Tambah Merek Admin**
    * **Deskripsi:** Tombol Back di halaman /#/admin/brands/add tidak memiliki atribut href aktif. Ketika diklik, peramban diam saja sehingga administrator terjebak di dalam formulir dan terpaksa menggunakan tombol navigasi peramban.
    * **Tautan:** Temuan observasi internal sesi (non-eskalasi)
6. **[BUG-06] [MEDIUM] Navbar Menu Akun Pengguna Menampilkan Teks Mentah "User Data not found"**
    * **Deskripsi:** Setelah login berhasil, dropdown menu pengguna pada bilah navigasi kanan atas (#user-menu) menampilkan teks mentah "User Data not found" alih-alih nama profil akun pengguna yang aktif.
    * **Tautan:** Temuan observasi internal sesi (non-eskalasi)
7. **[BUG-07] [MEDIUM] Fitur Pemilihan Bahasa (Language Selector) Hilang Total**
    * **Deskripsi:** Opsi pemilihan bahasa antarmuka (EN / DE) yang tersedia pada aplikasi versi acuan (Oracle) lenyap total pada antarmuka aplikasi AUT yang sedang diuji.
    * **Tautan:** Temuan observasi internal sesi (non-eskalasi)
8. **[BUG-08] [MEDIUM] Fitur Two-Factor Authentication (2FA) Hilang pada Halaman Akun**
    * **Deskripsi:** Opsi pengaturan keamanan autentikasi dua faktor (2FA) tidak tersedia dan hilang dari menu dashboard akun pengguna.
    * **Tautan:** Temuan observasi internal sesi (non-eskalasi)
9. **[BUG-09] [MEDIUM] Header Kolom Laporan Statistik Admin Salah Label secara Sistemik**
    * **Deskripsi:** Seluruh tabel laporan di /#/admin/reports/statistics menggunakan label header seragam "Category Name" dan "Total Earned", bahkan pada tabel yang menampilkan data agregasi pelanggan berdasarkan negara dan produk.
    * **Tautan:** Temuan observasi internal sesi (non-eskalasi)

#### PERTANYAAN & HAMBATAN
* **Pertanyaan:**
    * Apakah kesalahan penamaan endpoint PUT /user/{id} (singular vs plural) merupakan kelalaian konfigurasi routing frontend API client, atau ada perubahan spesifikasi API v1 ke v2 yang belum tersinkronisasi?
    * Mengapa data-binding pada form profil customer (/#/account/profile) menukar first_name dan last_name serta merusak pemetaan alamat, sementara form pada dashboard admin memetakan data yang sama dari basis data dengan benar?
    * Mengapa tombol Back pada halaman /#/admin/brands/add kehilangan atribut href aktif, sedangkan pada halaman sejenis seperti /categories/add dan /users/add atribut href terpasang dan berfungsi normal?
    * Mengapa opsi pemilihan bahasa dan fitur keamanan 2FA dihilangkan dari antarmuka akun pada build ini?
    * Mengapa seluruh tabel pada laporan statistik modul admin menggunakan template header kolom yang sama tanpa pemetaan nama metrik yang sesuai?
* **Hambatan:**
    * Tidak ada kendala eksternal yang berarti selama pengujian.

#### ESTIMASI RASIO DISTRIBUSI WAKTU (TBS RATIO - TOTAL 100%)
* **Setup (Persiapan lingkungan uji & verifikasi akun):** 10% (3.0 Menit)
* **Eksplorasi Aktif (Penjelajahan alur navigasi dan sesi):** 65% (19.5 Menit)
* **Investigasi Bug (Inspeksi DevTools Network/Storage, verifikasi API):** 25% (7.5 Menit)
*(Akumulasi Total: 10% + 65% + 25% = 100%)*

#### CATATAN DEBRIEF BERSAMA TEST LEAD (R1)
* Debrief mandiri / tim dilaksanakan dipimpin oleh Sultan Badra (R1).
* Keputusan Lead: Disepakati 2 temuan paling berdampak tinggi untuk dieskalasi (BUG-01 / Issue #13 berkategori Critical karena kegagalan pembaruan profil total, dan BUG-02 / Issue #14 berkategori Major terkait inversi nama depan/belakang dan kerusakan alamat).
