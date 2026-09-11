# Catatan untuk Claude

## Alur kerja git — JANGAN bikin pull request

Pemilik repo ini bekerja sendiri, tanpa reviewer. **Commit dan push
langsung ke `main`.** Jangan membuat branch kerja, jangan membuka pull
request, kecuali pemilik memintanya secara eksplisit di percakapan itu.

Alasannya: tiap push ke `main` langsung tayang lewat GitHub Pages, dan PR
per perubahan cuma menambah klik tanpa memberi manfaat review.

Karena tidak ada jeda review, **wajib diuji sebelum push**: jalankan lab
di browser headless, pastikan nol error JavaScript, dan lihat hasil
render minimal sekali. Lihat "Cara menguji" di bawah.

## Isi repo

Lab ini **satu berkas HTML** (`index.html`) berisi seluruh markup, CSS,
dan JavaScript — tanpa build step, tanpa dependency JavaScript. Satu-satunya
sumber eksternal adalah Google Fonts; kalau diblokir, lab jatuh ke font
sistem dan semua fitur tetap jalan. Jangan menambah bundler, framework,
atau memecah berkas tanpa diminta.

Gambar disimpan sebagai WebP di folder utama dan dirujuk **tanpa ekstensi**
lewat `ALL_BASES` di `index.html`.

Bahasa antarmuka dan pesan commit: **Bahasa Indonesia**.

## Cara menguji

```bash
python3 -m http.server 8765      # dari akar repo
```

Chromium sudah terpasang di lingkungan remote:
`/opt/pw-browsers/chromium-1194/chrome-linux/chrome`. Pakai Playwright
(`npm install playwright --no-save` di direktori scratchpad) dengan
`chromium.launch({ executablePath: ... })` — jangan jalankan
`playwright install`.

Yang minimal dicek tiap kali menyentuh `index.html`:

1. `node --check` pada isi blok `<script>` yang diekstrak.
2. Nol `pageerror` saat memuat halaman.
3. Screenshot tiap stasiun yang disentuh, plus lebar 400px dan tema gelap.
4. Kalau menyentuh penyimpanan: muat ulang halaman, pastikan tabel dan
   lencana kembali utuh.

## Setelah push

GitHub Pages tayang lewat **Deploy from a branch** (`main`, root), jadi
rebuild jalan otomatis — tidak ada berkas workflow yang perlu diurus.

HTML disajikan dengan `Cache-Control: max-age=600`, jadi perubahan tidak
langsung terlihat. Beri tahu pemilik untuk hard refresh
(`Cmd`/`Ctrl` + `Shift` + `R`) dan, kalau perlu, tambahkan `?v=N` di URL.
Query string juga wajib diganti kalau lab sudah ditempel sebagai iframe di
Google Sites atau Moodle, karena platform itu punya cache sendiri.

Catatan lingkungan: proxy sesi remote menolak `github.io`, jadi situs live
tidak bisa diambil dari sini. Verifikasi lewat isi repo dan status run
"pages build and deployment" di GitHub Actions.
