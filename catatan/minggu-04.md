# Catatan Minggu 04: Polimorfisme  

## Yang saya kerjakan
- `Aset.hitungDenda(int)` memanggil helper `protected final hitungDendaDasar(hariTerlambat, pengaliJenis)`. Pemeriksaan hari negatif dan perhitungan ada di satu tempat, jadi tidak diulang di tiap subclass.
- `Ruang`, `Laptop`, dan `AlatLab` meng-override `hitungDenda` dengan pengali masing-masing (1x, 2x, 4x).
- `KatalogAset.cari(kata)` mencocokkan nama tanpa membedakan huruf besar/kecil. `cari(kata, jenis)` memakai hasil `cari(kata)` lalu menyaring berdasarkan `getJenis()`.

## Rumus denda
denda = hari terlambat × pengali jenis × tarif dasar aktif (`Varian.aktif().dendaPerHari`).
Tarif tidak ditulis langsung di kode; diambil dari konfigurasi Varian (NIM berakhiran 9 → Rp6.000 per hari).
Hasil dihitung sebagai `long` (`(long) hariTerlambat * ...`) agar tidak overflow.

Contoh, terlambat 3 hari pada tarif Rp6.000:
| Jenis | Perhitungan | Denda |
|---|---|---|
| Ruang | 3 × 1 × 6.000 | Rp18.000 |
| Laptop | 3 × 2 × 6.000 | Rp36.000 |
| AlatLab | 3 × 4 × 6.000 | Rp72.000 |

Nol hari menghasilkan 0. Hari negatif melempar `IllegalArgumentException`.

## Dynamic binding
Variabel bertipe `Aset` dapat menunjuk objek Ruang, Laptop, atau AlatLab. Saat `hitungDenda` dipanggil,
Java memilih versi milik objek yang sebenarnya pada saat program berjalan, bukan versi dari tipe variabel.
Karena itu satu loop `for (Aset a : daftar) a.hitungDenda(3)` menghasilkan denda berbeda untuk tiap jenis tanpa `if`/`instanceof`.

## Overriding vs overloading
- **Overriding**: subclass mengganti isi method dengan tanda tangan sama (`hitungDenda(int)`); dipilih saat runtime.
- **Overloading**: nama sama, parameter berbeda (`cari(String)` dan `cari(String, JenisAset)`); dipilih saat kompilasi.

## Keputusan desain
- **Perhitungan denda ditaruh di satu helper `hitungDendaDasar` (protected final) di `Aset`.**
  Cek hari negatif dan rumus kali tarif ada di satu tempat, jadi tiga subclass tidak menyalin kode yang sama
  dan tidak bisa menghitung dengan cara sendiri. Subclass hanya menentukan pengalinya lewat `hitungDenda`.
- **Pengali jenis disimpan sebagai konstanta `PENGALI_DENDA` di tiap subclass**, bukan angka langsung di dalam rumus,
  supaya aturan 1x / 2x / 4x mudah dicari dan diubah.
- **Tarif dasar diambil dari `Varian.aktif().dendaPerHari`**, tidak ditulis langsung (hardcode).
  Tarif berbeda tiap mahasiswa; untuk NIM saya (digit terakhir 9, 9 mod 5 = 4) hasilnya Rp6.000 per hari.
- **Hasil bertipe `long` dan hari dikonversi dengan `(long)` sebelum dikali**, agar tidak overflow pada hari yang sangat besar.
- **`hitungDenda` di `Aset` dibuat bukan abstrak, dengan pengali 1** lewat `hitungDendaDasar(hariTerlambat, 1)`,
  karena kelas `Aset` baru menjadi abstrak di minggu 5. Subclass tetap meng-override-nya.
- **`cari(kata, jenis)` memanggil `cari(kata)` lalu menyaring jenis**, sehingga aturan pencocokan nama
  (tanpa membedakan huruf besar/kecil, `Locale.ROOT`) hanya ditulis sekali.

## Kendala
- Saat konstruktor `Ruang` dirapikan, baris field `kapasitas` tertimpa konstanta pengali sehingga muncul
  error `cannot find symbol kapasitas`. Diperbaiki dengan menambahkan kembali `private final int kapasitas;`.
- Pengali `AlatLab` sempat saya tulis 3, padahal aturannya 4x; saya cocokkan lagi dengan `docs/aturan-bisnis.md`.
- Import `DapatDipinjam` di `Aset.java` sempat hilang dan ada pemanggilan `hitungDenda` dengan dua argumen
  yang tidak ada; keduanya diperbaiki sampai `.\scripts\uji.ps1 -Minggu 4` hijau (27 tes).


## Penggunaan AI
- Alat yang dipakai: Claude (Anthropic).
- Bagian yang dibantu AI: arahan perubahan kode untuk `hitungDenda` di `Aset`, `Ruang`, `Laptop`, `AlatLab` dan `cari(...)` di `KatalogAset`; penjelasan dynamic binding serta overriding vs overloading; membaca pesan error kompilasi; dan menyiapkan kerangka catatan ini. Kode saya ketik dan terapkan sendiri di VS Code, lalu saya jalankan `.\scripts\uji.ps1 -Minggu 4`.
- Prompt utama: 
    1. Memberikan hasil `uji.ps1 -Minggu 4` yang berisi error kompilasi `cannot find symbol kapasitas` dan meminta penjelasan penyebabnya.
    2. Buatkan Catatan berdasarkan templet ini dengan prompt yang utama.
- Yang saya ubah atau tolak dari saran AI, dan alasannya:
- Satu hal yang saya pahami lebih baik sekarang: Dynamic binding. Variabel bertipe `Aset` bisa menunjuk objek Ruang, Laptop, atau AlatLab. Saat `aset.hitungDenda(3)` dipanggil, Java memakai versi milik objek yang sebenarnya pada saat program berjalan, bukan versi dari tipe variabelnya. Dengan tarif Rp6.000, objek Ruang menghasilkan Rp18.000, Laptop Rp36.000, dan AlatLab Rp72.000 dari satu pemanggilan yang sama. Ini berbeda dengan overloading `cari(kata)` dan `cari(kata, jenis)`, yang dipilih saat kompilasi berdasarkan jumlah dan tipe parameter.
