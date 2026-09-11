# Virtual Lab Sistem Pencernaan

Media pembelajaran interaktif sistem pencernaan manusia untuk IPA Kelas 8
(Kurikulum Merdeka). Satu file HTML, tanpa dependency, tanpa build step.

Isi lab: peta organ, perjalanan bolus, uji makanan, tabel pengamatan
(salin / unduh CSV), mode tebak, dan kuis 10 soal acak.

## 🔗 Link lab

Setelah GitHub Pages aktif (lihat di bawah), lab bisa dibuka di:

```
https://mathiasbagas11-ai.github.io/Virtual-lab-sistem-pencernaan/
```

## 🚀 Mengaktifkan GitHub Pages (wajib, sekali saja)

Lab ini tayang lewat **Deploy from a branch**: GitHub menyajikan langsung
isi `main` tanpa alur kerja apa pun. Tidak ada langkah build karena lab
hanya terdiri dari satu berkas HTML dan gambar.

Langkah ini **harus dikerjakan manual sekali** dan tidak bisa
diotomatiskan: membuat Pages site lewat API memerlukan izin
`administration: write`, dan `GITHUB_TOKEN` milik alur kerja tidak pernah
mendapat izin itu.

1. Buka **Settings → Pages** di repo ini.
2. Pada **Build and deployment → Source**, pilih **Deploy from a branch**.
3. Di baris **Branch**, pilih `main` dan folder `/ (root)`, lalu **Save**.

Tunggu sekitar satu menit, lalu buka
`https://mathiasbagas11-ai.github.io/Virtual-lab-sistem-pencernaan/`.

Setelah itu tiap push ke `main` tayang otomatis tanpa langkah tambahan.
Berkas `.nojekyll` membuat GitHub menyajikan berkas apa adanya, tanpa
diproses Jekyll.

## 📌 Cara menempel di website lain

### Cara paling gampang (tinggi tetap)

Tempel di Google Sites (Embed → Embed code), Moodle, Blogger, WordPress
(blok Custom HTML), Canvas LMS, atau halaman HTML mana pun:

```html
<iframe
  src="https://mathiasbagas11-ai.github.io/Virtual-lab-sistem-pencernaan/"
  title="Virtual Lab Sistem Pencernaan"
  width="100%"
  height="900"
  style="border:0; border-radius:12px;"
  loading="lazy"
  allow="fullscreen; clipboard-write"
  allowfullscreen
  referrerpolicy="no-referrer-when-downgrade"></iframe>
```

Atribut yang penting:

| Atribut | Gunanya |
| --- | --- |
| `allow="fullscreen"` | tombol ⛶ Fullscreen berfungsi penuh |
| `allow="clipboard-write"` | tombol 📋 Salin Tabel berfungsi |
| `allowfullscreen` | kompatibilitas browser lama |

Kalau atribut ini tidak dipasang (atau platform-nya melarang), lab tetap
jalan: fullscreen jatuh ke mode layar-penuh semu, dan salin tabel jatuh ke
`execCommand('copy')`.

### Tinggi otomatis (untuk halaman HTML sendiri)

Lab mengirim tinggi kontennya ke halaman induk lewat `postMessage`, jadi
iframe bisa menyesuaikan diri tanpa scrollbar ganda:

```html
<iframe id="labPencernaan"
  src="https://mathiasbagas11-ai.github.io/Virtual-lab-sistem-pencernaan/"
  title="Virtual Lab Sistem Pencernaan"
  width="100%" height="900"
  style="border:0; border-radius:12px;"
  allow="fullscreen; clipboard-write" allowfullscreen></iframe>

<script>
  window.addEventListener('message', function (e) {
    var d = e.data;
    if (!d || d.type !== 'lab-pencernaan:height') return;
    document.getElementById('labPencernaan').style.height = d.height + 'px';
  });
</script>
```

Untuk lebih ketat, saring juga asalnya:
`if (e.origin !== 'https://mathiasbagas11-ai.github.io') return;`

### Sandbox

Kalau platform memaksa atribut `sandbox`, minimal sertakan:

```html
sandbox="allow-scripts allow-same-origin allow-popups allow-downloads allow-modals"
```

Tanpa `allow-downloads`, tombol ⬇️ Unduh CSV diblokir browser — pakai
📋 Salin Tabel lalu tempel ke Google Sheets / Excel sebagai gantinya.

## 💻 Menjalankan secara lokal

Buka `index.html` langsung di browser sudah cukup. Kalau ingin persis
seperti versi online:

```bash
python3 -m http.server 8000
# lalu buka http://localhost:8000
```

## 📁 Struktur

```
index.html                        # seluruh lab (HTML + CSS + JS)
pencernaan.webp                   # peta organ utama
g-*.webp                          # ilustrasi per stasiun
.nojekyll                         # matikan pemrosesan Jekyll di Pages
```

Nama berkas gambar dirujuk dari `ALL_BASES` di dalam `index.html` **tanpa
ekstensi**. Tombol **🔍 Cek Gambar** di header menguji semua gambar satu
per satu dan melaporkan mana yang tidak ketemu.

## 🖼️ Catatan format gambar

Ilustrasi disimpan sebagai **WebP kualitas 90**, bukan PNG: total aset
turun dari 21,8 MB menjadi 1,6 MB (hemat 92,8%) tanpa perbedaan yang
terlihat pada teks label. Ini penting karena lab sering dibuka lewat WiFi
sekolah dan di dalam iframe.

WebP didukung semua browser modern (Chrome, Firefox, Safari 14+, Edge).
Pemuat gambar di `index.html` tetap mencoba `.png`, `.jpg`, dan `.jpeg`
sebagai cadangan, jadi menambah gambar berformat lain tetap bisa tanpa
mengubah kode.

Kalau ingin mengganti atau menambah ilustrasi, simpan sebagai WebP:

```bash
python3 -c "
from PIL import Image
im = Image.open('gambar-baru.png').convert('RGB')
im.save('gambar-baru.webp', 'WEBP', quality=90, method=6)
"
```
