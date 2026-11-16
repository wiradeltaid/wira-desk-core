# Kebijakan Keamanan wiradesk-core
<!-- Copied from the Wira Delta Indonesia legal source (wira-desk-core/security.id.md) on 2026-11-16.
     Edit the source, then copy it here again. -->

Naskah ini adalah naskah resmi dalam bahasa Indonesia. Terjemahan bahasa Inggris tersedia di
[SECURITY.md](https://github.com/wiradeltaid/wira-desk-core/blob/main/SECURITY.md).

**Berlaku sejak:** 2026-11-16 · berlaku untuk wiradesk-core 0.1.1 dan sesudahnya

wiradesk-core adalah pustaka Rust sans-IO. Kode pustaka ini tidak membuka berkas, membuat permintaan
jaringan, menjalankan thread, membaca jam, atau memanggil API sistem operasi. Aplikasi memberikan data
melalui argumen; fungsi dapat mengembalikan nilai atau mengubah data yang diberikan sebagai argumen
mutable.

Risiko bergantung pada cara aplikasi menggunakan keputusan dan tipe dari pustaka ini. Aplikasi dapat
meneruskan kejadian tombol dan mouse dari hook input seluruh sistem ke implementasi `InputSink`, lalu
menggunakan `Disposition` untuk menentukan apakah kejadian diteruskan. Wira Desk for Windows juga
menyerahkan deskriptor pembaruan yang diunduh kepada `update::decide` untuk menentukan apakah rilis
ditawarkan.

## Dua Fakta yang Paling Ingin Diketahui

- **Crate ini tidak merekam ketikan.** `ModifierState` menyimpan empat boolean untuk keadaan Win,
  Ctrl, Alt, dan Shift. `InputEvent::Key` membawa nilai tombol ke `InputSink`, tetapi crate ini tidak
  memiliki pemanggilan logging, berkas, atau jaringan di `src/`. Aplikasi mengimplementasikan
  `InputSink` sendiri dan bertanggung jawab atas cara implementasinya menangani atau menyimpan kejadian.
- **Crate ini tidak mengirim permintaan jaringan.** Wira Desk for Windows melakukan pemeriksaan
  pembaruan di aplikasi; crate ini mengurai dan memeriksa deskriptor yang diserahkan aplikasi melalui
  `update::decide`.

`#![forbid(unsafe_code)]` dipasang di akar crate, jadi crate ini tidak memuat kode `unsafe` dan
compiler menolak kode `unsafe` apa pun.

## Melaporkan Kerentanan

Gunakan **GitHub Security Advisories** di repositori ini ("Report a vulnerability" di tab
Security), supaya laporan tetap tertutup sampai ada perbaikan. Bila Anda tidak bisa memakai GitHub,
kirim surel ke [security@wiradelta.com](mailto:security@wiradelta.com). Jangan membuka issue publik
untuk dugaan kerentanan.

Yang membantu dalam laporan: tag atau commit, fungsi yang Anda panggil, masukannya, apa yang
terjadi, dan, bila ada, tes yang gagal. Tidak ada program bounty dan tidak ada jaminan waktu
tanggap. Ini proyek kecil, dan jujur tentang hal itu lebih berguna daripada janji yang tidak bisa
ditepati.

**Dalam lingkup:**

- deskriptor yang membuat `update::decide` menawarkan rilis yang seharusnya ditolak: alamat unduhan
  di luar unduhan rilis repositori aplikasi Windows (`github.com/wiradeltaid/wira-desk`), alamat
  yang bukan HTTPS, checksum yang bukan 64 karakter heksadesimal, atau versi yang tidak lebih baru
  daripada versi yang sedang berjalan;
- masukan yang membuat crate ini panic, overflow, atau tidak pernah kembali. Kegagalan saat aplikasi
  menangani input dapat mengganggu shortcut.

**Di luar lingkup, didokumentasikan alih-alih diperbaiki:**

- cara aplikasi mengumpulkan masukan atau bertindak atas hasilnya: hook, handle window, izin,
  unduhan, dan installer adalah kode aplikasi. Laporkan lewat kebijakan keamanan aplikasi itu
  sendiri;
- apakah installer yang diunduh cocok dengan `setup_sha256`. `update::decide` memeriksa bentuk
  checksum, bukan berkasnya, karena crate ini tidak pernah melihat berkas itu. Membandingkan
  keduanya adalah tugas aplikasi, dan perbandingan yang tidak dilakukan adalah bug aplikasi;
- kerentanan di `serde` atau `serde_json` sendiri. Keduanya proyek terpisah yang menerbitkan
  perbaikannya sendiri, jadi laporkan kepada pemeliharanya.

**Versi yang didukung.** Hanya tag terbaru yang menerima perbaikan. Tidak ada cabang dukungan
jangka panjang.

## Integritas Rilis

Rilis berupa tag Git, tanpa biner. Ketika aplikasi mengambil dependensi Git melalui Cargo, `Cargo.lock`
mencatat commit hasil resolusi. Selama entri lockfile tetap dipakai, build menggunakan commit itu
meskipun tag dipindahkan. Pembaruan dependensi, perubahan manifest, atau pembuatan ulang lockfile dapat
menghasilkan resolusi baru; periksa diff `Cargo.lock` untuk melihat perubahan commit.

Tag `v0.1.0` dan `v0.1.1` tidak ditandatangani, sehingga tag tersebut tidak menyediakan bukti
kriptografis tentang penandatangannya. Setiap tag sesudah `v0.1.1` ditandatangani dengan kunci SSH yang
terdaftar di akun GitHub pemelihara. GitHub dapat menampilkan status Verified; untuk memeriksanya secara
lokal dengan `git tag -v`, konfigurasikan `gpg.ssh.allowedSignersFile` dengan kunci publik penanda
tangan yang telah Anda percayai. Commit tidak ditandatangani. Anda juga dapat meninjau kode pada commit
yang dicatat dalam `Cargo.lock`.

## Bahasa

Bahasa. Naskah ini dibuat dalam bahasa Indonesia dan diterjemahkan ke bahasa Inggris. Bila terdapat
perbedaan tafsir antara keduanya, naskah bahasa Indonesia yang berlaku.
