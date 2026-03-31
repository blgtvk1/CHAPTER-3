# LexiaBuddy — Teknoloji Yığını

## Genel Yaklaşım

Başlangıç seviyesi için en doğru seçim: **tek bir HTML dosyası.**
Sunucu yok, framework yok, kurulum yok. Tarayıcıda çift tıklayıp açılır.

---

## Klasör Yapısı

```
lexiabuddy/
└── index.html        ← tek dosya, her şey burada
```

---

## Teknolojiler

### 1. HTML + CSS + Vanilla JavaScript
**Ne için:** Kullanıcı arayüzü — 4 kutu, buton, animasyon, tüm görsel yapı.
**Neden:** Tek dosya, sıfır kurulum. React, Vue gibi framework'ler öğrenme eğrisi ekler — bu projede gereksiz. Tarayıcı zaten her şeyi anlıyor.
**Nasıl dahil edilir:** Zaten dosyanın içinde, ek bir şey gerekmez.

---

### 2. pdf.js
**Ne için:** Kullanıcının yüklediği PDF'den metni çıkarmak.
**Neden:** Adobe'nin resmi açık kaynak kütüphanesi, en güvenilir seçenek. Kurulum yok — script tag ile dahil edilir.
**Nasıl dahil edilir:**
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/pdf.js/3.11.174/pdf.min.js"></script>
```

---

### 3. Groq API
**Ne için:** Çıkarılan metni disleksi dostu, sadeleştirilmiş metne dönüştürmek.
**Neden:** Düşük gecikme süresiyle hızlı LLM çıkarımı. Tarayıcıdan direkt `fetch()` ile çağrılır, arada sunucu gerekmez.
**Model:** `openai/gpt-oss-120b`
**Endpoint:** `https://api.groq.com/openai/v1/chat/completions`
**Max tokens:** `65536`
**API Key placeholder:**
```javascript
const GROQ_API_KEY = "YOUR_API_KEY_HERE"; // ← Groq API anahtarını buraya yapıştır
```

---

### 4. jsPDF
**Ne için:** Sadeleştirilmiş metni Lexend fontuyla yeni ve indirilebilir bir PDF'e dönüştürmek.
**Neden:** Tarayıcı tabanlı, sunucu gerektirmez. Script tag ile dahil edilir, kurulum yok.
**Nasıl dahil edilir:**
```html
<script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
```

---

### 5. Google Fonts → Lexend
**Ne için:** Sitenin tamamında ve oluşturulan PDF'te kullanılan yazı tipi. Disleksi araştırmalarına göre okunabilirlik için optimize edilmiştir.
**Nasıl dahil edilir:**
```html
<link href="https://fonts.googleapis.com/css2?family=Lexend&display=swap" rel="stylesheet">
```

---

## Özet Tablosu

| İhtiyaç | Araç | Nasıl dahil edilir |
|---------|------|--------------------|
| Kullanıcı arayüzü | HTML / CSS / Vanilla JS | Dosyanın içinde |
| PDF'den metin çıkarma | pdf.js | `<script>` CDN |
| AI metin sadeleştirme | Groq API | `fetch()` ile direkt çağrı |
| PDF oluşturma | jsPDF | `<script>` CDN |
| Yazı tipi | Google Fonts → Lexend | `<link>` CDN |
| Kod editörü | VS Code | Bir kez kur, hep kullan |

---

## Kurulum Adımları

**Adım 1 — Klasör oluştur**
Masaüstünde `lexiabuddy` adında bir klasör aç.

**Adım 2 — index.html dosyası oluştur**
Klasörün içine `index.html` adında boş bir dosya oluştur.

**Adım 3 — VS Code kur**
[code.visualstudio.com](https://code.visualstudio.com) adresinden indir ve kur.

**Adım 4 — Groq API anahtarını al**
[console.groq.com](https://console.groq.com) → API Keys → Create API Key → kopyala.
`index.html` içindeki `YOUR_API_KEY_HERE` yerine yapıştır.

**Adım 5 — Tarayıcıda aç**
`index.html` dosyasına çift tıkla. Chrome veya Firefox'ta açılır. Sunucu gerekmez.

**Adım 6 — Geliştirme döngüsü**
VS Code'da değişiklik yap → `Ctrl+S` ile kaydet → Tarayıcıda `F5` ile yenile.

---

## Kapsam Dışı (v1)

- Node.js, Python veya herhangi bir backend sunucusu
- npm, Webpack, Vite gibi build araçları
- React, Vue, Angular gibi frontend framework'leri
- Veritabanı veya dosya depolama
- Kullanıcı girişi veya hesap sistemi
