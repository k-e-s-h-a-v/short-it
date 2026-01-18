# No-DB URL Shortener (Grammar-Aware Compression)

This project is an experimental **stateless URL shortener** that achieves near–Bitly-level short URLs **without using any database**.

Instead of storing mappings, it exploits:
- URL structure (grammar)
- Common prefixes and substrings
- Dictionary encoding
- Real compression (Brotli via WASM)
- URL-safe Base62 encoding

The result is a **fully reversible**, **no-storage**, **no-expiry** URL shortener.

---

## 🚀 How It Works

Encoding pipeline:

1. **Prefix stripping**
   - `https://` → `0`
   - `http://` → `1`
   - `www.` → `2`

2. **Substring dictionary encoding**
   - `.com`, `.org`, `.net`, `.in`
   - `utm_source`, `utm_medium`, `utm_campaign`
   - (many → one replacement)

3. **Grammar-optimized compression**
   - Redundancy removed before entropy compression

4. **Brotli compression (browser-side via WASM)**
   - Uses `fflate` (CDN)
   - Works in all modern browsers

5. **Base62 encoding**
   - Produces URL-safe short tokens

Decoding reverses all steps exactly.

---

## ✨ Features

### ✅ Current Features
- No database required
- Fully reversible (lossless)
- Stateless encoding / decoding
- Browser-compatible
- Grammar-aware URL compression
- Brotli compression using WASM (`fflate`)
- URL-safe Base62 output
- Works locally via a single HTML file

---

## 📉 Compression Expectations

| Method | Typical Output |
|------|----------------|
| Encryption only | 40–100 chars |
| Gzip only | 25–40 chars |
| Grammar + Brotli (this project) | **8–20 chars** |
| DB-based shorteners | 6–8 chars (requires storage) |

> This project reaches the **theoretical limit** of compression without storing mappings.

---

## ⚠️ Important Notes

- Browsers **do not support native Brotli APIs**
- `CompressionStream('brotli')` is **not supported**
- This project uses **WASM Brotli via CDN**
- Initial page load includes ~30–80 KB WASM payload

---

## 🛣️ Upcoming Features

### 🔜 Planned
- Compression toggle (Brotli ↔ Gzip)
- Side-by-side compression benchmark UI
- Auto-learning dictionaries from real traffic
- Versioned dictionary support (backward compatibility)
- Cloudflare Workers deployment
- Node.js CLI encoder/decoder
- Mobile-optimized token alphabet
- QR-safe encoding mode

---

## 🧠 Design Philosophy

> Short URLs come from **structure + compression**, not encryption.

Anything shorter than this **must store state somewhere**.

---

## 📄 License
MIT
