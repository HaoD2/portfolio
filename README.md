# Panduan Deploy ke GitHub Pages

## Konfigurasi Selesai ✅

Berikut konfigurasi yang telah disiapkan:

1. **Konfigurasi Vite** (`vite.config.ts`)
   - Mengatur `base: '/portfolio/'` agar aplikasi dapat berjalan di GitHub Pages.

2. **GitHub Actions** (`.github/workflows/deploy.yml`)
   - Workflow untuk build dan deploy secara otomatis.

3. **Script NPM** (`package.json`)
   - Menambahkan perintah `deploy`.

---

# Opsi 1 - Deploy Manual

## 1. Install package `gh-pages`

```bash
npm install --save-dev gh-pages
```

## 2. Tambahkan Homepage pada `package.json`

Tambahkan properti berikut setelah `"name"`:

```json
"homepage": "https://{username}.github.io/portfolio/"
```

Ganti `{username}` dengan username GitHub milik Anda.

Contoh:

```json
"homepage": "https://haod2.github.io/portfolio/"
```

## 3. Jalankan Deploy

```bash
npm run deploy
```

Perintah tersebut akan:

- Melakukan build project (`npm run build`)
- Mengirim hasil build (`dist`) ke branch `gh-pages`

---

# Opsi 2 - Deploy Otomatis Menggunakan GitHub Actions (Disarankan)

## 1. Push project ke GitHub

```bash
git add .
git commit -m "Menambahkan konfigurasi GitHub Pages"
git push origin main
```

## 2. Tunggu proses GitHub Actions

Setelah proses **push** selesai, GitHub Actions akan otomatis:

- Menginstall dependency
- Melakukan build project
- Melakukan deploy ke GitHub Pages

Anda dapat melihat progresnya pada tab **Actions** di repository GitHub.

---

# Konfigurasi GitHub Pages

1. Buka halaman pengaturan repository:

```
https://github.com/HaoD2/portfolio/settings
```

2. Pilih menu **Pages**.

3. Pada bagian **Build and deployment**, pilih salah satu:

### Jika menggunakan `gh-pages`

- Source: **Deploy from a branch**
- Branch: **gh-pages**
- Folder: **/(root)**

### Jika menggunakan GitHub Actions

- Source: **GitHub Actions**

---

# Verifikasi Deploy

Jika proses deploy berhasil, website dapat diakses melalui:

```
https://haod2.github.io/portfolio/
```

---

# Catatan

- Pastikan repository bersifat **Public** atau GitHub Pages telah diaktifkan.
- Jika menggunakan nama repository selain **portfolio**, ubah nilai `base` pada `vite.config.ts` dan `homepage` di `package.json` sesuai nama repository tersebut.
- Setiap kali melakukan `git push` ke branch **main**, GitHub Actions akan melakukan deploy ulang secara otomatis (jika menggunakan metode GitHub Actions).