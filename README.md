# yudha-mega-p
aplikasi
# yudha-mega-p
aplikasi

## 📝 Belajar Membuat Aplikasi: Daftar Tugas (To-Do List)

Aplikasi web sederhana untuk belajar dasar pemrograman. Tidak perlu install apa pun, cukup pakai browser.

### Cara menjalankan
1. Download / clone repo ini.
2. Klik dua kali file `index.html`, nanti file itu terbuka di browser (Chrome, Firefox, dll).
3. Selesai! Coba tambah, centang, dan hapus tugas.

### Tiga bahan utama aplikasi web

| File | Peran | Analogi rumah |
|------|-------|---------------|
| `index.html` | **Struktur**: apa saja yang ada di layar | Tembok & ruangan |
| `style.css` | **Tampilan**: warna, ukuran, jarak | Cat & dekorasi |
| `app.js` | **Logika**: apa yang terjadi saat diklik | Listrik & air |

Buka ketiga file itu. Setiap bagian sudah diberi komentar berbahasa Indonesia.

### Alur kerja aplikasi (inti dari hampir semua aplikasi)

```
Pengguna klik / ketik  →  EVENT
        ↓
Fungsi AKSI mengubah   →  DATA (array tugasTugas)
        ↓
Data disimpan          →  localStorage (tidak hilang saat refresh)
        ↓
Fungsi tampilkan()     →  LAYAR diperbarui
```

Kalau kamu paham pola **Event → Data → Tampilan** ini, kamu sudah paham dasar
cara kerja aplikasi, termasuk aplikasi besar seperti Instagram atau Tokopedia.

### 🏋️ Latihan (coba sendiri, dari yang mudah ke yang sulit)
1. **Ganti warna** tombol di `style.css` (cari `#2f6fed`).
2. **Ganti judul** aplikasi di `index.html`.
3. **Tambah tombol "Hapus semua yang selesai"**.
   Petunjuk: pakai `tugasTugas.filter(t => !t.selesai)`.
4. **Tambah filter**: tombol "Semua / Aktif / Selesai".
5. **Tambah tanggal** pada setiap tugas (petunjuk: `new Date().toLocaleDateString('id-ID')`).

### Langkah belajar berikutnya
- HTML & CSS dasar: https://developer.mozilla.org/id/docs/Learn
- JavaScript dasar: https://javascript.info
- Setelah itu: coba framework seperti React, atau buat aplikasi HP dengan Flutter.
/*
  LANGKAH 3: JavaScript = OTAK / LOGIKA aplikasi.
  JavaScript membuat halaman bisa "berbuat sesuatu":
  menambah tugas, mencentang, menghapus, dan menyimpan data.
*/

// --- 1. Ambil elemen HTML yang akan kita pakai ---
const form = document.getElementById('form-tugas');
const input = document.getElementById('input-tugas');
const daftar = document.getElementById('daftar-tugas');
const info = document.getElementById('info');

// --- 2. DATA: semua tugas disimpan di sebuah array (daftar) ---
// Setiap tugas adalah objek: { id, teks, selesai }
// Kita coba ambil data lama dari localStorage (penyimpanan di browser),
// supaya tugas tidak hilang saat halaman di-refresh.
let tugasTugas = muatData();

function muatData() {
  try {
    const dataTersimpan = localStorage.getItem('tugas');
    return dataTersimpan ? JSON.parse(dataTersimpan) : [];
  } catch {
    return []; // kalau gagal, mulai dengan daftar kosong
  }
}

function simpanData() {
  try {
    localStorage.setItem('tugas', JSON.stringify(tugasTugas));
  } catch {
    // Abaikan: aplikasi tetap jalan walau tidak bisa menyimpan
  }
}

