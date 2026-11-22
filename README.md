# Toentoen Web

Website resmi dan dashboard internal **Toentoen**, jembatan digital yang menghubungkan kebutuhan masyarakat dengan potensi bisnis lokal melalui teknologi berbasis lokasi.

Repository ini berisi dua bagian:

1. **Website publik** - halaman profil perusahaan untuk pengguna dan mitra bisnis.
2. **Dashboard internal** - panel kerja tim Toentoen (admin, sales, CMO, social media) yang diakses lewat login.

---

## Fitur

### Website publik

| Halaman | Route | Isi |
|---|---|---|
| Beranda | `/` | Hero, fitur, platform user & merchant, target pengguna, berita terbaru |
| Tentang | `/about` | Profil, visi-misi, perjalanan Toentoen |
| Produk | `/product` | Fitur untuk user dan merchant |
| Berita | `/news`, `/news/[id]` | Daftar dan detail berita |
| Tim | `/team` | Profil tim |
| Kebijakan Privasi | `/privacy-policy` | Kebijakan privasi |
| Customer Service | `/customer-service` | Workspace live chat dan tiket |

### Dashboard internal

Setelah login di `/login`, pengguna diarahkan otomatis ke dashboard sesuai **role** yang dikirim API (`data.user.role`):

| Role | Route | Fitur utama |
|---|---|---|
| Admin (Supervisor) | `/login/admin/supervisor` | Dashboard performa, inbox, report & support ticket (Kanban), **verifikasi KYC** |
| Admin (Staff) | `/login/admin/staff` | Dashboard, inbox tiket pelanggan, report & support ticket |
| Sales Staff | `/login/sales/staff` | Dashboard penjualan, pengaturan akun |
| Sales Supervisor | `/login/sales/supervisor` | Dashboard tim, detail performa staff, pengaturan akun |
| CMO | `/login/cmo` | Dashboard admin & social media, daftar post |
| Social Media | `/login/social-media` | Dashboard, buat & jadwalkan post, chat, kredensial akun |

**Verifikasi KYC** (Supervisor Admin) dipakai untuk meninjau pengajuan identitas (KTP, dll.) dari owner dan business admin:

- melihat semua pengajuan, dengan filter status, role, pencarian, dan pagination;
- melihat detail pengajuan beserta dokumen;
- menyetujui atau menolak pengajuan (alasan penolakan minimal 5 kata).

### Responsif

Semua halaman disesuaikan untuk HP, tablet, dan desktop. Di HP, sidebar dashboard berubah menjadi navigasi bawah, dan halaman dengan beberapa kolom (inbox, report) memakai pola daftar-lalu-detail.

---

## Tech Stack

| Bagian | Teknologi |
|---|---|
| Framework | [Next.js](https://nextjs.org) 16 (App Router) |
| UI | React 19, Tailwind CSS 4 |
| Bahasa | TypeScript 5 |
| Linting | ESLint 9 (`eslint-config-next`) |
| Backend | REST API Toentoen Ops (`https://api-ops.toentoen.com/api/v1`) |

> **Catatan untuk developer:** Next.js versi ini punya beberapa perubahan dari versi sebelumnya. Baca panduan di `node_modules/next/dist/docs/` sebelum mengubah konfigurasi atau memakai API Next.js yang baru (lihat juga `AGENTS.md`).

---

## Menjalankan di Lokal

### Prasyarat

- Node.js 20 atau lebih baru
- npm

### Langkah

```bash
# 1. Clone repository
git clone https://github.com/ToenToenProject/toentoenweb.git
cd toentoenweb

# 2. Install dependency
npm install

# 3. Siapkan environment variable
#    Buat file .env.local di root project (lihat bagian Environment Variables)

# 4. Jalankan development server
npm run dev
```

Buka [http://localhost:3000](http://localhost:3000) di browser.

### Scripts

| Perintah | Fungsi |
|---|---|
| `npm run dev` | Menjalankan development server |
| `npm run build` | Build untuk production |
| `npm run start` | Menjalankan hasil build production |
| `npm run lint` | Mengecek kode dengan ESLint |

---

## Environment Variables

Buat file `.env.local` di root project. File ini **tidak di-commit** (sudah ada di `.gitignore`).

| Variabel | Keterangan |
|---|---|
| `NEXT_PUBLIC_API_BASE_URL` | Base URL API Toentoen Ops |

Minta nilai yang benar ke tim backend. Jangan pernah commit token atau kredensial ke repository.

---

## Struktur Folder

```
src/
  app/                    Routing (App Router)
    login/                Login dan semua dashboard internal
      admin/supervisor/   Dashboard, inbox, report, kyc
      admin/staff/        Dashboard, inbox, report
      sales/              Sales staff dan supervisor
      cmo/                Dashboard CMO
      social-media/       Dashboard social media
    about/ news/ product/ team/ privacy-policy/ customer-service/
  components/             Komponen UI, dikelompokkan per fitur
    layout/               Header, Footer, sidebar dashboard
    admin/ sales/ cmo/ social-media/ dashboard/ home/ news/ ...
    ui/                   Komponen dasar (Button, Logo, dll.)
  constants/              Data statis dan konfigurasi
    api/admin/            Fungsi pemanggil API (login, report, kyc) dan tipe data
  services/               Service (mis. autentikasi)
public/                   Aset statis (gambar, ikon)
```

---

## Autentikasi

- Login memakai `workerId` dan `password` ke endpoint `/auth/login`.
- Token disimpan di `localStorage` dengan key `token` dan dikirim sebagai header `Authorization: Bearer <token>`.
- Tujuan setelah login ditentukan oleh role di `src/app/login/page.tsx`.

---

## Kontribusi

1. Buat branch baru dari `main`, misalnya `feature/nama-fitur` atau `fix/nama-bug`.
2. Pastikan `npm run lint` dan `npm run build` berjalan tanpa error.
3. Commit dengan pesan yang jelas, lalu buat Pull Request ke `main`.

---

## Lisensi

Project ini bersifat privat dan milik Toentoen. Dilarang menyalin atau mendistribusikan tanpa izin.
