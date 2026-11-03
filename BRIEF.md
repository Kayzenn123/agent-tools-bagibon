# BRIEF — BagiBon — 2026-11-03

## Masalah/usecase
Habis makan bareng, selalu ada drama "siapa bayar berapa" — apalagi kalau ada yang cuma pesan es teh tapi ikut patungan rata. BagiBon: patungan bill makan bareng yang adil per item, 100% client-side, tanpa akun, tanpa server.

## Konsep visual (1 kalimat)
Situs = 1 buku arisan raksasa milik "Bu Bendahara": pengunjung langsung megang buku patungan raksasa bertinta risograph (ungu-kertas + tinta emas + tinta biru), judul tulisan tangan "PATUNGAN" jadi pintu zoom-through ke kalkulator.

## Hasil seed (builds/2026-11-03/seed.json)
- domain: arisan | layout: timeline satu sumbu | reference: risograph 2-3 warna | constraint: tanpa shadow dan gradient
- fonts: display Caveat / body Jaldi / aksen Playwrite CU
- palet: mid/triadic hue 318 — bg oklch(0.72 0.146 318) kertas mauve, surface oklch(0.60 0.110 318), text oklch(0.16 0.030 318), muted oklch(0.32 0.040 318), accent oklch(0.73 0.259 78) emas, accent2 oklch(0.71 0.227 198) biru, pop oklch(0.79 0.213 120) hijau stempel
- motion: A zoom-through teks jadi pintu | B marquee diagonal | C teks di path melengkung | D cursor senter/spotlight mask | E curtain wipe antar section
- ikon: outline tebal 3px sudut tajam, Phosphor (bold)
- RE-SEED (ATURAN 19 v5): seed asli 2026-11-03 duplikat 5/5 sumbu dengan KarcisParkir 2026-10-21 (domain/layout/font/palet/kamera identik). Diganti base 2026-12-07 SEBELUM tulis kode; lolos cek anti-seragam vs 14 build terakhir.

## 3 arah desain + arah terpilih
1. "Struk kasir raksasa": struk thermal vertikal raksasa, font mono. Terlalu mirip POSKILAT (10-25? no — 11-02) & WartegKaset.
2. "Papan tulis warteg": kapur putih di papan hijau. Terlalu mirip HitungHPP (tiang ukur, 10-30).
3. TERPILIH — "Buku arisan raksasa": buku tulis tangan raksasa, tinta 3 warna riso, garis tepi perforasi. JAUH dari 14 build terakhir: belum ada yang pakai handwritten display raksasa sebagai satu-satunya navigasi + timeline sumbu tengah.

## Keputusan berani
Tanpa navbar, tanpa hero marketing, tanpa tombol "mulai". Pengunjung mendarat di buku kosong bertuliskan tangan "tulis namanya dulu, jangan asal" — kalkulatornya LANGSUNG di depan mata dalam 3 detik pertama. Judul "PATUNGAN" outline raksasa = pintu zoom-through ke bawah.

## Persona/suara
Bu Bendahara — bendahara arisan RT yang tegas tapi sayang: "tulis namanya dulu, jangan asal", "harganya kok kosong, Bu? isi dulu gih", "udah lunas semua, alhamdulillah". Konsisten sampai error state, empty state, dan footer.

## Palet (harmoni, kenapa, psikologi, token, kontras)
- Harmoni triadic hue 318: ungu kertas (dominan 60%), tinta emas 78 (pendukung 30%), tinta biru 198 (aksen 10%), hijau 120 hanya untuk stempel "LUNAS" (<5%).
- Psikologi: kertas ungu-mauve hangat seperti buku tulis lama; 3 tinta riso = kesan cetakan gotong-royong, tegang tapi akrab.
- Token CSS: --bg --surface --text --muted --accent --accent-2 --pop dari nilai OKLCH seed.
- Kontras AA: text (L 0.16) di atas bg (L 0.72) ≈ 9:1 ✓; muted (L 0.32) di atas bg ≈ 5.5:1 ✓. Tinta emas/biru HANYA untuk teks besar (≥24px/bold) atau grafis, bukan body. Verifikasi ulang dengan axe/playwright di QA.

