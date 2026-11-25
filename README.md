# PorsiBalik

PorsiBalik adalah aplikasi web lokal untuk membantu kantin kecil atau UMKM makanan mencatat jumlah porsi dibuat, terjual, tersisa, dan permintaan yang tidak terpenuhi. Catatan tersebut diubah menjadi ringkasan sisa serta rekomendasi produksi berikutnya.

## Masalah dan bukti

Proyek ini didasarkan pada sumber publik, bukan observasi atau wawancara lapangan:

- **Bappenas, Laporan Pelaksanaan SDGs 2021** (dipublikasikan 2023, cakupan nasional) mencatat timbulan *food loss and waste* 2000–2019 sebesar 115–184 kg/kapita/tahun dan menyebut kelebihan porsi sebagai salah satu penyebab.  
  <https://sdgs.bappenas.go.id/website/wp-content/uploads/2023/02/LAPORAN-PELAKSANAAN-SDGS-2021.pdf>
- **Pemerintah Provinsi NTB, 2026** (cakupan NTB) menyatakan sekitar 40% timbunan sampah berupa sisa makanan dan mendorong gerakan Stop Boros Pangan.  
  <https://ntbprov.go.id/post/ketahanan-pangan-ntb-berpacu-dari-produksi-stok-hingga-konsumsi>
- **Pemerintah Kota Mataram, 27 Januari 2026** menjelaskan pengurangan sampah organik berbasis sumber akan diperluas ke pasar, lembaga pendidikan, serta ruang usaha dan bisnis.  
  <https://web.mataramkota.go.id/tempah-dedoro-jawaban-pengelolaan-sampah-kota-mataram/>

Temuan yang didukung bukti adalah besarnya masalah sisa pangan dan perlunya pengurangan dari sumber. Asumsi desainnya: kantin atau UMKM kecil bersedia mencatat data harian. Data contoh sepenuhnya sintetis; aplikasi belum diuji lapangan di kantin Mataram.

## Fitur

1. Pencatatan porsi dibuat, terjual, tersisa, dan permintaan tak terpenuhi.
2. Ringkasan rasio sisa, omzet perkiraan, dan grafik tujuh catatan terbaru.
3. Rekomendasi produksi per menu berdasarkan median permintaan tujuh catatan terakhir.
4. Pengelolaan katalog menu dan harga.
5. Autosave, impor/ekspor JSON, serta ekspor CSV yang aman dari formula spreadsheet.

## Jaminan perilaku

Rekomendasi bersifat deterministik: urutan catatan masukan tidak mengubah hasil karena catatan selalu diurutkan berdasarkan tanggal sebelum dihitung. Median dipilih agar satu hari ekstrem tidak langsung mendominasi. Perubahan rekomendasi dibatasi maksimal ±20% dari produksi terakhir agar hasil tidak melonjak tajam.

Catatan hanya diterima jika `terjual + tersisa = dibuat`, seluruh angka berupa bilangan bulat nonnegatif, dan tidak ada tanggal ganda untuk menu yang sama. Impor divalidasi seluruhnya sebelum mengganti data aktif.

## Menjalankan

Syarat: Node.js 20 atau lebih baru.

```bash
npm start
```

Buka <http://127.0.0.1:4173>. Tidak ada dependensi eksternal.

1. Atur menu dan harga pada tab **Menu**.
2. Masukkan hasil hari ini melalui **Catat harian**.
3. Lihat rasio sisa, tren, dan rekomendasi pada **Ringkasan**.
4. Gunakan **Ekspor** untuk menyimpan proyek JSON dan catatan CSV.

Contoh impor tersedia di [`examples/data-contoh.json`](examples/data-contoh.json).

## Pengujian

```bash
npm test
npm run check
```

Enam tes mencakup alur pencatatan hingga rekomendasi, determinisme terhadap urutan data, batas lonjakan 20%, transaksi yang tidak seimbang, tanggal ganda, angka negatif, impor rusak, dan keamanan CSV.

## Struktur

```text
porsibalik/
├── assets/logo.svg
├── examples/data-contoh.json
├── src/core.js
├── src/app.js
├── tests/core.test.js
├── index.html
├── styles.css
└── server.js
```

## Batasan

- Rekomendasi hanya bantuan keputusan, bukan ramalan permintaan yang teruji secara statistik.
- Belum mempertimbangkan cuaca, hari libur, promosi, atau masa kedaluwarsa bahan.
- Omzet adalah perkiraan `jumlah terjual × harga`, bukan laba.
- Data tersimpan hanya di browser dan tidak tersinkron antarperangkat.

## Lisensi

MIT.
