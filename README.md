# 🚀 Panduan Install Tailwind CSS v4 di Laravel 10 + Vite

## 📋 Requirements
- Composer vLatest
- Laravel 10.x
- Node.js >= 20.x
- NPM atau Yarn
- Tailwind CSS v4.x

---

## 🔄 Tested Configuration
- **Laravel:** 10.x
- **Tailwind CSS:** v4.x
- **Vite:** v5.4.19 (Recommended - stable)
- **Node.js:** 22.x

---

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

## 6. Setup Layout dan Blade Templates

### 🎨 **REKOMENDASI: Gunakan Layout System yang Proper!**

Alih-alih langsung coding di `welcome.blade.php`, sebaiknya gunakan layout system yang lebih terstruktur dan professional. 

> 📖 **[Lihat Panduan Lengkap Laravel Layout Best Practice →](https://github.com/ahmad-syaifuddin/tailwind4-best-practice-laravel10)**

Panduan tersebut mencakup:
- ✅ Struktur layout yang scalable
- ✅ Component-based architecture  
- ✅ Dark mode support
- ✅ Mobile responsive design
- ✅ SEO optimization
- ✅ Accessibility features
- ✅ Performance optimization

### 📝 Contoh Quick Start (Welcome Page)
Jika ingin langsung test, buat file `resources/views/welcome.blade.php`:

```blade
<!DOCTYPE html>
<html lang="en" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    @vite(['resources/css/app.css', 'resources/js/app.js'])
    <title>Laravel + Tailwind v4</title>
</head>
<body class="bg-gradient-to-br from-gray-900 to-gray-800 text-gray-100 font-sans">
    <div class="min-h-screen flex flex-col items-center justify-center p-4">
        <div class="max-w-2xl w-full bg-white/5 backdrop-blur-md rounded-2xl border border-white/10 p-8 shadow-2xl transition-all hover:shadow-indigo-500/20">
            <h1 class="text-5xl md:text-6xl font-extrabold bg-gradient-to-r from-indigo-400 to-purple-500 bg-clip-text text-transparent mb-6">
                Welcome to <span class="animate-pulse">v4</span>
            </h1>
            
            <p class="text-lg text-gray-300 mb-8">
                The future of CSS with Laravel & Vite
            </p>
            
            <div class="space-y-4">
                <button class="w-full px-6 py-3 bg-indigo-600 hover:bg-indigo-700 rounded-lg font-medium transition-all transform hover:-translate-y-1">
                    Get Started
                </button>
                
                <a href="https://github.com/ahmad-syaifuddin/tailwind4-best-practice-laravel10" 
                   class="block w-full px-6 py-3 bg-white/10 hover:bg-white/20 rounded-lg font-medium text-center transition-all border border-white/20">
                    📖 View Layout Best Practice
                </a>
            </div>
            
            <div class="mt-8 flex gap-4 flex-wrap justify-center">
                <span class="px-3 py-1 bg-white/10 rounded-full text-xs font-mono">Laravel 10</span>
                <span class="px-3 py-1 bg-white/10 rounded-full text-xs font-mono">Tailwind v4</span>
                <span class="px-3 py-1 bg-white/10 rounded-full text-xs font-mono">Vite</span>
            </div>
        </div>
    </div>
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
- Blade layout (recommended): `resources/views/layouts/app.blade.php`

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

## 🎯 Next Steps

Setelah instalasi berhasil, disarankan untuk:

1. **Setup Layout System**: Gunakan panduan layout best practice untuk struktur yang lebih maintainable
2. **Component Library**: Buat komponen reusable untuk button, form, card, dll
3. **Authentication**: Integrate dengan Laravel Breeze atau Jetstream
4. **Database Design**: Setup model dan migration
5. **Testing**: Tambahkan unit test dan feature test

---

> 📚 Panduan ini cocok untuk Laravel 10 yang menggunakan Vite sebagai bundler. Sudah tested dengan Tailwind CSS v4 dan kedua opsi PostCSS config sudah confirmed work!

---

**Kalau panduan ini membantu, jangan lupa kasih ⭐ ya!** 😄

## 🤝 Contributing
Ada masalah atau punya saran improvement? Feel free untuk buka issue atau submit PR!

---

## 📖 Related Resources

- 🎨 **[Laravel Layout Best Practice](https://github.com/ahmad-syaifuddin/tailwind4-best-practice-laravel10)** - Complete layout system guide
- 📚 [Laravel 10 Documentation](https://laravel.com/docs/10.x)
- 🎨 [Tailwind CSS v4 Documentation](https://tailwindcss.com/docs)
- ⚡ [Vite Laravel Plugin](https://laravel.com/docs/10.x/vite)
