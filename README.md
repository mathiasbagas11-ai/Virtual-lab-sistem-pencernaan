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

## 🚀 Mengaktifkan GitHub Pages (sekali saja)

1. Buka **Settings → Pages** di repo ini.
2. Bagian **Build and deployment → Source**, pilih **GitHub Actions**.
3. Selesai. Tiap push ke `main` otomatis ter-deploy lewat
   `.github/workflows/deploy-pages.yml`.

> Alternatif tanpa Actions: pilih **Deploy from a branch** → `main` → `/ (root)`.
> Berkas `.nojekyll` sudah ada supaya Jekyll tidak mengubah struktur file.

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
pencernaan.png                    # peta organ utama
g-*.png                           # ilustrasi per stasiun
.nojekyll                         # matikan pemrosesan Jekyll di Pages
.github/workflows/deploy-pages.yml
```

Nama berkas gambar dirujuk dari `ALL_BASES` di dalam `index.html`. Tombol
**🔍 Cek Gambar** di header menguji semua gambar satu per satu dan
melaporkan mana yang tidak ketemu.
