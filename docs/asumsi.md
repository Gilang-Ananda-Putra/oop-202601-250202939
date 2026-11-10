# Asumsi dan Keputusan Desain

Isi bagian ini sendiri. Satu entri per keputusan. Dinilai dari kejelasan alasan dan konsistensi dengan kode dan tes Anda,
bukan dari jawaban "benar".

Format tiap entri:

```
## A-NN: judul singkat
- Pertanyaan: ...
- Keputusan: ...
- Alasan: ...
- Dampak ke kode: ... (kelas/metode)
- Tes yang membuktikan: ... (nama tes di folder mahasiswa/)
```

## Aturan ambigu dari docs/aturan-bisnis.md

Salin setiap pertanyaan ambigu (nomor 1 sampai 7) ke sini dan jawab. Contoh:

## A-01: batas waktu bentrok
- Pertanyaan: 09:00-11:00 dan 11:00-13:00 pada aset yang sama bentrok atau tidak?
- Keputusan: (isi)
- Alasan: (isi)
- Dampak ke kode: (isi)
- Tes yang membuktikan: (isi)

## Asumsi lain
## A-02: normalisasi kode dan nama aset
- Pertanyaan: Apakah kode (dan nama) aset di-trim atau diubah menjadi huruf besar saat disimpan?
- Keputusan: Tidak. Nilai disimpan persis seperti yang diberikan. Hanya null, kosong, atau
  hanya spasi yang ditolak (IllegalArgumentException).
- Alasan: Repository dan DAO mencari dengan kode mentah, jadi mengubah kode di konstruktor
  membuat pencarian "a-01" gagal menemukan "A-01" tanpa terlihat. Menormalisasi juga
  mengubah data pemanggil secara diam-diam dan rawan salah lokal (toUpperCase tanpa
  Locale.ROOT). Validasi dipisahkan dari normalisasi.
- Dampak ke kode: Aset (konstruktor, setNama), metode bantu private wajibIsi dan wajibAda.
- Tes yang membuktikan: (isi nama tes buatan sendiri di folder mahasiswa/ bila sudah dibuat;
  tes bawaan Minggu02AsetTest.kodeKosongDitolak membuktikan penolakan spasi saja)
  
## A-03: rumus denda progresif
- Pertanyaan: rumus apa untuk DendaProgresif yang memenuhi syarat tes (0 hari = 0, tidak lebih murah dari flat, tidak turun, hari ke-30 lebih mahal dari flat)?
- Keputusan: 7 hari pertama sama dengan flat. Mulai hari ke-8, tiap hari tambahan dikenai 50% dari tarif harian.
  Rumus: denda = flat + tarifHarian × (hari − 7) / 2, dengan tarifHarian = aset.hitungDenda(1).
- Alasan: sederhana dijelaskan. Ada masa wajar 7 hari, lalu keterlambatan makin terasa. Tarif harian diambil dari
  hitungDenda(1), jadi multiplier jenis dan tarif Varian tetap dipakai tanpa hardcode. Angka 7 hari dan 50% adalah
  pilihan saya sendiri, bukan dari soal.
- Dampak ke kode: DendaProgresif.hitung (konstanta HARI_NORMAL = 7); DendaFlat dipakai sebagai pembanding.
- Tes yang membuktikan: Minggu05AbstraksiTest.dendaProgresifMemenuhiSyaratMinimal

Tambahkan di sini keputusan lain yang Anda ambil: rumus denda progresif, perubahan skema database, format error API, dan seterusnya.
