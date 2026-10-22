# Catatan Minggu 02: Struktur OOP: Class, Object, Encapsulation

## Yang saya kerjakan
Melengkapi kelas Aset di model/Aset.java. Empat private (kode, nama, status, kondisi), konstruksi, getter, tiga setter (nama, status, kondisi), dan validasi. Status awal TERSEDIA dan kondisi awal BAIK. Kode dan mana tidak boleh null, kosong atau hanya spasi (IllegalArgumentException), sedangkan setter status dan kondisi menolak null. kode tidak punya setter.

## Keputusan desain
1. Field kode dibuat final tanpa setter karena kode adalah identitas aset, dan juga dipakai kunci Map di InMemoryAsetRepository, jadi kalau bisa berubah, aset tidak ditemukan lagi.
2. Validasi dilakukan sebelum field diisi, sehingga jika gagal, nilai lama tetap.
3. Pemeriksaan "kosong" memakai isBlank() karena tes menguji kode berisi spasi saja. Pemerikasaan null ditulis lebih dulu agar tidak terjadi NullPointerException.

## Keputusan Desain: Normalisasi Kode

Saya memilih tidak melakukan normalisasi kode menggunakan `trim()` atau mengubah huruf menjadi kapital. Validasi hanya memastikan kode tidak `null`, kosong, atau berisi spasi saja, tanpa mengubah nilai yang dimasukkan.

Alasannya, repository menggunakan kode aset sebagai kunci pencarian. Perbedaan spasi atau huruf besar dan kecil dapat memengaruhi hasil pencarian. Jika normalisasi diperlukan di kemudian hari, penerapannya harus konsisten di seluruh proses penyimpanan dan pencarian.

## Encapsulation dan Field Private

Encapsulation adalah konsep OOP yang membatasi akses langsung ke data objek. Field dibuat `private` agar perubahan data dilakukan melalui metode yang dapat menerapkan validasi.

Contohnya, setter status menolak nilai `null`. Dengan demikian, aturan validasi tidak mudah dilewati melalui perubahan langsung dari kelas lain. Getter digunakan untuk membaca nilai, sedangkan setter digunakan untuk mengubah nilai jika perubahan diizinkan.

## Kendala
- Pada percobaan pertama, proses kompilasi gagal karena beberapa kesalahan penulisan, yaitu Kondisi.Baik yang seharusnya BAIK, Kode yang seharusnya kode, IllegaArgumentException yang kurang satu huruf l, serta penggunaan field name padahal parameternya bernama nama. Saya memperbaikinya dengan membaca nomor baris pada pesan error, lalu mencocokkannya dengan kode. Setelah mengganti field menjadi nama, bagian getNama() juga perlu diperbaiki dari return name menjadi return nama.

- Saya sempat menjalankan .\scripts\uji.ps1 tanpa menambahkan parameter -Minggu 2. Akibatnya, hanya tes Minggu 1 yang dijalankan karena minggu-aktif.txt masih berisi angka 1. Tes Minggu 2 baru berhasil diperiksa setelah saya menggunakan parameter -Minggu 2.

## Penggunaan AI
- Alat yang dipakai: Claude
- Bagian yang dibantu AI: Memberikan arahan dalam pembuatan bagian-bagian kelas Aset.java, mulai dari field, konstruktor, getter, hingga setter. AI juga membantu menjelaskan kode dan encapsulation, membahas pertimbangan normalisasi kode, memahami pesan error kompilasi, serta menyusun draf catatan.
- Prompt utama: 
1. Bantu saya menjelaskan apa yang diperlukan untuk minggu 2, dan berikan arahan.
2. Oke sekarang bantu saya menyusun catatn, dan pada bagian prompt masukan prompt utamannya.
- Yang saya ubah atau tolak dari saran AI, dan alasannya: Memparafrase semua isi catatan dengan ini atau inti yang sama
- Satu hal yang saya pahami lebih baik sekarang: Field private serta validasi pada konstruktor dan setter membantu menjaga agar objek tetap memenuhi aturan yang ditentukan. Saya juga memahami bahwa validasi bertujuan menolak nilai yang tidak sesuai, sedangkan normalisasi mengubah nilai agar mengikuti format tertentu.
