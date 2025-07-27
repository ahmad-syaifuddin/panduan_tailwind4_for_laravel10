
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
npm install -D tailwindcss
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

### ✔️ Buat juga `postcss.config.js` manual:

```js
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

Tanpa ini, **Tailwind nggak bakal ke-compile dengan Vite**.

## 4. Buat File `resources/css/app.css`

```css
@import 'tailwindcss';
```

## 5. Update `vite.config.js`

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

---

## 🧠 Troubleshooting

- Jika style tidak muncul:
  - Cek file `tailwind.config.js` dan pastikan path Blade benar.
  - Hapus `node_modules` dan `package-lock.json`, lalu ulang:
    ```bash
    rm -rf node_modules package-lock.json
    npm install
    npm run dev
    ```
  - Pastikan `@vite` dipakai, **jangan** `asset()`.

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
plugins: [
  require('@tailwindcss/forms'),
  require('@tailwindcss/typography'),
]
```

---

## 🌙 Gunakan Dark Mode (Rekomendasi)

Tambahkan ke `tailwind.config.js`:

```js
darkMode: 'class',
```

Lalu gunakan `class="dark"` di tag `<html>` atau `<body>`.

---

## 💡 Tips Debug Tambahan

- Jalankan `npm run build --debug` untuk lihat log error saat build.
- Cek apakah Vite berhasil mendeteksi file Tailwind.
- Coba hapus cache browser atau inspect network file CSS.

---

> 📚 Panduan ini cocok untuk Laravel 10 yang menggunakan Vite sebagai bundler. Tested with Tailwind CSS v4.

---

Kalau kamu suka panduan ini, jangan lupa kasih ⭐ repo GitHub ini ya 😄
