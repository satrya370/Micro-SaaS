# PLANNING.md — Repeatable Landing Page & Catalog

## Fase Eksekusi

---

### Fase 0: Persiapan & Setup Project

**Goal:** Struktur proyek siap, asset dasar ada, tooling jalan.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 0.1 | Inisialisasi struktur folder | Buat `/t/`, `/assets/css/`, `/assets/js/`, `/assets/img/`, `/assets/fonts/`, `/schema/` | 5m |
| 0.2 | Download & self-host font | IBM Plex Mono + IBM Plex Sans (woff2), taruh di `/assets/fonts/` | 10m |
| 0.3 | Setup `style.css` | Definisikan CSS custom properties (token warna + tipografi dari spec 4.3) | 10m |
| 0.4 | Setup `main.js` | Skeleton JS: filter katalog, micro-interaction (efek sobek tombol) | 10m |
| 0.5 | Siapkan favicon | Buat favicon sederhana (ikon nota/duplicate) | 5m |
| 0.6 | Buat `robots.txt` | Allow: GPTBot, ClaudeBot, PerplexityBot, Google-Extended | 2m |

---

### Fase 1: Homepage (`index.html`)

**Goal:** Halaman utama dengan semua section dari spec 5.1.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 1.1 | **Hero section** | Brand name + tagline + Definition Lead + CTA "Browse templates" + animasi kartu nota terisi | 20m |
| 1.2 | **What is this** | Quick Answer block (2-3 kalimat, 200 kata pertama) | 5m |
| 1.3 | **Katalog grid** | Kartu template dengan signature "tumpukan nota" (3 lembar: putih/kuning/pink), filter by niche tag | 30m |
| 1.4 | **How it works** | 3 langkah numbered: Pilih → Duplicate → Pakai, divider garis putus-putus | 15m |
| 1.5 | **FAQ section** | Min 5 pertanyaan + schema FAQPage JSON-LD | 15m |
| 1.6 | **Email capture** | Embed MailerLite form snippet | 5m |
| 1.7 | **Footer** | Link ke /license, SEO footer sederhana | 5m |
| 1.8 | **Schema Organization** | JSON-LD Organization di homepage | 5m |

---

### Fase 2: Halaman Template (`/t/`)

**Goal:** Minimal 2 halaman template sebagai proof-of-concept untuk SEO.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 2.1 | Buat `/t/template-slug-1.html` | Definition Lead, Quick Answer, statistik pembanding, embed demo, daftar fitur, FAQ, CTA ganda | 30m |
| 2.2 | Buat `/t/template-slug-2.html` | Sama seperti di atas, konten berbeda (niche lain) | 30m |
| 2.3 | Schema `Product` JSON-LD | Tiap halaman template dapat schema Product | 10m |

---

### Fase 3: Halaman Pendukung

**Goal:** Halaman non-produk yang wajib ada.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 3.1 | `/thanks.html` | Konfirmasi email + instruksi cek inbox | 5m |
| 3.2 | `/license.html` | Ketentuan personal vs komersial, kebijakan refund | 10m |

---

### Fase 4: SEO & Schema

**Goal:** Semua checklist spec 7 terpenuhi.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 4.1 | JSON-LD `FAQPage` | Di homepage + tiap halaman template | Termasuk di Fase 1.5 & 2.1-2.2 |
| 4.2 | JSON-LD `Product` | Di tiap halaman template | Termasuk di Fase 2.3 |
| 4.3 | JSON-LD `Organization` | Di homepage | Termasuk di Fase 1.8 |
| 4.4 | Meta title unik | Setiap halaman dapet title spesifik, bukan template generik | 5m |
| 4.5 | `sitemap.xml` | Generate manual (site kecil, cukup hardcode) | 5m |
| 4.6 | Verifikasi "view source" | Pastikan semua teks konten muncul tanpa JS | 5m |

---

### Fase 5: CSS Styling & Polish (Carbon Copy Design)

**Goal:** Implementasi penuh konsep desain "Carbon Copy" dari spec 4.2-4.3.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 5.1 | Token CSS lengkap | Warna, tipografi, spacing variables | 10m |
| 5.2 | Signature "tumpukan nota" | CSS untuk efek 3 lembar (putih + kuning offset + pink offset) | 20m |
| 5.3 | Divider perforasi | Garis putus-putus antar section | 10m |
| 5.4 | Tombol "sobek" micro-interaction | Klik → animasi CSS + JS efek lembar kuning disobek | 15m |
| 5.5 | Responsive layout | Mobile-first sampai desktop | 20m |
| 5.6 | Hover & focus states | Keyboard focus visible, hover transisi ringan | 10m |
| 5.7 | `prefers-reduced-motion` | Respect user preference | 5m |
| 5.8 | Kartu template interaktif | Hover → lembar offset bergerak sedikit | 10m |

---

### Fase 6: Integrasi Pihak Ketiga

**Goal:** Form, demo, dan checkout terhubung.

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 6.1 | Embed MailerLite form | Di homepage (email capture) + gate di halaman template | 10m |
| 6.2 | Setup link Gumroad/Lemon Squeezy | Placeholder link di tombol "Buy Pro version" | 5m |
| 6.3 | Embed demo Notion/Airtable | iframe read-only di halaman template | 5m |

---

### Fase 7: QA & Pre-Deploy

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 7.1 | Lighthouse audit | Target >90 performance, cek accessibility | 10m |
| 7.2 | Cek responsive mobile | Semua halaman di 375px, 768px, 1024px, 1440px | 10m |
| 7.3 | Validasi HTML/W3C | No broken tags, semantic HTML | 5m |
| 7.4 | Cek "view source" SEO | Semua teks konten visible tanpa JS | 5m |
| 7.5 | Cek semua link | Tidak ada broken link internal/eksternal | 5m |

---

### Fase 8: Deploy ke Cloudflare Pages

| # | Task | Detail | Estimasi |
|---|------|--------|----------|
| 8.1 | Push ke GitHub repo | Semua file ke repo baru | 5m |
| 8.2 | Connect Cloudflare Pages | Auto-deploy dari GitHub | 10m |
| 8.3 | Setup custom domain | DNS Namecheap/Porkbun → Cloudflare | 15m |
| 8.4 | Unblock AI crawlers | Cek dashboard Cloudflare: GPTBot, ClaudeBot, PerplexityBot tidak diblokir | 5m |
| 8.5 | Submit sitemap | Google Search Console | 5m |
| 8.6 | Pasang GA/analytics | Sebelum soft-launch untuk baseline data | 10m |

---

## Urutan Eksekusi yang Disarankan

```
Fase 0 (Setup)
  → Fase 5 (CSS/Design System — paralel awal biar jadi acuan)
    → Fase 1 (Homepage)
      → Fase 2 (Halaman Template)
        → Fase 3 (Halaman Pendukung)
          → Fase 4 (SEO Schema)
            → Fase 6 (Integrasi)
              → Fase 7 (QA)
                → Fase 8 (Deploy)
```

---

## Catatan Penting

1. **Nama brand "Repeatable"** → find & replace nanti setelah nama final diputuskan
2. **Template dummy** → untuk Fase 2, pakai 2 template fiktif dulu, konten bisa diisi belakangan
3. **MailerLite snippet** → perlu didapatkan dulu dari dashboard MailerLite
4. **Gumroad/Lemon Squeezy link** → placeholder, ganti dengan link asli nanti
5. **Domain** → beli setelah nama final diputuskan

---

## Total Estimasi Waktu: ~7 jam (termasuk buffer)