## Font (display/body/aksen, alasan, id Fontsource, variable?)
- Display: Caveat (tulisan tangan Bu Bendahara) — id fontsource: caveat. Variable? Caveat punya variable weight; kalau flag variable ada, animasikan wght halus di hover judul. Kalau tidak, skip.
- Body: Jaldi (sans bersih, terbaca di 390px) — id: jaldi. (hasil --reroll body; Lexend Zetta terlalu lebar untuk body)
- Aksen: Playwrite CU (coretan stempel/catatan pinggir) — id: playwrite-cu.
- Maks 3 family, ≤4 file weight, Astro Fonts API di astro.config.ts, font-display: swap.

## Ikon (gaya, sumber, daftar 12+, 3+ custom)
- Gaya: outline tebal 3px, sudut tajam. Sumber: Phosphor Bold via astro-icon (`ph-bold:...`).
- Daftar (12+): users, user-plus, x, receipt, percent, wallet, copy, link-simple, download-simple, upload-simple, printer, trash, check, arrow-right, calculator, coins, hand-coins, stamp, banknote, equals.
- Custom SVG (3+, digambar sendiri): (1) buku arisan terbuka, (2) stempel bulat "LUNAS" (teks di path melingkar), (3) uang kertas terbelah dua (split), (4) divider perforasi (lubang-lubang).
- Dilarang emoji sebagai ikon.

## Gerak (kategori A-F, narasi, MODE RINGAN)
Narasi: "membuka buku arisan & menghitung bareng" — scroll = membalik halaman buku.
- A (kamera, WAJIB): judul "PATUNGAN" outline raksasa (20vw) jadi pintu: scroll → zoom-through ke section kalkulator.
- B (marquee, WAJIB 2 band): 2 pita diagonal berlawanan arah: band 1 angka-angka patungan ("Rp12.500 • Rp8.000 •"), band 2 kata ("adil • patungan • lunas •"); skew mengikuti velocity scroll; tombol jeda (aria).
- C (teks): headline section di path SVG melengkung (mis. "siapa ikut makan?"); outline text terisi (fill) saat scroll masuk.
- D (input): spotlight cursor — radial mask mengikuti pointer di desktop (sembunyi di touch); tilt 3D ringan pada kartu hasil (pointer, bukan gyro wajib).
- E (reveal): curtain wipe antar section timeline.
- F: tidak dipakai (cukup 5 lapisan; jaga performa).
- MODE RINGAN (hardwareConcurrency ≤4 / deviceMemory ≤4 / Save-Data / FPS <40 2 detik): matikan spotlight + tilt, sederhanakan marquee (1 band statis), matikan zoom-through (layout statis).
- Semua gerak: transform/opacity/clip-path saja; hormati prefers-reduced-motion + toggle UI "Gerak: nyala/mati".

## Storyboard hero moment
| Scroll | Yang terlihat | Kamera | Marquee | Teks | Interaksi |
|---|---|---|---|---|---|
| 0% | Buku arisan raksasa kosong + "PATUNGAN" outline 20vw + coretan "tulis namanya dulu, jangan asal" | — | 2 pita diagonal jalan | judul outline | spotlight cursor |
| 25% | Zoom-through judul → section 1: daftar peserta di sumbu timeline | zoom 1→1.6 melalui huruf | pita lanjut | "siapa yang makan bareng?" (path melengkung, terisi saat scroll) | tambah peserta |
| 50% | Section 2: item + pilih pemakan per item (checkbox) | pan halus | — | — | tambah item, centang pemakan |
| 75% | Section 3: pajak/service + "yang bayar duluan" → Section 4: kartu hasil per orang + odometer total | — | — | angka odometer naik | hitung otomatis live |
| 100% | Section 5: salin WA / deep-link / ekspor / cetak + stempel "LUNAS" | — | pita lagi | — | salin, unduh, cetak |
Pertunjukan utama: zoom-through "PATUNGAN" + odometer total yang naik-turun live saat item diubah. 5 detik pertama: buku kosong + perintah Bu Bendahara.

