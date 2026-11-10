# Catatan Minggu Minggu 05: Abstraksi

## Yang saya kerjakan
- `Aset` dijadikan `abstract` dan `hitungDenda(int)` dijadikan metode abstrak, jadi `new Aset(...)` tidak bisa lagi dan tiap subclass wajib menentukan dendanya.
- `bisaDipinjam()` diisi: true hanya jika status TERSEDIA dan kondisi bukan RUSAK_BERAT.
- `DendaFlat` mengembalikan `aset.hitungDenda(hari)`. `DendaProgresif` memakai rumus yang saya putuskan sendiri.

## Keputusan desain
- Rumus progresif: 7 hari pertama sama dengan flat, sesudahnya ditambah 50% tarif harian per hari (lihat A-03 di docs/asumsi.md).
  Contoh Ruang tarif Rp6.000: hari 10 = Rp69.000 (flat Rp60.000), hari 30 = Rp249.000 (flat Rp180.000).
- Tarif harian diambil dari `aset.hitungDenda(1)` agar multiplier jenis dan tarif Varian tetap dipakai.

## Abstract class vs interface
- `Aset` adalah abstract class karena menyimpan keadaan (kode, nama, status, kondisi) dan kode bersama, dan memaksa subclass mengisi `hitungDenda`. Satu kelas hanya bisa mewarisi satu abstract class.
- `DapatDipinjam` dan `AturanDenda` adalah interface karena hanya kontrak kemampuan tanpa keadaan; satu kelas boleh mengimplementasikan banyak interface.

## Pola Strategy
`AturanDenda` adalah antarmuka strategi. `DendaFlat` dan `DendaProgresif` adalah dua cara menghitung denda yang bisa ditukar tanpa mengubah kode pemakainya.

## Kendala
Menentukan rumus denda progresif yang memenuhi aturan pengujian, yaitu denda tidak boleh lebih murah daripada flat, tidak boleh turun ketika jumlah hari bertambah, dan harus lebih mahal daripada flat pada hari ke-30.

## Penggunaan AI
- Alat yang dipakai: Claude, ChatGPT
- Bagian yang dibantu AI: usulan rumus progresif, arahan perubahan Aset, DendaFlat, DendaProgresif, penjelasan abstract class vs interface dan Strategy, draf catatan dan asumsi
- Prompt utama: 
    - Mengusulkan rumus denda progresif yang memenuhi invariant tes, tunjukkan contoh perhitungan, dan jelaskan kelebihannya sebelum implementasi.
    - cek dan uji file yang saya kirim apakah ada typenya.
    - buatkan catatan dan asumsi seusai catatan yang sudah ku kirim sebelumnnya
- Yang saya ubah atau tolak dari saran AI, dan alasannya: Menolak usulan awal kenaikan denda sebesar 10% setiap tujuh hari dan memilih rumus tujuh hari pertama flat, kemudian tambahan 50% tarif harian untuk setiap hari berikutnya. Rumus ini dipilih karena lebih sederhana dan kenaikan dendanya lebih mudah dipahami serta mengikuti multiplier masing-masing jenis aset.

- Satu hal yang saya pahami lebih baik sekarang: Perbedaan abstract class dan interface. `Aset` dibuat abstract karena menyimpan keadaan (kode, nama, status, kondisi) dan kode bersama, serta memaksa tiap subclass mengisi `hitungDenda`, sehingga `new Aset(...)` tidak bisa dipakai lagi. `DapatDipinjam` dan `AturanDenda` berupa interface karena hanya kontrak kemampuan tanpa keadaan. Dengan `AturanDenda`, cara menghitung denda bisa ditukar antara `DendaFlat` dan `DendaProgresif` tanpa mengubah kode pemakainya (pola Strategy). Dari kodemu: `flat.hitung(aset, 5)` dan `progresif.hitung(aset, 5)` dipanggil dengan cara yang sama, tetapi hasilnya ditentukan oleh objek strateginya.
