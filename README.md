# 🎓 EduMate AI — Smart Learning Companion

> **Final Project** — LLM Based Tools and Gemini API Integration for Data Scientists

[![Model](https://img.shields.io/badge/LLM-Claude%20Sonnet%204-6C63FF?style=for-the-badge)](https://anthropic.com)
[![Language](https://img.shields.io/badge/Language-Bahasa%20Indonesia-2DD4BF?style=flat-square)]()
[![Status](https://img.shields.io/badge/Status-Production%20Ready-22c55e?style=flat-square)]()
[![License](https://img.shields.io/badge/License-MIT-F5C842?style=flat-square)]()

---

## 📋 Deskripsi Proyek

**EduMate AI** adalah chatbot tutor berbasis AI yang dirancang untuk membantu pelajar dan mahasiswa Indonesia memahami berbagai mata pelajaran dengan cara yang mudah, interaktif, dan menyenangkan. Dibangun menggunakan **Large Language Model (LLM)** terkini dengan arsitektur *single-page application* tanpa dependensi framework eksternal.

### 🎯 Use Case
**Education Bot / AI Tutor** — Pendamping belajar cerdas yang mampu menjelaskan konsep kompleks, membuat soal latihan, memberikan pembahasan, dan menyesuaikan gaya pengajaran sesuai kebutuhan pengguna.

---

## ✨ Fitur Unggulan

### 🤖 AI & NLP Features
| Fitur | Deskripsi |
|-------|-----------|
| **Multi-turn Memory** | Riwayat percakapan dikirim setiap request untuk konteks yang berkelanjutan |
| **Web Search Integration** | Tombol 🔍 mengaktifkan tool `web_search` untuk info terkini |
| **Adaptive Teaching Mode** | 4 mode: Normal, ELI5, Mendalam, Buat Soal |
| **Subject Context Injection** | Setiap pesan dilengkapi konteks mata pelajaran aktif |
| **Mood-aware Responses** | Parameter mood memengaruhi tone dan pendekatan penjelasan |

### 🎨 UI/UX Features
| Fitur | Deskripsi |
|-------|-----------|
| **8 Mata Pelajaran** | Matematika, Fisika, Kimia, Biologi, Pemrograman, Sejarah, Bahasa Inggris, Ekonomi |
| **Dynamic Quick Prompts** | 4 prompt shortcut yang berubah sesuai mata pelajaran dipilih |
| **Chip Controls** | Toggle: Jelaskan / Contoh / Kuis / Ringkasan |
| **XP & Level System** | Gamifikasi: Pemula → Berkembang → Mahir → Ahli |
| **System Prompt Viewer** | Transparansi penuh konfigurasi AI |
| **Session Stats** | Tracking jumlah sesi & pesan |
| **Daily Tips** | 5 tips belajar yang tampil acak tiap sesi |
| **Dark Theme Premium** | Desain dark mode modern dengan warna aksen ungu |

---

## 🔧 Konfigurasi Parameter Kreatif

```json
{
  "model": "claude-sonnet-4-20250514",
  "max_tokens": 1000,
  "temperature": 0.6,
  "top_p": 0.9,
  "domain": "Education / Multi-subject Tutoring",
  "language": "Bahasa Indonesia",
  "style": "Santai + Informatif + Encouraging",
  "memory": "Multi-turn context window",
  "web_search": "Optional (user-controlled toggle)",
  "gamification": "XP points + Level progression"
}
```

### Strategi System Prompt
EduMate AI menggunakan system prompt berlapis yang mendefinisikan:

1. **Kepribadian** — Antusias, sabar, suportif, merayakan kemajuan pelajar
2. **Mode Pengajaran** — 4 mode berbeda yang diinjeksikan ke setiap pesan
3. **Format Output** — Heading, bullet points, bold untuk istilah kunci, code block
4. **Batasan Etis** — Tidak mengerjakan PR langsung, mendorong pemahaman
5. **Chip Instructions** — Instruksi tambahan berdasarkan chip yang aktif

---

## 🏗️ Arsitektur Teknis

```
edumate-ai/
├── index.html              # Single-file application (~600 baris)
│   ├── <style>             # CSS custom design system (dark theme)
│   ├── HTML Structure      # Layout: Nav + Sidebar + Chat + Right Panel
│   └── <script>            # Logika chatbot + state management + API
└── README.md
```

### Data Flow
```
User Input
    ↓
buildContext()              ← Injeksi: subject + mode + mood + chips
    ↓
history.push(userMsg)       ← Tambah ke multi-turn history
    ↓
fetch('/v1/messages')       ← POST ke Anthropic API
    │   ├── system: SP      ← System prompt lengkap
    │   ├── messages: history
    │   └── tools?: [{web_search}]  ← Jika wsOn = true
    ↓
Parse response              ← Gabung semua text blocks
    ↓
appendMsg('bot', reply)     ← Render dengan Markdown formatter
    ↓
addXP(2)                    ← Update gamifikasi
```

### Markdown Formatter
Mendukung rendering:
- `**bold**` → `<strong>`
- `*italic*` → `<em>`
- `` `code` `` → `<code>`
- ` ```codeblock``` ` → `<pre>`
- `### heading` → `<h3>`
- `> quote` → `<blockquote>`
- `- list` / `1. list` → `<ul><li>`

---

## 🚀 Cara Menjalankan

### Prasyarat
- Browser modern (Chrome 90+, Firefox 88+, Edge 90+)
- API Key dari [Anthropic Console](https://console.anthropic.com)

### Langkah Instalasi

**1. Clone repository**
```bash
git clone https://github.com/username/edumate-ai.git
cd edumate-ai
```

**2. Tambahkan API Key**

Buka `index.html`, cari fungsi `sendMsg()`, tambahkan header berikut:

```javascript
const res = await fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
    'x-api-key': 'sk-ant-XXXXXXXX',        // ← API key kamu
    'anthropic-version': '2023-06-01',      // ← tambahkan ini
    'anthropic-dangerous-direct-browser-access': 'true'  // ← untuk browser
  },
  body: JSON.stringify(body)
});
```

> ⚠️ **Keamanan:** Untuk produksi, gunakan backend proxy (Node.js/Python) agar API key tidak terekspos di client-side.

**3. Jalankan**
```bash
# Opsi 1: Langsung buka file
open index.html

# Opsi 2: Live server (direkomendasikan)
npx serve .
# Buka http://localhost:3000

# Opsi 3: Python HTTP server
python -m http.server 8080
# Buka http://localhost:8080
```

### Deploy ke GitHub Pages
```bash
git add .
git commit -m "feat: EduMate AI final project"
git push origin main
# Aktifkan GitHub Pages di Settings → Pages → Branch: main
```

---

## 💬 Contoh Percakapan

### Mode Normal
**User:** Jelaskan konsep limit dalam kalkulus  
**EduMate:** Hei! Pertanyaan yang bagus tentang kalkulus 😊

**### Apa itu Limit?**
Bayangkan kamu berjalan mendekati sebuah pintu. Kamu bisa terus mendekat, semakin dekat, dan semakin dekat lagi — tapi belum tentu kamu masuk ke dalamnya. Nah, **limit** adalah "nilai yang didekati" oleh suatu fungsi ketika variabelnya mendekati suatu titik tertentu...

### Mode ELI5
**User:** Jelaskan atom [Mode: ELI5]  
**EduMate:** Oke, bayangkan kamu punya bola LEGO raksasa 🧱 Kalau kamu pecah jadi bagian kecil-kecil... sampai sekecil mungkin... nah bagian terkecil itu namanya **atom**! Atom itu kayak "batu bata" semua benda di dunia ini...

---

## 🎮 Sistem Gamifikasi

| Level | Poin | Badge |
|-------|------|-------|
| Pemula | 0–19 | 🌱 |
| Berkembang | 20–49 | 📘 |
| Mahir | 50–79 | ⭐ |
| Ahli | 80–100 | 🏆 |

Setiap respons bot yang berhasil memberikan **+2 XP**.

---

## 📸 Screenshots

Lihat folder `/screenshots/` di repository ini.

---

## 🛠️ Tech Stack

- **Frontend:** HTML5 + CSS3 + Vanilla JavaScript (zero framework)
- **AI Engine:** Anthropic Claude Sonnet 4 (REST API)
- **Fonts:** Syne (display) + Outfit (body) via Google Fonts
- **Icons:** Unicode emoji (no external icon library)
- **Deployment:** Static file — GitHub Pages / Netlify / Vercel

---

## 📝 Catatan Akademis

Proyek ini dibuat sebagai **Final Project** pelatihan *LLM Based Tools and Gemini API Integration for Data Scientists*. EduMate AI menyediakan informasi edukatif berbasis AI. Untuk keperluan ujian resmi, selalu verifikasi materi dengan buku teks, modul resmi, atau pengajar.

---

## 👨‍💻 Author

Dikembangkan sebagai demonstrasi implementasi LLM-based chatbot dengan kreativitas penuh dalam desain UI/UX, konfigurasi parameter, dan arsitektur prompt engineering.

---

## 📄 License

MIT License — bebas digunakan, dimodifikasi, dan didistribusikan.
