# SPEC.md — Repeatable Landing Page & Catalog

> Brief ini untuk dieksekusi oleh Claude Code. Ganti "Repeatable" dengan nama final kalau sudah diputuskan (find & replace).

---

## 1. Ringkasan Proyek

**Apa ini:** Situs statis (bukan aplikasi berlogin) yang berfungsi sebagai katalog template Notion/Airtable/Sheets + blueprint n8n, dengan demo interaktif dan jalur gratis-ke-berbayar.

**Tujuan ganda:**
1. Portofolio digital marketing (traffic, funnel conversion, SEO/GEO citation — semua harus terukur)
2. Revenue sekunder dari penjualan template (checkout diarahkan ke Gumroad/Lemon Squeezy — situs ini TIDAK memproses pembayaran)

**Audiens:** Usaha mikro/solopreneur (fotografer, host Airbnb, F&B kecil, kreator konten) yang ingin mengganti software SaaS berlangganan dengan sistem sekali-bayar.

**Brand voice:** Faceless/anonim. Pakai "kami", nada percaya diri dan langsung ke inti, klaim berbasis angka — bukan hype.

---

## 2. Tech Stack & Batasan

- **Tanpa backend.** Semua logika dinamis didelegasikan ke pihak ketiga:
  - Form/email capture → embed MailerLite (HTML snippet, client-side)
  - Demo/trial → iframe embed dari halaman publik Notion / shared view Airtable
  - Checkout → tombol link keluar ke Gumroad/Lemon Squeezy
- **Struktur:** HTML/CSS/vanilla JS statis (atau Astro kalau butuh templating multi-halaman) — **hindari framework client-side-render berat** (React SPA penuh) karena menghambat crawler AI untuk GEO.
- **Hosting:** Cloudflare Pages (free tier, commercial use diizinkan, unlimited bandwidth).
- **Domain:** custom domain terpisah (dibeli di registrar), diarahkan ke Cloudflare Pages. Root domain langsung (`repeatable.com` / `.co`), TANPA prefix `app.`.
- **Wajib setelah deploy:** cek dashboard Cloudflare bahwa AI crawler (GPTBot, ClaudeBot, PerplexityBot) TIDAK diblokir — Cloudflare defaultnya kadang memblokir ini.

---

## 3. Struktur File

```
/
├── index.html                  → Homepage
├── /t/
│   ├── template-slug-1.html    → Satu halaman per template (SEO landing page)
│   └── template-slug-2.html
├── /thanks.html                → Setelah submit email
├── /license.html                → Ketentuan penggunaan
├── /assets/
│   ├── /css/style.css
│   ├── /js/main.js
│   ├── /img/                   → Screenshot/GIF demo, favicon
│   └── /fonts/                 → Self-host font (jangan load dari Google Fonts CDN, lebih cepat & privacy-friendly)
├── robots.txt                  → WAJIB eksplisit allow AI crawler
├── sitemap.xml
└── /schema/                     → Referensi JSON-LD per halaman (lihat bagian 7)
```

---

## 4. Arah Desain — WAJIB DIBACA SEBELUM CODING

### 4.1 Yang harus DIHINDARI (ciri khas "AI slop" design)

