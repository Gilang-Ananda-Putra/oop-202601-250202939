# Catatan Minggu 01: Paradigma Pemrograman dan Setup

## Yang saya kerjakan
Memalakukan instalasi JDK 21, Maven dan Memeriksa Git dari penginstalan yang dulu. Selanjutnya mengisi NIM dan nama di mahasiswa.properties, dan melengkapi 3 file HelloProsedural, HelloOOP, dan HelloFungsional. 

## Keputusan desain
Memakai format sapaan yang sama di ketiga file karena karena tes membandingkan hasilnya.
""Hello Aset-Pinjam, saya <nama> (<NIM>)"
Pada gaya fungsional, memakai lambda yang mengingat NIM dari parameter pembuatSapaan, sehingga tidak ada state yang berubah.

## Perbandingan tiga gaya (untuk sistem pinjaman aset)
1. **Prosedural**
- Kelebihan: Sederhana dan mudah di baca untuk proses seperti menghitung denda.
- Keterbatasan: Data aset harus dikirim lewat banyak parameter sehingga sulit di rawat ketika jenis aset bertambah.
2. **OOP**
- Kelebihan: Data dan perlaku aset terbungkus dalam satu class, sehingga, ruang, laptop, dan alat lab bisa punya aturan sendiri lewat pewarisan.
- Keterbatasan: Kodenya lebih panjang untuk hal sederhana.
3. **Fungsional**
- Kelebihan: Aturan seperti denda mudah di tukar dan diuji karena tidak asa state yang berubah. 
- Keterbatasan: Kurang natural untuk model status pinjaman yang berubah seiring waktu.

## Kendala
- Maven tidak tersedia lewat winget ("No package found matching input criteria"), dan situs resmi https://maven.apache.org/download.cgi tidak bisa di buka. Dan Berhasil menginstal nya dari https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/3.9.9/apache-maven-3.9.9-bin.zip, lalu mengestraknya ke C:tools dan menambahkan folder bin ke Path. 
- JAVA_HOME milik user sempat menunjuk ke JDK bawaan Android Studio, jadi di ubah ke Temurin 21.
- Kendala syntax di java tentang sapaan yang mana kurang sepasi di belakang kata saya

## Penggunaan AI
- Alat yang dipakai: Claude
- Bagian yang dibantu AI: Panduan instalasi JDK, Maven, dan Git. Memberikan penjelasan lebih dalam mengenai isi tugas. Memerikasa kode saya di kesalahan syntax.
- Prompt utama: 
1. "Bagaimana cara memasang JDK 21, Maven, pada windows."
2. "Coba jelaskan lebih lanjut mengenai tugas tersebut."
3. "Tolong bantuk pengecekkan pada kode apakah ada error."
- Yang saya ubah atau tolak dari saran AI, dan alasannya: Menolak saran dari ai untuk tetap memakai forder maven dari C:\tools\apache-maven-3.9.9/apache-maven-3.9.9 menjadi apache-maven-3.9.9, menjadi lebih singkat
- Satu hal yang saya pahami lebih baik sekarang: bahwa lambda di HelloFungsional bisa memakai nim dari luar closure
