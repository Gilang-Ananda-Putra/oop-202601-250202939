# SOLID pada Aset-Pinjam

## Open/Closed Principle (OCP): hierarki Aset

`Aset` adalah class abstrak dengan `hitungDenda(int)` abstrak. Menambah jenis aset baru (misalnya `Proyektor`) cukup membuat subclass `Aset` yang meng-override `hitungDenda`. `KatalogAset`, `JadwalPeminjaman`, dan `DendaFlat` tidak perlu diubah karena hanya bergantung pada tipe `Aset`. Tanpa OCP, setiap jenis baru memaksa `if`/`instanceof` baru di banyak tempat.

## Single Responsibility Principle (SRP): satu alasan untuk berubah

`Aset` hanya menyimpan data aset dan menjaga aturannya. `JadwalPeminjaman` hanya mengurus daftar peminjaman dan pemeriksaan jadwal. `KatalogAset` hanya mengurus daftar dan pencarian aset. Aturan denda dipisah ke paket `denda` (`DendaFlat`, `DendaProgresif`), sehingga perubahan rumus denda tidak menyentuh `JadwalPeminjaman`.

## Interface Segregation Principle (ISP): kontrak kecil

`DapatDipinjam` hanya punya `bisaDipinjam()` dan `AturanDenda` hanya punya `hitung(Aset, int)`. Implementor tidak dipaksa menulis method yang tidak dipakai, dan pemakai hanya bergantung pada kemampuan yang dibutuhkan.

## Catatan jujur

`Aset.getJenis()` saat ini masih melempar `UnsupportedOperationException` dan hanya benar bila subclass meng-override-nya. Menjadikannya `abstract` akan memperkuat OCP dan Liskov, karena kompilator memaksa tiap jenis menjawab.