## Konten (15+ item nyata, easter egg)
- Preset contoh 1 "Warung Kopi Sore" (4 orang: Budi, Sari, Andi, Maya): kopi susu gula aren 15rb, es teh manis 8rb, indomie goreng + telur 13rb, pisang goreng 10rb, mendoan 8rb.
- Preset contoh 2 "Warteg Bahari" (3 orang): nasi + ayam geprek 15rb, nasi + telur balado 12rb, es jeruk 7rb, kerupuk 2rb, sayur sop 8rb.
- Preset contoh 3 "Kafe Nongkrong" (5 orang): matcha latte 28rb, croffle 22rb, kentang goreng 18rb, air mineral 5rb, red velvet 25rb.
- Semua harga = contoh yang jelas dilabeli (bukan fakta). Pajak/service default KOSONG (isi manual, placeholder "cth: 10").
- Pesan Bu Bendahara: empty peserta "belum ada yang nulis nama, gimana mau patungan?", error harga "harganya kok kosong, Bu? isi dulu gih", sukses "udah lunas semua, alhamdulillah".
- Easter egg: ketuk stempel LUNAS 5× → muncul coretan Playwrite CU "ditunggu traktirannya 😏" — TANPA emoji, pakai teks saja: "ditunggu traktirannya!".

## Teknik visual orisinal (2+)
1. Risograph misregistration: judul display di-layer ganda (teks emas offset 3px di belakang teks ungu, tanpa shadow — duplikat glyph, bukan text-shadow).
2. Divider perforasi SVG (lubang-lubang) antar section timeline + noise grain via feTurbulence (constraint mewajibkan grain).
3. Stempel "LUNAS" berputar lambat (SVG textPath melingkar), tinta pop hijau.

## Suara
Tidak (bonus dilewati; fokus fungsi).

## Fungsi (v2 FASE 1 — backend beneran)
- Semua fitur jalan client-side, tanpa server.
- Persistence: localStorage key `bagibon:v1` (riwayat patungan + state terakhir); validasi skema + try/catch; data rusak → fallback default + pesan.
- Deep-link: `?bon=<base64url JSON>` — state penuh bisa dibagikan; parse tervalidasi (tipe/panjang), gagal → pesan Bu Bendahara.
- Ekspor: salin ringkasan WA (clipboard + fallback textarea), unduh JSON, cetak struk (print CSS).
- Impor JSON (file) dengan validasi skema.
- Hitung live otomatis (tanpa tombol hitung).
- Validasi: nama kosong, harga ≤0/bukan angka, peserta <2 → pesan spesifik.
- Empty state (belum ada peserta/item), loading state (saat impor/decode), error state — didesain.
- Tombol utama ("Salin ringkasan WA") terlihat ≤3 detik pertama scroll ke section hasil.

## QA & DoD
- tools/qa.js exit 0 di lokal & URL live (chromium); screenshot 390px → ~/agent-lab/proof/bagibon-mobile-390.png
- Lighthouse mobile+desktop ≥90 (performance/accessibility/best-practices/SEO); catat di README. Catatan: Chrome mungkin tak bisa launch di VM (no-egress) — kalau gagal, catat + cek manual setara.
- FPS ≥45 di CPU throttle 4x (qa.js).
- Keyboard-only lulus; reduced-motion lulus; MODE RINGAN lulus.
- README.md, CRITIQUE.md, seed.json, screenshots, og-image 1200x630.
- Repo: Kayzenn123/agent-tools-bagibon (branch main + gh-pages dengan .nojekyll).
- Deploy: GitHub Pages → https://kayzenn123.github.io/agent-tools-bagibon/
- LOG.md: tambah 1 baris (tanggal, domain, layout, font, palet, gerak, ikon, keputusan berani, skor).
- agent.log: 1 baris sukses. Telegram: HANYA jika gagal.
