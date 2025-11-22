# Crypto Market App (Ionic + Vue + REST API)

Aplikasi **Crypto Market** adalah aplikasi mobile berbasis **Ionic Vue** yang menampilkan data harga cryptocurrency secara real-time dari REST API CoinLore.  
Project ini dibuat untuk memenuhi **Tugas 3 – Pemrograman Berbasis Perangkat Bergerak**.

---

## ✨ Fitur Utama

- ✅ Menampilkan daftar cryptocurrency dari API CoinLore
- ✅ Menampilkan **Rank, Nama, Simbol, Harga (USD)**  
- ✅ Tampilan modern berbentuk **card list** yang rapi dan profesional
- ✅ **Search bar** untuk mencari coin berdasarkan nama/simbol
- ✅ Tombol **Refresh**
- ✅ **Pull to Refresh**
- ✅ Loading state & Error handling
- ✅ Data otomatis update saat aplikasi dibuka

---

## 🧰 Teknologi yang Digunakan

- **Ionic Framework** (UI mobile hybrid)
- **Vue.js 3**
- **TypeScript**
- **Axios + Vue-Axios**
- **Vite**

---

## 🔗 Sumber Data API

Aplikasi mengambil data dari:

```
https://api.coinlore.net/api/tickers/
```

Contoh response utama:

```json
{
  "data": [
    {
      "id": "90",
      "symbol": "BTC",
      "name": "Bitcoin",
      "rank": "1",
      "price_usd": "..."
    }
  ]
}
```

---

## 📦 Instalasi & Menjalankan Project

### 1. Clone / Download Project
Jika menggunakan GitHub:

```bash
git clone <link-repository-anda>
cd crypto_app
```

Atau download ZIP, lalu extract.

---

### 2. Install dependency

```bash
npm install
```

---

### 3. Jalankan aplikasi

**Opsi A (tanpa Ionic CLI):**
```bash
npm run dev
```

**Opsi B (pakai Ionic CLI):**
```bash
ionic serve
```

Buka URL yang muncul di terminal (biasanya):

- `http://localhost:5173` (vite)
- `http://localhost:8100` (ionic)

---

## 🗂 Struktur Folder

```
crypto_app/
 ├─ src/
 │   ├─ components/
 │   │   └─ CoinListItem.vue   # Komponen card tiap item crypto
 │   ├─ views/
 │   │   └─ Home.vue           # Halaman utama + fetch API + search
 │   ├─ router/
 │   │   └─ index.ts           # Routing halaman
 │   ├─ App.vue
 │   ├─ main.ts                # Konfigurasi IonicVue + axios plugin
 │   └─ theme/
 │       └─ variables.css
 ├─ package.json
 ├─ vite.config.ts
 ├─ tsconfig.json
 └─ ionic.config.json
```

---

## 📌 Cara Kerja Singkat

1. Saat halaman `Home.vue` dimount, fungsi `loadData()` dipanggil.
2. `axios.get()` mengambil data dari API CoinLore.
3. Data disimpan dalam array `coins`.
4. Component `CoinListItem.vue` menampilkan tiap item sebagai card profesional.
5. Search bar memfilter data secara real-time menggunakan computed property `filteredCoins`.

---

## 🧪 Testing

Beberapa skenario yang telah diuji:

- ✅ Tampilan muncul normal saat API sukses diambil
- ✅ Error message muncul saat internet dimatikan
- ✅ Refresh memperbarui data
- ✅ Search memfilter nama/simbol dengan benar
- ✅ UI tetap rapi di berbagai ukuran layar

---

## 👤 Identitas Pembuat

- **Nama:** [Isi Nama Anda]  
- **NIM:** [Isi NIM Anda]  
- **Kelas:** [Isi Kelas Anda]  
- **Mata Kuliah:** Pemrograman Berbasis Perangkat Bergerak  

---

## 📜 Lisensi

Project ini dibuat untuk keperluan pembelajaran dan tugas akademik.  
Bebas digunakan sebagai referensi dengan tetap mencantumkan sumber.

---
