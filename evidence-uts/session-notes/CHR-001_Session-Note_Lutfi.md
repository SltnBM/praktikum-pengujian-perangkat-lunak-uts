### SESSION NOTE (CATATAN EKSEKUSI SESI) - CHR-001

* **Tester:** Lutfi Khoirul Umam (240209043)
* **Peran:** R3 (Exploratory Tester)
* **Test Lead / Debriefer:** Sultan Badra Maulana (240209062 - R1)
* **Durasi Sesi:** 30 Menit (Full Session)
* **Waktu Pelaksanaan:** 21:54 - 22:24 WIB
* **Status Sesi:** [X] Selesai Penuh (Full)
* **Test Charter:** [CHR-001] Eksplorasi Validasi Form Registrasi Akun Pengguna Baru pada platform Practice Software Testing untuk menemukan masalah terkait validasi input, konsistensi data, dan penanganan nilai abnormal/ekstrem dalam waktu 30 menit.

#### DATA UJI & AREA PENJELAJAHAN
* **Target URL:** https://with-bugs.practicesoftwaretesting.com/#/auth/register
* **Akun Uji:** Guest (tanpa login) – data dummy (First Name = Mamat, Last Name = Ganteng, dst.)
* **Rute Explorasi:** Buka halaman registrasi -> isi semua field dengan data valid -> pada tiap iterasi ganti satu field dengan nilai invalid (DOB future, Postcode symbol + emoji, Postcode negative, Country corrupt, Email malformed, Password short/weak, Phone non-numeric, First Name > 300 karakter) -> klik Register -> amati pesan error, status akun, serta keutuhan UI.

#### ANOMALI / BUG YANG DITEMUKAN
1. **[BUG-REG-01] [HIGH] Country dropdown encoding error**
   * *Deskripsi:* Pilihan "Åland Islands" tampil sebagai "land Islands".
   * *Severity / Priority:* Major / P2
   * *Tautan:* Temuan observasi internal sesi (non-eskalasi)
2. **[BUG-REG-02] [MEDIUM] Postcode accepts special characters + emoji**
   * *Deskripsi:* Kolom Postcode menerima `!@#$ 🛠️🔧` tanpa penolakan; form berhasil disubmit.
   * *Severity / Priority:* Minor / P3
   * *Tautan:* https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/15
3. **[BUG-REG-03] [MEDIUM] Postcode accepts negative value**
   * *Deskripsi:* Nilai `-2020` pada Postcode diterima, tidak ada pesan error.
   * *Severity / Priority:* Minor / P3
   * *Tautan:* Temuan observasi internal sesi (non-eskalasi)
4. **[BUG-REG-04] [MEDIUM] Password field does not enforce minimum length (10 karakter)**
   * *Deskripsi:* Password `123456789` (9 karakter) diterima; tidak ada pesan "Password must be at least 10 characters".
   * *Severity / Priority:* Minor / P2
   * *Tautan:* Temuan observasi internal sesi (non-eskalasi)
5. **[BUG-REG-05] [MEDIUM] Phone field accepts non-numeric characters**
   * *Deskripsi:* Input `+AB!#` pada Phone tidak ditolak.
   * *Severity / Priority:* Minor / P3
   * *Tautan:* Temuan observasi internal sesi (non-eskalasi)
6. **[BUG-REG-06] [MEDIUM] First Name > 300 karakter causes layout overflow**
   * *Deskripsi:* Mengisi First Name dengan 350 karakter "A" membuat UI meluber; tidak ada pemotongan atau pesan error.
   * *Severity / Priority:* Minor / P3
   * *Tautan:* Temuan observasi internal sesi (non-eskalasi)
7. **[BUG-REG-07] [MEDIUM] Password field does not enforce minimum strength**
   * *Deskripsi:* Password `12345` (hanya huruf kecil, kurang dari 8 karakter, tanpa simbol) dapat disubmit tanpa menampilkan pesan "Password must be at least 8 characters and contain upper-case, lower-case, a number and a special character".
   * *Severity / Priority:* Minor / P2
   * *Tautan:* https://github.com/SltnBM/praktikum-pengujian-perangkat-lunak-uts/issues/16

#### PERTANYAAN & HAMBATAN
* **Pertanyaan:** Tidak ada pertanyaan terbuka.
* **Hambatan:** Tidak ada kendala eksternal; semua langkah dapat dijalankan pada lingkungan Staging.

#### ESTIMASI RASIO DISTRIBUSI WAKTU (TBS RATIO - TOTAL 100%)
* **Setup (Persiapan lingkungan uji & data dummy):** 15% (4.5 Menit)
* **Eksplorasi Aktif (Proses penjelajahan dan variasi input):** 60% (18.0 Menit)
* **Investigasi Bug (Reproduksi anomali, penangkapan evidence, pencatatan):** 25% (7.5 Menit)
*(Akumulasi Total: 15% + 60% + 25% = 100%)*

#### CATATAN DEBRIEF BERSAMA TEST LEAD (R1)
* Debrief dilaksanakan dipimpin oleh Sultan Badra (R1).
* Keputusan Lead: Dari temuan anomali yang diobservasi, 2 bug paling signifikan disepakati untuk dieskalasi ke repositori GitHub Issues (BUG-REG-02 / Issue #15 dan BUG-REG-07 / Issue #16).
