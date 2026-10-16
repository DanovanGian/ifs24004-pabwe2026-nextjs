# Linimasa — Aplikasi Postingan

Aplikasi linimasa (timeline) untuk membuat dan melihat postingan, dibangun dengan **Next.js (App Router)** dan **TypeScript**. Data diambil dari [Delcom Open API (Posts)](https://open-api.delcom.org/docs/1.0/api-posts).

Proyek ini adalah bagian dari **Studi Kasus 2.2 — Praktikum PABWE 2026**.

## Teknologi

| Bagian                      | Yang dipakai                                         |
| --------------------------- | ---------------------------------------------------- |
| Runtime dan package manager | [Bun](https://bun.sh)                                |
| Framework                   | Next.js (App Router, Turbopack)                      |
| Bahasa                      | TypeScript                                           |
| State management            | Redux Toolkit                                        |
| Styling                     | Tailwind CSS v4, font Plus Jakarta Sans (Google Fonts) |
| Ikon                        | `react-icons` (set Tabler)                           |
| Dialog                      | SweetAlert2                                          |
| Pengujian                   | Vitest dengan coverage v8                            |
| CI dan kualitas kode        | Jenkins (`Jenkinsfile`) dan SonarQube (`sonar-project.properties`) |

## Fitur

- **Autentikasi**: registrasi dan login pengguna.
- **Postingan**: melihat daftar postingan di linimasa dan membuka halaman detail postingan.
- **Pengguna**: halaman daftar pengguna.
- **Profil**: halaman profil dan pengaturan akun, termasuk ubah kata sandi.

## Prasyarat

- [Bun](https://bun.sh) sudah terpasang. Cek dengan `bun --version`.

## Memulai

```bash
# 1. Pasang dependensi
bun install

# 2. Salin contoh berkas lingkungan, lalu sesuaikan nilainya
cp .env.example .env

# 3. Jalankan server pengembangan
bun run dev
```

Aplikasi berjalan di port yang ditentukan oleh `APP_PORT`.

## Variabel Lingkungan

| Variabel                     | Fungsi                                      |
| ---------------------------- | ------------------------------------------- |
| `NEXT_PUBLIC_DELCOM_BASEURL` | URL dasar Delcom Open API                   |
| `APP_PORT`                   | Port untuk server pengembangan dan produksi |

Gunakan `.env.example` sebagai acuan format. Jangan commit berkas `.env` yang berisi nilai rahasia.

## Perintah

| Perintah                          | Fungsi                                                          |
| --------------------------------- | --------------------------------------------------------------- |
| `bun run dev`                     | Menjalankan server pengembangan lewat `src/server.ts`           |
| `bun run build`                   | Build untuk produksi                                            |
| `bun run start`                   | Menjalankan hasil build                                         |
| `bun run lint`                    | Memeriksa kode dengan ESLint                                    |
| `bun run test`                    | Menjalankan tes Vitest beserta coverage (ambang batas 100%)     |
| `bunx tsc -p tsconfig.test.json`  | Memeriksa tipe pada berkas tes                                  |

## Rute

| Rute               | Halaman                |
| ------------------ | ---------------------- |
| `/auth/*`          | Login dan registrasi   |
| `/`                | Linimasa postingan     |
| `/posts/[postId]`  | Detail postingan       |
| `/users`           | Daftar pengguna        |
| `/profile`         | Profil dan akun        |

Halaman selain `/auth/*` berada dalam grup rute `(dashboard)`.

## Struktur Proyek

```
src/
├── server.ts          # Launcher; membaca APP_PORT dari env, .env, atau .env.example
├── store.ts           # Konfigurasi Redux store
├── app/               # Rute App Router: auth/* dan grup (dashboard)
├── components/        # Komponen bersama (Providers, Avatar, Modal)
├── helpers/           # apiHelper, toolsHelper
├── hooks/             # useInput dan hook Redux
├── lib/               # Konfigurasi
├── types/             # Definisi tipe TypeScript
└── features/
    ├── auth/          # api, states, pages, dst.
    ├── users/
    └── posts/
```

Setiap modul di `features/` memakai pola yang sama:

- `api/`: fungsi pemanggil endpoint.
- `states/`: action types, action creators, thunk, dan reducer.
- `pages/`: komponen halaman untuk fitur tersebut.

## Pengujian

Tes dijalankan dengan Vitest dan coverage v8, dengan **ambang batas 100%**.

```bash
bun run test
```

Berkas yang dikecualikan dari coverage:

- `src/server.ts`
- `src/app/**` (hanya pembungkus rute)
- `src/types/**`
- Berkas pendukung pengujian

Berkas tes tidak ikut dalam pemeriksaan tipe saat build. Untuk memeriksanya, jalankan `bunx tsc -p tsconfig.test.json`.

## Catatan

- Endpoint ubah kata sandi mengikuti dokumentasi Delcom, yaitu `PUT /users/password`. Soal praktikum menulis `/users/me/password`, jadi jika server berbeda, ubah satu konstanta di `userApi.ts`.
- Respons login diasumsikan berisi `data.token`, karena halaman dokumentasi Auth tidak dapat diakses saat aplikasi dibuat.

## Pembuat

- **Nama**: Gian Danovan
- **NIM**: ifs24004
- **Mata kuliah**: Pengembangan Aplikasi Web (PABWE) 2026