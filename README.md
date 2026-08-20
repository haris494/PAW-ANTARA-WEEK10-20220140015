# PAW Pertemuan 10 — CS Bot API

Project ini merupakan tugas **Pertemuan 10 Pemrograman Aplikasi Web (PAW)** berupa REST API Customer Service otomatis berbasis **Generative AI** menggunakan **Express.js**, **Sequelize**, **MySQL**, dan **Gemini API**.

Bot dirancang untuk menjawab pertanyaan mengenai produk yang tersedia di database. Pertanyaan di luar konteks produk akan ditolak menggunakan **prompt engineering** dan **guardrail**.

## Fitur

* Login dan logout admin menggunakan session
* CRUD data produk
* Menampilkan daftar produk
* Integrasi Gemini API
* Customer Service Bot berbasis AI
* Jawaban berdasarkan data produk di database
* Guardrail untuk menolak pertanyaan di luar konteks
* Validasi input menggunakan middleware
* API key disimpan menggunakan `.env`
* Pengujian endpoint menggunakan Thunder Client

## Teknologi

* Node.js
* Express.js
* Sequelize
* MySQL
* Laragon
* Gemini API
* bcrypt
* express-session
* dotenv
* Thunder Client

## Struktur Folder

```text
PAW-ANTARA-WEEK10/
├── config/
│   ├── database.js
│   └── gemini.js
├── controllers/
│   ├── admin.controller.js
│   ├── chat.controller.js
│   └── product.controller.js
├── middlewares/
│   ├── auth.middleware.js
│   └── validateChatInput.middleware.js
├── models/
│   ├── admin.model.js
│   ├── index.js
│   └── product.model.js
├── routes/
│   ├── admin.routes.js
│   ├── chat.routes.js
│   └── product.routes.js
├── seeders/
│   └── seed.js
├── services/
│   └── gemini.service.js
├── utils/
│   └── response.js
├── screenshots/
├── app.js
├── Tugas.md
├── package.json
└── README.md
```

## Persiapan Database

Project ini menggunakan **MySQL melalui Laragon**.

Jalankan Laragon dan pastikan MySQL aktif pada port:

```text
3306
```

Buat database:

```sql
CREATE DATABASE cs_bot_db;
```

## Konfigurasi Environment

Buat file `.env` dan sesuaikan konfigurasi berikut:

```env
PORT=3000

DB_HOST=localhost
DB_PORT=3306
DB_NAME=cs_bot_db
DB_USER=root
DB_PASS=

SESSION_SECRET=ganti-dengan-secret-yang-random

GEMINI_API_KEY=API_KEY_GEMINI_KAMU

STORE_NAME=Toko Kita
```

> File `.env` tidak boleh di-commit ke GitHub karena berisi informasi sensitif seperti Gemini API Key.

## Instalasi

Install semua dependency:

```bash
npm install
```

Pastikan driver MySQL tersedia:

```bash
npm install mysql2
```

## Menjalankan Seeder

Jalankan:

```bash
npm run seed
```

Jika berhasil, terminal akan menampilkan hasil seperti:

```text
Koneksi database berhasil
Admin siap: admin
Produk dummy berhasil ditambahin

Seeding selesai ✅
Login admin pake:
username: admin | password: admin123
```

Seeder akan membuat akun admin dan data produk awal.

## Menjalankan Server

Jalankan:

```bash
npm run dev
```

Server berjalan pada:

```text
http://localhost:3000
```

## Endpoint API

### Admin

| Method | Endpoint            | Auth   | Keterangan   |
| ------ | ------------------- | ------ | ------------ |
| POST   | `/api/admin/login`  | Publik | Login admin  |
| POST   | `/api/admin/logout` | Admin  | Logout admin |

Contoh login:

```json
{
  "username": "admin",
  "password": "admin123"
}
```

### Product

| Method | Endpoint            | Auth   | Keterangan               |
| ------ | ------------------- | ------ | ------------------------ |
| GET    | `/api/products`     | Publik | Menampilkan semua produk |
| POST   | `/api/products`     | Admin  | Menambahkan produk       |
| PUT    | `/api/products/:id` | Admin  | Mengubah produk          |
| DELETE | `/api/products/:id` | Admin  | Menghapus produk         |

Contoh body tambah produk:

```json
{
  "name": "Nama Produk",
  "description": "Deskripsi produk",
  "price": 100000,
  "stock": 10
}
```

### Chat

Endpoint:

```text
POST /api/chat
```

Contoh pertanyaan mengenai produk:

```json
{
  "message": "kaos polos harganya berapa?"
}
```

Bot akan memberikan jawaban berdasarkan data produk yang tersedia di database.

## Guardrail

Bot dibatasi agar hanya menjawab pertanyaan yang berhubungan dengan produk.

Contoh pertanyaan di luar konteks:

```json
{
  "message": "buatkan saya kode HTML untuk landing page"
}
```

Bot akan menolak permintaan tersebut dan mengarahkan pengguna kembali ke topik produk.

Guardrail diterapkan melalui `services/gemini.service.js` dengan **system instruction** yang mengatur:

* Peran bot sebagai Customer Service
* Data produk yang boleh digunakan
* Larangan memberikan informasi di luar data produk
* Larangan membuat kode atau HTML
* Pencegahan prompt injection
* Grounding jawaban berdasarkan database

Selain guardrail pada prompt, validasi juga dilakukan melalui middleware sebagai penerapan **defense in depth**.

## Hasil Pengujian

Pengujian dilakukan menggunakan **Thunder Client**.

Fitur yang telah berhasil diuji:

* Login admin dengan status `200 OK`
* Menampilkan data produk
* Chat mengenai produk menggunakan Gemini
* Menampilkan harga dan stok berdasarkan database
* Guardrail menolak pertanyaan di luar konteks produk

Dokumentasi lengkap kontrak API dan screenshot hasil pengujian dapat dilihat pada:

```text
Tugas.md
```

## Kesimpulan

Project **CS Bot API** berhasil mengintegrasikan **Generative AI Gemini** dengan REST API menggunakan **Express.js**, **Sequelize**, dan **MySQL**.

Bot dapat memberikan informasi berdasarkan data produk yang tersedia di database serta menerapkan **prompt engineering**, **grounding**, **guardrail**, dan **defense in depth** agar jawaban tetap sesuai dengan konteks Customer Service.
