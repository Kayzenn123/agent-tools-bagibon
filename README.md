# BagiBon — Patungan Bill Makan Bareng ala Buku Arisan

**Live:** https://kayzenn123.github.io/agent-tools-bagibon/

Habis makan bareng, selalu ada drama "siapa bayar berapa" — apalagi kalau ada yang
cuma pesan es teh tapi ikut patungan rata. BagiBon menghitung patungan yang adil
**per item**: tiap makanan dibagi hanya ke yang memakannya, pajak/service dibagi
proporsional, dan yang bayar duluan ("nombok") langsung kelihatan berapa yang harus
dibalikin.

100% client-side. Tanpa akun, tanpa server, tanpa database.

## Cara pakai

1. **Tulis nama peserta** — minimal 2 orang. Atau pencet preset contoh
   ("Warung Kopi Sore", "Warteg Bahari", "Kafe Nongkrong" — semua harga contoh).
2. **Catat item ke bon** — nama, harga, lalu centang siapa saja yang makan item itu.
3. **Isi pajak/service** (manual, dalam %) dan pilih **yang bayar duluan** kalau ada.
4. **Hasil kehitung otomatis** — total bon (odometer), rincian per orang, dan box
   "nombok" untuk yang bayar duluan.
5. **Bagikan** — salin ringkasan WA, salin tautan bon (`?bon=...`), unduh/impor JSON,
   cetak struk, atau simpan ke riwayat (localStorage).

Easter egg: ketuk stempel **LUNAS** 5 kali.

## Fitur

- Hitung live otomatis (tanpa tombol hitung), validasi + pesan khas Bu Bendahara
- Deep-link `?bon=<base64url JSON>` — state penuh bisa dibagikan, tervalidasi
- Persistence `localStorage` key `bagibon:v1` (state terakhir + riwayat, skema berversi)
- Ekspor JSON, impor JSON (validasi skema), cetak struk (print CSS)
- Empty / loading / error states, keyboard-only bisa dipakai penuh
- Gerak A–E: zoom-through "PATUNGAN", 2 marquee diagonal + tombol jeda, teks path
  melengkung, spotlight cursor (desktop), curtain wipe antar section
- Toggle "Gerak: nyala/mati" + hormat `prefers-reduced-motion` + MODE RINGAN otomatis
  (CPU/RAM kecil atau Save-Data)

## Teknologi

- [AstroWind](https://github.com/arthelokyo/astrowind) (Astro 7 + Tailwind CSS v4),
  build statis — seluruh logika di satu file
  `src/components/bagibon/AppScript.astro` (vanilla JS, tanpa framework client)
- Font: Caveat (display), Jaldi (body), Playwrite CU (aksen) via Astro Fonts API
- Ikon: Phosphor Bold via `astro-icon` (`ph:*-bold`) + 4 SVG custom digambar tangan
  (buku arisan, stempel LUNAS textPath, uang terbelah, perforasi)
- Palet risograph triadic hue 318 (token OKLCH, tanpa shadow & gradient)

## Pengembangan

```bash
npm install
npm run dev      # http://localhost:4321
npm run build    # output ke dist/
```

## QA

- `node ~/agent-lab/tools/qa.js <URL>` — exit 0 (tanpa overflow, tanpa console
  error, alt/label lengkap, tepat 1 h1, FPS 4x ≥ 45)
- Terakhir diukur: FPS 60.3 (CPU throttle 4x), reduced-motion 0 animasi berjalan
- Lighthouse: tidak bisa diukur di VM ini (Chrome butuh egress; lihat CRITIQUE.md) —
  sebagai gantinya: cek manual setara (semantic HTML, alt, label, kontras AA,
  tap target, meta/OG)

## Struktur

```
src/
  pages/index.astro                 # rakitan halaman
  components/bagibon/
    bagibon.css                     # token + seluruh visual
    AppScript.astro                 # logika aplikasi (state, hitung, gerak)
    TimelineApp.astro               # 5 section timeline buku
    MarqueeBands.astro              # Gerak B
    StampLunas.astro                # stempel SVG + easter egg
    BukuIcon.astro / UangBelahIcon.astro / Perforasi.astro
public/og-image.png                 # Open Graph 1200x630
```

## Lisensi

MIT — silakan pakai buat patungan beneran.
