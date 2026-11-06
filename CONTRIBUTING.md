# Contributing Guide

Dokumen ini adalah standar penggunaan Git untuk proyek **jembergo**
(stack: **Laravel (PHP) + MySQL**). Semua anggota
tim wajib mengikutinya agar riwayat repositori rapi, mudah ditelusuri, dan siap
untuk CI/CD.

## 📌 Branch Convention

Format: `<type>/<nomor-tiket>-<deskripsi-singkat>`

| Type | Kegunaan | Contoh |
|------|----------|--------|
| `feature/` | Fitur baru | `feature/123-user-login` |
| `fix/` | Perbaikan bug | `fix/cart-total` |
| `hotfix/` | Perbaikan darurat di production | `hotfix/payment-down` |
| `refactor/` | Merapikan kode tanpa ubah perilaku | `refactor/order-service` |
| `docs/` | Dokumentasi | `docs/api-reference` |
| `test/` | Menambah/memperbaiki test | `test/checkout-flow` |
| `chore/` | Maintenance (dependency, config) | `chore/update-deps` |

**Aturan:**
- Huruf kecil semua
- Pakai hyphen (`-`), bukan spasi atau underscore
- Sertakan nomor tiket bila ada
- Maksimal 50 karakter
- Selalu dibuat dari `main` yang terbaru

| ✅ Benar | ❌ Salah | Alasan |
|---------|---------|--------|
| `feature/201-product-search` | `Feature/Product_Search` | Huruf besar & underscore |
| `fix/login-redirect` | `fix login bug` | Mengandung spasi, tanpa prefix `/` |
| `docs/api-reference` | `budi-branch` | Tidak menjelaskan tujuan |

## 📝 Commit Convention

Format: `<type>(<scope>): <subject>` (mengikuti [Conventional Commits](https://www.conventionalcommits.org/))

**Type:** `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`, `revert`

**Scope:** bagian proyek yang diubah, mis. `auth`, `user`, `migration`, `routes`, `view`, `api`, `readme`.

**Aturan:**
- Subject huruf kecil, *imperative mood* ("add", bukan "added"), tanpa titik di akhir
- Maksimal 50 karakter untuk subject (batas keras `commitlint`: 72 karakter header)
- Body dipisah satu baris kosong dari subject, jelaskan *mengapa*, bukan hanya *apa*
- Referensikan issue di footer: `Closes #123`
- Satu commit = satu perubahan logis (commit atomik)

| ✅ Benar | ❌ Salah |
|---------|---------|
| `feat(auth): add JWT refresh token` | `fix bug login` |
| `fix(cart): correct total when discount applied` | `Update README` |
| `docs(readme): update installation steps` | `wip` |
| `refactor(cart): simplify price calculation logic` | `Fixed the thing.` |

Validasi otomatis dilakukan oleh **commitlint + Husky**; commit yang tidak sesuai akan ditolak.

## 🔀 Pull Request

- Judul PR mengikuti format commit message, mis. `feat(web): add hello world page`
- Ukuran PR kecil (< 400 baris perubahan)
- Isi deskripsi memakai template: Deskripsi, Perubahan, Tipe Perubahan, Checklist, Referensi
- Wajib minimal **1 approval** dari reviewer
- Wajib **lolos CI** (build, lint, test)
- Gunakan **Squash and merge** agar riwayat `main` bersih
- Hapus branch setelah merge

## 👀 Code Review

Gunakan label pada komentar:

| Label | Arti |
|-------|------|
| `blocking:` | Harus diperbaiki sebelum merge |
| `nit:` | Saran kecil, opsional |
| `question:` | Butuh klarifikasi dari penulis |
| `praise:` | Apresiasi untuk kode yang bagus |

Contoh: `blocking: query ini rentan SQL injection, gunakan prepared statement.`

Reviewer menanggapi maksimal 1 hari kerja. Kritik ditujukan pada kode, bukan orangnya.

## ⚙️ Workflow

1. `git checkout main && git pull origin main`
2. `git checkout -b feature/123-nama-fitur`
3. Kerjakan, lalu commit secara atomik (`git commit -m "feat(scope): ..."`)
4. `git push -u origin feature/123-nama-fitur`
5. Buat Pull Request di GitHub
6. Tunggu review & CI hijau
7. Squash and merge
8. Hapus branch (remote & lokal):
   `git checkout main && git pull && git branch -d feature/123-nama-fitur`

Branch hidup sesingkat mungkin (idealnya < 2 hari) dan sering di-sinkronkan dengan
`main` untuk menghindari konflik besar.

## 🚫 Yang Dilarang

- Push langsung ke `main`
- Commit message tidak jelas (`wip`, `test`, `asdf`, `update`)
- Commit file `.env`, folder `vendor/`, `node_modules/`, atau credential (gunakan `.gitignore`)
- Mengubah file migration yang sudah di-merge ke `main` (buat migration baru)
- Rebase / force-push pada branch yang sudah di-push dan dipakai orang lain
- Merge PR tanpa review dan CI lulus

## 🧩 Aturan Khusus Laravel

- **Migration:** satu perubahan skema = satu migration baru. Jangan edit migration yang sudah di-merge.
  Contoh commit: `feat(migration): add bookings table`
- **Environment:** `.env` tidak boleh di-commit. Setiap variabel baru wajib ditambahkan ke `.env.example`.
- **Dependency:** perubahan `composer.json` harus disertai `composer.lock`. Commit dengan type `build(deps)`
  atau `chore(deps)`, mis. `chore(deps): update laravel framework`.
- **Test:** jalankan `php artisan test` sebelum membuat PR.
- **Gaya kode:** ikuti PSR-12; jalankan `./vendor/bin/pint` sebelum commit jika Laravel Pint terpasang.

## 🛠️ Setup Lokal

```bash
git clone <url-repo>
cd jembergo
composer install
cp .env.example .env
php artisan key:generate
npm install          # otomatis mengaktifkan hook Husky (commitlint)
```
