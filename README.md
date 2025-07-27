# 🚀 Panduan Install Tailwind CSS v4 di Laravel 10 + Vite

## 1. Install Laravel 10 (jika belum)
```bash
composer create-project laravel/laravel nama_Project "10.*"
```
### Buka/masuk ke dalam folder Project
```bash
cd nama_Project
```

## 2. Install Tailwind CSS + Vite dependencies
```bash
npm install -D vite laravel-vite-plugin tailwindcss@latest @tailwindcss/postcss postcss autoprefixer
```

## 3. Inisialisasi Konfigurasi Tailwind

### ⛔ Tailwind v4 tidak lagi membuat file otomatis
> Sejak Tailwind CSS v4, perintah `npx tailwindcss init` **tidak otomatis membuat** file `tailwind.config.js` dan `postcss.config.js`. Jadi harus dibuat **manual**!

### ✔️ Buat `tailwind.config.js` manual:
```js
export default {
  content: [
    './resources/views/**/*.blade.php',
    './resources/js/**/*.{js,vue,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [],
}
```

### ✔️ Buat juga `postcss.config.cjs` manual:

**Opsi 1 (Minimal - Recommended):**
```js
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {},
    autoprefixer: {}
  }
}
```

**Opsi 2 (Explicit config path):**
```js
module.exports = {
  plugins: {
    '@tailwindcss/postcss': {
      tailwindConfig: './tailwind.config.js'
    },
    autoprefixer: {}
  }
}
```

> 💡 **Tip:** Opsi 1 lebih simple karena Tailwind v4 auto-detect config file. Kedua opsi sudah tested dan work!

> ⚠️ **Penting:** Tanpa file ini, **Tailwind nggak bakal ke-compile dengan Vite**.

## 4. Buat atau cari File `resources/css/app.css`

> 📝 **Catatan:** File ini biasanya sudah ada di Laravel fresh install. Kalau belum ada, buat manual di `resources/css/app.css`.

Ganti atau tambahkan isi file dengan:
```css
@import 'tailwindcss';
```

> ⚠️ **Penting:** Di Tailwind v4, cukup pakai `@import 'tailwindcss';` saja. Jangan pakai yang lama seperti `@tailwind base; @tailwind components; @tailwind utilities;`

## 5. Update `vite.config.js` (Opsional, tapi jaga-jaga)
```js
import { defineConfig } from 'vite';
import laravel from 'laravel-vite-plugin';

export default defineConfig({
  plugins: [
    laravel({
      input: ['resources/css/app.css', 'resources/js/app.js'],
      refresh: true,
    }),
  ],
});
```

## 6. Panggil Vite di Blade
Contoh `resources/views/welcome.blade.php`:
```blade
<!DOCTYPE html>
<html>
<head>
    <title>Laravel + Tailwind</title>
    @vite(['resources/css/app.css', 'resources/js/app.js'])
</head>
<body class="bg-gray-100">
    <h1 class="text-3xl font-bold underline text-blue-500">
        Hello world!
    </h1>
</body>
</html>
```

## 7. Jalankan Dev Server
```bash
npm run dev
```
> 💡 Info: Di Laravel 10 + Vite, cuma ada npm run dev untuk development dan npm run build untuk production. Script watch sudah tidak ada lagi.

---

## 🧠 Troubleshooting

- **Jika style tidak muncul:**
  - Cek file `tailwind.config.js` dan pastikan path Blade benar.
  - Hapus `node_modules` dan `package-lock.json`, lalu ulang:
    ```bash
    rm -rf node_modules package-lock.json
    npm install
    npm run dev
    ```
  - Pastikan `@vite` dipakai, **jangan** `asset()`.

- **Jika PostCSS error:**
  - Pastikan pakai `postcss.config.cjs` (extension `.cjs`, bukan `.js`)
  - Coba kedua opsi konfigurasi PostCSS di atas

- **Debug build issues:**
  ```bash
  npm run build --debug
  ```

---

## 🧩 Struktur Folder Default
- CSS: `resources/css/app.css`
- JS: `resources/js/app.js`
- Blade layout (optional): `resources/views/layouts/app.blade.php`

---

## ⚙️ Plugin Tailwind Tambahan (Opsional)

### Install:
```bash
npm install -D @tailwindcss/forms @tailwindcss/typography
```

### Tambahkan ke `tailwind.config.js`:
```js
export default {
  content: [
    './resources/views/**/*.blade.php',
    './resources/js/**/*.{js,vue,ts,jsx,tsx}',
  ],
  theme: {
    extend: {},
  },
  plugins: [
    require('@tailwindcss/forms'),
    require('@tailwindcss/typography'),
  ],
}
```

---

## 🌙 Gunakan Dark Mode (Rekomendasi)
Tambahkan ke `tailwind.config.js`:
```js
export default {
  content: [
    './resources/views/**/*.blade.php',
    './resources/js/**/*.{js,vue,ts,jsx,tsx}',
  ],
  darkMode: 'class',
  theme: {
    extend: {},
  },
  plugins: [],
}
```

Lalu gunakan `class="dark"` di tag `<html>` atau `<body>`.

---

## 💡 Tips Debug Tambahan

- Jalankan `npm run build --debug` untuk lihat log error saat build.
- Cek apakah Vite berhasil mendeteksi file Tailwind di browser console.
- Coba hapus cache browser atau inspect network file CSS.
- Pastikan file `postcss.config.cjs` ada dan formatnya benar.

---

## 📋 Requirements
- Laravel 10.x
- Node.js >= 20.x
- NPM atau Yarn
- Tailwind CSS v4.x

---

## 🔄 Tested Configuration
- **Laravel:** 10.x
- **Tailwind CSS:** v4.x
- **Vite: v5.4.19 (Recommended - stable)
- **Node.js:** 22.x

---

> 📚 Panduan ini cocok untuk Laravel 10 yang menggunakan Vite sebagai bundler. Sudah tested dengan Tailwind CSS v4 dan kedua opsi PostCSS config sudah confirmed work!

---

**Kalau panduan ini membantu, jangan lupa kasih ⭐ ya!** 😄

## 🤝 Contributing
Ada masalah atau punya saran improvement? Feel free untuk buka issue atau submit PR!
