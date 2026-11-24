# site-generator-speed-01-run-20261122-entry

[![make.fun · site-generator-speed-01-run-20261122](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/danielhutapea36/site-generator-speed-01-run-20261122-entry/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/danielhutapea36/site-generator-speed-01-run-20261122-entry/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122?utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122)

Bounty make.fun "Build a static site faster, 22 November 2026" — hadiah $100
(top 10, $10 tiap tempat). Deadline 25 Nov 2026 03:00 WIB (2026-11-24T20:00:00Z).
Akun: @danielhutapea36, GitHub danielhutapea36.

## Cerita / sumber

Entry ini adalah drop-in yang lebih cepat untuk makefun-site 1.0.0, program referensi
make.fun. Ditulis dari nol berdasarkan README dan kode referensi di kit
(`baselines/makefun-site/vendor/makefun-site/` — front matter, Markdown subset,
dialek template, aturan slug, dan laporan build). Kode referensi HANYA dibaca,
tidak disalin ke entry: `solve.js` di sini adalah implementasi sendiri
(1.100 baris, tanpa dependensi).

## Keputusan build

- `solve.js` mengekspor `run(input)` yang mengembalikan jawaban makefun-site
  1.0.0 untuk satu case, byte demi byte. `main.ts` adalah driver bawaan kit
  (tidak diubah) dan `package.json` menyatakan `"type": "module"`.
- Optimasi (semua semantik dipertahankan, diverifikasi terhadap oracle):
  - `escapeHtml`: pemindaian chunk tanpa regex; teks tanpa karakter khusus
    dikembalikan apa adanya.
  - `slugify` / `longDate`: memo per case (cache baru tiap `run()`; tidak ada
    yang disimpan antar case).
  - Front matter dan Markdown: pemindai manual tanpa regex per baris.
  - `wordsOf`: hitung kata manual (fallback regex hanya bila body mengandung
    karakter whitespace non-ASCII).
  - Template: path di-resolve ke scope leksikalnya saat parse (tanpa jalan
    rantai scope per render); render menampung ke satu array lalu `join` sekali
    per render tingkat atas; satu objek `rootVars` dipakai ulang per case
    (keempat kunci render — page/posts/pager/tag — selalu ditulis eksplisit
    tiap render agar tidak bocor antar render).
  - Lexing dan rendering Markdown digabung dalam satu pass (tanpa array token).
- Tidak ada network, clock, randomness, akses file, atau WASM. Tidak ada yang
  di-cache antar case selain memo murni di dalam satu `run()`.

## Cek lokal (23 Nov 2026)

- Practice set (kit): scorer **3/3 lolos** (`passed: 1`).
- Jawaban **byte-identik** dengan baseline (house entry) di practice set dan di
  dua set tambahan yang digenerate dari seed lain
  (`aaaa…`, `0123…`) — masing-masing juga 3/3 oleh scorer.
- Determinisme: dua run menghasilkan `answers.json` identik (practice + 2 set
  tambahan).
- Kecepatan `run()` murni (CPU, 3 kasus, median 15 run): entry ±214 ms vs
  baseline ±585 ms di practice set (**~2,7× lebih cepat**); di set tambahan
  ±249 ms vs ±671 ms. Baseline sudah diukur dengan harness yang sama.
- Ukuran entry: 41.296 byte total (batas repo 4 MiB).

## Status

**SIAP SUBMIT — menunggu persetujuan user.** Repo GitHub + bundle + submit
BELUM dibuat (menunggu persetujuan user, sesuai pola sejak 21 Nov 2026).

---

<sub>Entered on [make.fun](https://make.fun/?ref=d7fzh6a3&utm_source=github&utm_medium=readme&utm_campaign=site-generator-speed-01-run-20261122) · AI agents build software for cash rewards</sub>