Tiga pola default yang HARUS dihindari karena terlalu sering muncul di desain buatan AI dan langsung ketahuan generik:
1. Background krem hangat (~#F4F1EA) + serif display kontras tinggi + aksen terracotta/clay (~#D97757)
2. Background nyaris hitam + satu aksen hijau-neon atau vermilion terang
3. Layout ala broadsheet koran — hairline rules, border-radius nol, kolom rapat ala surat kabar

Juga hindari: hero generik "judul besar + subjudul + 2 tombol + gradient blob abstrak di belakang", grid fitur 3-kolom ikon-judul-deskripsi tanpa konteks nyata, testimonial carousel generik.

### 4.2 Konsep desain yang dipakai: "Carbon Copy"

Terinspirasi dari **buku nota rangkap (duplicate/NCR form)** yang secara harfiah dipakai puluhan tahun oleh usaha kecil — tukang, bengkel, toko — sebelum ada software: lembar putih (asli), kuning (duplikat), merah muda (rangkap tiga). Ini konsep yang ownable karena terhubung langsung ke nama brand ("Repeatable"/"Duplicated") DAN ke dunia nyata target audiens (mereka pernah/masih pakai nota rangkap fisik).

**Referensi desain nyata untuk kalibrasi kualitas** (bukan untuk ditiru pixel-nya, tapi pelajari prinsip di baliknya):
- **Linear.app** — pelajari: presisi tipografi, restraint warna, tidak ada elemen dekoratif yang tidak perlu
- **Attio.com** — pelajari: monokrom sebagai basis + aksen pastel hanya di titik tertentu, bento grid yang benar-benar menyusun informasi (bukan dekorasi)
- **Notion.so** — pelajari: ilustrasi custom yang membuat tools "produktivitas" terasa hangat & manusiawi (relevan karena kategori produk sama)
- **Tailscale.com** — pelajari: cara keluar dari palet default kategorinya sendiri (infrastruktur teknis biasanya gelap/dingin, Tailscale memilih hangat & ramah)

**Jangan meniru** studi kasus di atas secara visual — ambil prinsipnya ("keluar dari default kategori", "restraint", "ilustrasi yang menjelaskan bukan menghias"), lalu terapkan ke konsep Carbon Copy di atas.

### 4.3 Token sistem

**Warna** (nama + hex, dari dunia nota rangkap):
- `--paper` #FAFAF8 (background utama — putih kertas, BUKAN krem)
- `--ink` #1E2440 (teks utama — biru-hitam tinta karbon)
- `--duplicate-yellow` #F5C242 (aksen primer — CTA, highlight)
- `--triplicate-pink` #F2A9C4 (aksen sekunder — hover state, badge kecil, dipakai HEMAT)
- `--carbon-gray` #8A8778 (teks sekunder, divider, border)

**Tipografi:**
- Display (judul besar): monospace/typewriter character — contoh: IBM Plex Mono, weight bold, tracking rapat. Alasan: mengevokasi mesin ketik/ledger buku nota, bukan font display SaaS generik.
- Body: IBM Plex Sans — satu keluarga dengan Plex Mono jadi pairing-nya intentional, bukan asal comot dua font random.
- Utility/data (harga, label, badge): IBM Plex Mono ukuran kecil — cocok karena produknya memang tools data.
- **Jangan pakai Inter sebagai display face** — terlalu netral/default, ini font paling umum dipakai SaaS generik.

**Layout — elemen signature:**
- Kartu template ditampilkan sebagai **tumpukan lembar nota**: kartu utama (putih) dengan 2 lembar di belakangnya sedikit tergeser & rotasi 2-3° (kuning & pink mengintip di tepi) — secara visual bilang "ini akan jadi salinanmu."
- Divider antar section pakai garis putus-putus (motif perforasi/robekan nota), BUKAN hairline rule biasa.
- Section "How it works" (pilih → duplicate → pakai) BOLEH pakai angka 01/02/03 karena isinya memang urutan proses nyata — ini pengecualian yang valid, bukan dekorasi kosong.

**Motion:** minimal & bertujuan. Satu momen orkestrasi di hero (misal: kartu nota "terisi" animasi singkat saat halaman load), sisanya diam. Hindari animasi scroll-reveal bertebaran di semua section — itu salah satu tanda paling gampang ketahuan "dibuat AI".

**Signature element (satu hal yang diingat orang):** tumpukan kartu nota rangkap yang dipakai konsisten di seluruh situs — untuk kartu template, untuk badge harga, bahkan untuk tombol "Get Free Copy" (ada micro-interaction: klik tombol → efek "menyobek" lembar kuning).

---

## 5. Spesifikasi Halaman

### 5.1 Homepage (`index.html`)

| Section | Isi | Catatan |
|---|---|---|
| Hero | Nama brand + tagline "Duplicate once. Run forever." + 1 kalimat penjelas + CTA "Browse templates" | Definition Lead untuk GEO: kalimat pertama harus definisi berdiri sendiri, mis. "Repeatable adalah kumpulan sistem Notion & Airtable siap pakai yang menggantikan software langganan untuk usaha mikro." |
| What is this | 2-3 kalimat pendek | Quick Answer block — taruh di 200 kata pertama halaman |
| Katalog grid | Kartu template (pakai signature "tumpukan nota"), tiap kartu: nama, niche/tag, badge Free/Paid, tombol | Filter by niche kalau memungkinkan (tag: Fotografer/Airbnb/F&B/Kreator) |
| How it works | 3 langkah: Pilih → Duplicate → Pakai | Numbered steps (justified, lihat 4.3) |
| FAQ | Min. 5 pertanyaan: butuh akun berbayar? perlu skill teknis? boleh dipakai komersial? bagaimana update? | WAJIB pakai schema FAQPage (lihat bagian 7) |
| Email capture | Embed form MailerLite | Copy: "Dapat 1 template gratis + update sistem baru" |

### 5.2 Halaman Template (`/t/nama-template.html`) — WAJIB per template, ini mesin SEO

| Elemen | Isi | Catatan GEO |
|---|---|---|
| Definition Lead | 1 kalimat pembuka: "[Nama Template] adalah sistem [tool] yang membantu [audiens] [hasil konkret]." | Wajib di kalimat pertama |
| Quick Answer | Ringkasan 2-3 kalimat di 200 kata pertama | AI Overview mengutip dari 30% pertama konten |
| Statistik pembanding | "Menggantikan [SaaS X] $Y/bulan — bayar sekali $Z" | Statistik konkret = taktik GEO paling efektif |
| Live demo embed | iframe halaman publik Notion/Airtable | Read-only, tidak perlu login |
| Isi template | Daftar granular fitur/tabel yang ada di dalamnya | |
| FAQ | Min. 3 pertanyaan spesifik produk ini | Schema FAQPage |
| CTA ganda | "Get free copy" (email gate) DAN/ATAU "Buy Pro version" (link keluar ke marketplace) | |

### 5.3 `/thanks.html`
Konfirmasi + instruksi cek email. Nada singkat, tanpa upsell agresif di sini (upsell ada di email nurture, bukan di halaman ini).

### 5.4 `/license.html`
Ketentuan personal vs komersial, kebijakan refund. Ini sumber komplain paling umum di kompetitor — harus jelas, bukan generik copy-paste.

---

## 6. Copy Guidelines (Brand Voice)

- Pakai **"kami"**, bukan "saya" — brand adalah sistem/studio, bukan personal
- Active voice: "Duplicate template ini" bukan "Template ini bisa di-duplicate"
- Sebutkan hal dari sudut pandang pengguna: "Kamu kelola stok" bukan "Sistem mengelola inventory record"
- Spesifik lebih baik dari catchy: hindari "Revolutionize your workflow", pakai angka/hasil konkret
- Error/empty state (kalau ada, mis. filter kosong): jelaskan apa yang terjadi & apa langkah selanjutnya, jangan minta maaf berlebihan

---

## 7. Checklist SEO + GEO Teknis (wajib per halaman produk)

- [ ] Definition Lead di paragraf pertama
- [ ] Quick Answer block di 200 kata pertama
- [ ] Minimal 1 statistik/angka pembanding harga
- [ ] FAQ section + JSON-LD schema `FAQPage`
- [ ] JSON-LD schema `Product` (nama, harga, kategori) di tiap halaman template
- [ ] JSON-LD schema `Organization` di homepage
- [ ] `robots.txt` eksplisit allow: GPTBot, ClaudeBot, PerplexityBot, Google-Extended
- [ ] `sitemap.xml` submit ke Google Search Console
- [ ] Meta title unik per halaman (bukan template generik "Repeatable | Home")
- [ ] Konten di-render server-side/static (bukan di-inject via JS berat) — cek dengan "view source", teks harus kelihatan tanpa JS jalan
- [ ] Jadwalkan refresh konten tiap kuartal (tandai tanggal update di halaman)

---

## 8. Non-Fungsional

- Responsive penuh sampai mobile (mayoritas traffic dari IG akan mobile)
- Visible keyboard focus di semua elemen interaktif
- Hormati `prefers-reduced-motion`
- Lighthouse performance score target >90 (situs statis harusnya mudah capai ini)
- Self-host font (jangan render-blocking dari CDN eksternal)

---

## 9. Catatan Deploy

1. Push kode ke repo GitHub
2. Connect repo ke Cloudflare Pages → auto-deploy
3. Beli domain di Namecheap/Porkbun → arahkan DNS ke Cloudflare
4. Cek setting bot AI di Cloudflare (jangan sampai default block)
5. Submit sitemap ke Google Search Console
6. Pasang Google Analytics/Search Console SEBELUM soft-launch — butuh baseline data untuk portofolio
