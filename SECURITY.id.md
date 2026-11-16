# Kebijakan Keamanan wiradesk-core
<!-- Copied from the Wira Delta Indonesia legal source (wira-desk-core/security.id.md) on 2026-11-16.
     Edit the source, then copy it here again. -->

Naskah ini adalah naskah resmi dalam bahasa Indonesia. Terjemahan bahasa Inggris tersedia di
[SECURITY.md](SECURITY.md).

**Berlaku sejak:** 2026-11-16 · berlaku untuk wiradesk-core 0.1.1 dan sesudahnya

wiradesk-core adalah pustaka Rust. Ia tidak membuka berkas, tidak membuat permintaan jaringan,
tidak menjalankan thread, tidak membaca jam, dan tidak memegang handle sistem operasi. Setiap
masukan datang sebagai argumen fungsi dari aplikasi yang menyematkannya, dan setiap hasil kembali
sebagai nilai balik.

Risikonya ada pada apa yang ia putuskan dari masukan itu. Aplikasi bisa memberinya kejadian tombol
dan mouse yang diambil dari hook input seluruh sistem, lalu memutuskan dari jawabannya
(`Disposition`) apakah kejadian itu ditelan. Wira Desk for Windows juga memberinya deskriptor
pembaruan yang diunduhnya, dan mengandalkan putusannya tentang alamat installer mana yang diterima.
Dua jalur itu layak diperiksa.

## Dua Fakta yang Paling Ingin Diketahui

- **Tidak ada ketikan yang direkam.** Satu-satunya keadaan tombol yang disimpan crate ini adalah
  `ModifierState`: empat boolean yang menyatakan apakah Win, Ctrl, Alt, dan Shift sedang ditekan.
  Tidak ada pemanggilan logging, berkas, atau jaringan di mana pun di `src/`, jadi nilai tombol
  tidak punya jalan keluar. `InputSink` adalah trait; setiap aplikasi mengimplementasikannya
  sendiri, jadi apa yang disimpan sebuah implementasi adalah kode dan kebijakan aplikasi itu.
- **Tidak ada yang dikirim.** Crate ini tidak membuat permintaan keluar. Pemeriksaan pembaruan di
  Wira Desk for Windows dilakukan oleh aplikasinya; crate ini hanya mengurai balasan yang
  diserahkan aplikasi ke `update::decide`.

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
- masukan yang membuat crate ini panic, overflow, atau tidak pernah kembali. Aplikasi memanggilnya
  saat menangani setiap kejadian tombol di desktop, jadi panic di sana bisa membuat shortcut
  berhenti bekerja.

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

Rilis berupa tag git, dan tidak ada biner. Aplikasi mengambil kode sumber per tag lewat Cargo, dan
`Cargo.lock` setiap aplikasi mencatat commit persis yang ditunjuk tag itu. Karena itu, build dari
lockfile yang sudah ada tetap memakai commit tersebut walaupun sebuah tag dipindahkan; hanya
`cargo update` yang akan mengambil perubahannya, dan perubahan itu terlihat sebagai diff lockfile.

Tag dan commit tidak ditandatangani. Sebuah tag hanya membuktikan bahwa seseorang dengan hak push ke
repositori ini membuatnya, bukan siapa orangnya. Karena seluruh crate berupa kode sumber,
verifikasi terkuat yang tersedia adalah membaca kodenya pada commit yang dicatat `Cargo.lock` Anda.
Menandatangani tag adalah perbaikan sesungguhnya, dan itu belum dilakukan.

## Bahasa

Bahasa. Naskah ini dibuat dalam bahasa Indonesia dan diterjemahkan ke bahasa Inggris. Bila terdapat
perbedaan tafsir antara keduanya, naskah bahasa Indonesia yang berlaku.