// --- 3. TAMPILKAN: ubah data menjadi elemen HTML di layar ---
function tampilkan() {
  daftar.innerHTML = ''; // kosongkan dulu, lalu gambar ulang

  for (const tugas of tugasTugas) {
    const li = document.createElement('li');
    li.className = tugas.selesai ? 'tugas selesai' : 'tugas';

    const centang = document.createElement('input');
    centang.type = 'checkbox';
    centang.checked = tugas.selesai;
    centang.addEventListener('change', () => ubahStatus(tugas.id));

    const teks = document.createElement('span');
    teks.textContent = tugas.teks; // textContent aman dari kode jahat
    teks.addEventListener('click', () => ubahStatus(tugas.id));

    const hapus = document.createElement('button');
    hapus.className = 'tombol-hapus';
    hapus.textContent = '✕';
    hapus.title = 'Hapus tugas';
    hapus.addEventListener('click', () => hapusTugas(tugas.id));

    li.append(centang, teks, hapus);
    daftar.append(li);
  }

  // Ringkasan di bawah daftar
  const sisa = tugasTugas.filter((t) => !t.selesai).length;
  info.textContent = tugasTugas.length === 0
    ? 'Belum ada tugas. Yuk tambahkan satu!'
    : `${sisa} dari ${tugasTugas.length} tugas belum selesai.`;
}

// --- 4. AKSI: fungsi-fungsi yang mengubah data ---
function tambahTugas(teks) {
  tugasTugas.push({ id: Date.now(), teks: teks, selesai: false });
  simpanData();
  tampilkan();
}

function ubahStatus(id) {
  const tugas = tugasTugas.find((t) => t.id === id);
  tugas.selesai = !tugas.selesai; // balik: true <-> false
  simpanData();
  tampilkan();
}

function hapusTugas(id) {
  tugasTugas = tugasTugas.filter((t) => t.id !== id);
  simpanData();
  tampilkan();
}

// --- 5. EVENT: jalankan aksi saat pengguna menekan tombol "Tambah" ---
form.addEventListener('submit', (event) => {
  event.preventDefault(); // cegah halaman reload (perilaku bawaan form)
  const teks = input.value.trim();
  if (teks === '') return;
  tambahTugas(teks);
  input.value = '';
  input.focus();
});

// Tampilkan daftar pertama kali saat halaman dibuka
tampilkan();
/*
  LANGKAH 2: CSS = TAMPILAN / DESAIN aplikasi.
  CSS mengatur warna, ukuran, jarak, dan posisi elemen HTML.
  Cara bacanya:  selektor { properti: nilai; }
*/

/* Semua elemen: hitung ukuran termasuk padding & border */
* {
  box-sizing: border-box;
}

body {
  margin: 0;
  min-height: 100vh;
  font-family: system-ui, sans-serif;
  background: #eef2f7;
  display: flex;
  justify-content: center;   /* tengah secara horizontal */
  align-items: flex-start;
  padding: 40px 16px;
}

/* ".kartu" artinya: elemen yang punya class="kartu" */
.kartu {
  width: 100%;
  max-width: 480px;
  background: white;
  padding: 24px;
  border-radius: 16px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.08);
}

h1 {
  margin-top: 0;
  font-size: 1.5rem;
}

/* "#form-tugas" artinya: elemen yang punya id="form-tugas" */
#form-tugas {
  display: flex;
  gap: 8px;
}

#input-tugas {
  flex: 1;                   /* input mengisi sisa ruang */
  padding: 10px 12px;
  font-size: 1rem;
  border: 1px solid #ccd3dd;
  border-radius: 8px;
}

button {
  padding: 10px 16px;
  font-size: 1rem;
  border: none;
  border-radius: 8px;
  background: #2f6fed;
  color: white;
  cursor: pointer;
}

button:hover {
  background: #1f56c4;
}

#daftar-tugas {
  list-style: none;
  padding: 0;
  margin: 20px 0 0;
}

.tugas {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 10px 0;
  border-bottom: 1px solid #eef0f3;
}

.tugas span {
  flex: 1;
  cursor: pointer;
}

/* Tugas yang sudah selesai: dicoret dan dibuat pudar */
.tugas.selesai span {
  text-decoration: line-through;
  color: #9aa3af;
}

.tombol-hapus {
  background: transparent;
  color: #d33;
  padding: 4px 8px;
}

.tombol-hapus:hover {
  background: #fdecec;
}

.info {
  color: #6b7280;
  font-size: 0.9rem;
  margin-bottom: 0;
}
