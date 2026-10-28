# Catatan Minggu NN: (topik)

## Yang saya kerjakan
Melengkapi tiga subclass dari Aset: Ruang (kapasitas), Laptop (spesifikasi dannomor inventaris), dan AlatLab (laboratorium dan tingkat risiko). Tiap subclass memanggil super(kode, nama), memvalidasi atribut khususnya, menyediakan getter, dan meng-override getJenis() agar mengembalikan RUANG, LAPTOP, atau ALAT_LAB. Validasi kode dan nama tetap berlaku karena dijalankan oleh konstruktor Aset.

## Keputusan desain
Atribut yang khusus satu jenis tetap di subclass dan tidak dipindah ke Aset. Kalau kapasitas ada di Aset, laptop dan alat lab ikut punya kapasitas yang tidak bermakna. Aset hanya memegang atribut yang dimiliki semua jenis: kode, nama, status, dan kondisi. Atribut subclass dibuat final tanpa setter karena tugas minggu ini tidak meminta perubahan setelah objek dibuat. Pengecekan teks memakai null dulu baru isBlank(), sama seperti di Aset, supaya tidak terjadi NullPointerException.

## Keterbatasan inheritance: contoh komposisi lebih tepat
Kalau aturan denda dibuat lewat subclass (misalnya LaptopDendaFlat dan
LaptopDendaProgresif), jumlah kelas berlipat untuk tiap kombinasi jenis dan aturan, dan
aturannya tidak bisa diganti saat program berjalan. Lebih tepat memakai komposisi: aset
atau layanan memakai objek AturanDenda yang bisa ditukar (DendaFlat atau
DendaProgresif). Peminjaman juga memakai komposisi: ia memiliki Aset dan Peminjam
sebagai field, karena Peminjaman bukan sebuah Aset.

## Kendala
Pada percobaan pertama, metode getJenis() ditulis dengan tipe kembalian String dan konstanta JenisAset.Ruang. Keduanya salah: Aset mendeklarasikan getJenis() dengan tipe kembalian JenisAset, jadi override harus memakai tipe yang sama, dan konstanta enum ditulis huruf besar semua (RUANG, LAPTOP, ALAT_LAB). Dan memperbaikinya dengan mencocokkan tanda tangan metode di Aset dan isi enum JenisAset.

## Penggunaan AI
- Alat yang dipakai: Claude, GitHub Copilot(Extensions)
- Bagian yang dibantu AI: Membantu menulis kode, menjelaskan urutan konstruktor dan is-a, contoh komposisi, Memerikasa kode, dan draf catatan.
- Prompt utama: 
    1. Claude
        - Berdasarkan kode yang saya kirimkan apakah ada kode yang salah?
        - coba berikan semua penjelasan tentang kode tersebut.
        - buatkan catatan berdasarkan draf tersebut.
    2. GitHub Copilot
        - tidak ada prompt, di karenakan membantu dalam menulis dan mengubah kode dengan sendirinya dan pengaplikasian menggunakan tab.
- Yang saya ubah atau tolak dari saran AI, dan alasannya: Kesalahan tipe getJenis() saya perbaiki setelah AI menunjukkan bahwa tipenya harus JenisAset. memparafrase catatan hasil AI.
- Satu hal yang saya pahami lebih baik sekarang: saat meng-override metode, tipe kembaliannya harus sama dengan metode di induk (di sini JenisAset), dan @Override membantu kompiler menangkap kesalahan itu. Saya juga memahami bahwa konstruktor induk berjalan lebih dulu daripada konstruktor anak.
