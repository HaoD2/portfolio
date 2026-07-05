# 🎨 Portfolio Redesign Plan & Updated Source Code

## ✅ Arsitektur & Struktur Saat Ini
- Single File Component (SFC) Vue 3 (`<script setup>`)
- State management via `ref` & `computed`
- Responsive grid layout dengan CSS native
- Scoped styling tanpa framework eksternal
- Siap untuk refactor desain tanpa mengubah logika

## 🎯 Spesifikasi Redesign yang Diterapkan
1. **Background Dark-Midnight Purple** → Layered radial gradients + subtle mesh effect
2. **Card Design** → Enhanced glassmorphism, refined borders, hover lift, inner glow
3. **Typography** → Optimized scale (`clamp()`), better line-height, consistent weights
4. **Icons (Tech & Social)** → Unified sizing, proper alignment, CSS variable-driven colors

## 📦 Full Updated `App.vue`
[Code block here]

## 🔧 Cara Integrasi
- Replace seluruh isi `src/App.vue` dengan kode di atas
- Tidak perlu install dependency tambahan
- Semua animasi & interaksi tetap berfungsi

## 🚀 Next Steps (Opsional)
- Tambahkan CSS noise texture via `::before` pseudo-element
- Konversi warna ke CSS variables untuk theming dinamis
- Optimasi gambar project dengan lazy loading (`loading="lazy"`)
