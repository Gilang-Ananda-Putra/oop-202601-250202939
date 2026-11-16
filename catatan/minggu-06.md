# Catatan Minggu 06: UML dan SOLID

## Yang saya kerjakan
- Membuat Class Diagram dari kode Minggu 1–5: atribut, method, tipe, visibility, relasi dengan multiplicity, dan pengelompokan per paket (`docs/uml/class-diagram.mmd`).
- Membuat Sequence Diagram pengajuan peminjaman dengan fragmen `alt` untuk skenario sukses dan skenario bentrok (`docs/uml/sequence-pengajuan.mmd`).
- Menulis `docs/uml/solid.md` yang menunjuk tiga prinsip SOLID pada kode nyata: OCP, SRP, dan ISP.
- Menjalankan `.\scripts\uji.ps1 -Minggu 6`: 35 tes lulus (minggu 1–6). 

## Keputusan desain
- **Prinsip SOLID yang ditunjukkan: OCP, SRP, dan ISP.** Ketiganya paling jelas terlihat di kode yang sudah ada.
  - OCP pada hierarki `Aset`: jenis aset baru cukup `extends Aset` dan meng-override `hitungDenda`, tanpa mengubah `KatalogAset`, `JadwalPeminjaman`, atau `DendaFlat`.
  - SRP: `Aset` mengurus data aset, `JadwalPeminjaman` mengurus daftar dan pemeriksaan jadwal, `KatalogAset` mengurus pencarian, dan aturan denda dipisah di paket `denda`.
  - ISP: `DapatDipinjam` dan `AturanDenda` masing-masing hanya punya satu method, jadi implementor tidak dipaksa menulis method yang tidak dipakai.
- **Aggregation, bukan composition, untuk `JadwalPeminjaman` ke `Peminjaman` dan `KatalogAset` ke `Aset`.** Sebuah `Peminjaman` atau `Aset` tetap bermakna walau daftar yang memegangnya dihapus.
- **Diagram dibuat dari kode yang ada, bukan dari rancangan ideal.** `LayananDenda` hanya digambar sebagai dependency ke `AturanDenda` karena field-nya baru dikerjakan di minggu 10. Alur `ajukan` pada Sequence Diagram disusun dari Javadoc-nya, karena implementasinya baru di minggu 9. Keduanya akan diperbarui di minggu 14.
- **Paket `dao`, `factory`, dan `minggu01` belum dimasukkan ke Class Diagram** supaya diagram tetap terbaca. Paket itu ditambahkan saat minggu 10–14.
- **Memakai format Mermaid (`.mmd`)** karena sumbernya teks, mudah disimpan di Git, dan bisa diekspor menjadi PNG atau SVG.
- **Refactor kecil:** `Aset.getJenis()` [dijadikan `abstract` / dibiarkan seperti semula (pilih salah satu sesuai yang kamu lakukan)], karena [alasan: kompilator memaksa tiap jenis aset mengisinya / belum diperlukan oleh tugas].

## Kendala
- Saat mengubah `getJenis()` menjadi abstrak, deklarasi `public abstract JenisAset getJenis();` sempat ditaruh di dalam badan method sehingga tidak valid. Method abstrak tidak boleh punya kurung kurawal. Diperbaiki dengan mengganti seluruh method lama dengan satu baris deklarasi berakhiran titik koma.
- Sempat ragu apakah penanda komentar `%%` di file Mermaid sama dengan `#`. Ternyata `%%` adalah komentar Mermaid, sedangkan `#` punya arti lain di sintaksnya.
- Diagram harus sesuai dengan kode yang ada, padahal sebagian method (`ajukan`, `bentrok`, `LayananDenda`) masih dikerjakan di minggu berikutnya. Penyelesaiannya: menggambar sesuai kode dan Javadoc yang ada, lalu mencatat bagian yang akan diperbarui.

## Penggunaan AI
- Alat yang dipakai: Claude (Anthropic).
- Bagian yang dibantu AI: draf sumber Class Diagram dan Sequence Diagram (Mermaid), penjelasan aggregation vs composition, OCP, dan DIP, serta koreksi penulisan method abstrak.
- Prompt utama: 
    1. Menempelkan potongan `getJenis()` yang salah dan menanyakan apakah penulisannya sudah benar, serta apakah `%%` di file `.mmd` sama dengan `#`.
    3. Menempelkan hasil `uji.ps1 -Minggu 6` dan meminta membuatkan catatan minggu 6 beserta penyesuaiannya.
- Yang saya ubah atau tolak dari saran AI, dan alasannya: Bagian diagram yang disesuaikan, prinsip SOLID yang diganti atau ditambah, atau saran yang tidak diikuti
- Satu hal yang saya pahami lebih baik sekarang: Perbedaan aggregation dan composition. `JadwalPeminjaman` memegang daftar `Peminjaman`, tetapi `Peminjaman` tetap ada walau jadwalnya dihapus, jadi relasinya aggregation. Pada composition, bagian ikut hilang bersama induknya.
