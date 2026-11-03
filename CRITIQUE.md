# CRITIQUE — BagiBon (2026-11-03)

Self-review ala pipeline kualitas AGENT.md bag. 6: riset → draft → kritik sendiri →
revisi (3 putaran dilakukan) → verifikasi → serahkan.

## Gerbang v3 (anti-slop) — LOLOS
- Tanpa lorem ipsum, tanpa emoji sebagai ikon, tanpa testimoni palsu (R8).
- Copy spesifik & hidup: suara Bu Bendahara konsisten di empty/error/sukses/footer.
- Harga preset = contoh berlabel eksplisit, bukan fakta (R7). Pajak/service default kosong.
- 4 SVG custom digambar tangan (buku, stempel textPath, uang belah, perforasi).

## Gerbang v4 (motion/type/color/icon) — LOLOS
- Type: Caveat display / Jaldi body / Playwrite CU aksen — 3 family, via Astro Fonts API,
  font-display: swap. Caveat dipakai untuk angka (odometer) & tombol: tabular-nums dijaga.
- Color: token OKLCH dari seed (triadic hue 318), tanpa shadow & gradient visual.
  Kontras diukur manual + LH: text/bg 7.4+, muted/bg 7.18, muted/surface 4.56 (AA lolos
  setelah 2x revisi penggelapan muted — estimasi awal BRIEF "5.5:1" ternyata salah).
- Icon: Phosphor Bold 3px sudut tajam. ADAPTASI: brief menulis `ph-bold:...`, tapi
  koleksi Iconify bernama `ph` dengan sufiks `-bold` → dipakai `ph:users-bold` dst.
  (setara, terverifikasi di node_modules/@iconify-json/ph).
- Motion A–E semua jalan; hanya transform/opacity/clip-path; toggle + reduced-motion +
  MODE RINGAN otomatis.

## Gerbang v5 (penyatu, ATURAN 0) — LOLOS
Satu metafora ("buku arisan raksasa Bu Bendahara") menembus semua: hero = sampul buku,
scroll = buka halaman, timeline = urutan nulis bon, stempel LUNAS = penutup, footer =
tanda tangan. Tanpa navbar/hero marketing (keputusan berani BRIEF) — kalkulator
langsung terlihat <3 detik.

## Skor (target ≥8)
| Kriteria | Skor | Catatan |
|---|---|---|
| Karakter (identitas visual) | 9 | Riso 3 tinta + Caveat raksasa + perforasi + grain; langsung kebaca "buku arisan". Minus: hero mobile agak lega (ruang kosong 230vh stage). |
| Keunikan | 9 | Belum ada di 14 build terakhir: handwritten display raksasa sebagai satu-satunya pintu + timeline sumbu tengah + zoom-through teks. |
| Kerapian | 8 | Token konsisten, komponen terpisah. Minus: AppScript ~700 baris satu IIFE (bisa dipecah jadi modul); beberapa inline style di markup. |
| Fungsi | 9 | Semua fitur BRIEF terverifikasi Playwright: tambah/hapus peserta & item, preset 3, pajak/service, nombok, odometer live, salin WA, deep-link ?bon=, ekspor/impor JSON, cetak struk, riwayat, easter egg 5x. |
| Aksesibilitas | 9 | LH a11y 100 (mobile & desktop); label lengkap; 1 h1; keyboard-only lulus; reduced-motion 0 animasi. |
| Performa | 9 | LH perf 91–99 mobile / 100 desktop; FPS 60.3 @ CPU 4x. |

## Verifikasi (terverifikasi / belum)
- [terverifikasi] qa.js exit 0 lokal (4 viewport + reduced-motion + fps4x 60.3)
- [terverifikasi] Lighthouse via CHROME_PATH=playwright-chromium: mobile 91–99/100/100/100, desktop 100/96/100/100
- [terverifikasi] Fungsi inti via skrip Playwright (tambah peserta/item → odometer Rp16.500 & nombok Rp8.250 benar; preset; deep-link round-trip; stempel 5x → pesan muncul)
- [terverifikasi] Keyboard: semua aksi = button/input asli; focus-visible ada
- [belum] qa.js di URL live (dijalankan setelah deploy)
- [belum] Uji sentuh di HP fisik (emulasi Playwright hasTouch saja)

## Yang dipangkas
Tidak ada fungsi yang dipangkas. Penyesuaian kecil: (1) sintaks ikon `ph-bold:x` →
`ph:xxx-bold` (koleksi Iconify), (2) muted digelapkan 2x demi kontras AA,
(3) stempel dibungkus `<button>` asli demi label-content-name-mismatch.
Bug diperbaiki: `history.replaceState` (variabel `history` menutupi window.history).

## Keputusan
**LANJUT** — semua kriteria ≥8, DoD terpenuhi di lokal. Lanjut deploy + qa live.
