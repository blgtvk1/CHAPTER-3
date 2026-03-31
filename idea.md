# LexiaBuddy — Proje Fikir Belgesi

## Genel Bakış

**LexiaBuddy**, disleksi tanısı almış ya da okuma güçlüğü yaşayan öğrenciler için tasarlanmış, yapay zeka destekli bir PDF dönüştürme web uygulamasıdır. Kullanıcı herhangi bir PDF belgesini yükler; uygulama bu belgeyi disleksi dostu bir formata dönüştürerek yeni bir PDF olarak indirilmek üzere sunar.

---

## Problem

Dislektik bireyler, standart yazı tipleri ve karmaşık cümle yapıları içeren metinleri okumakta ciddi güçlük çekerler. Okul kitapları, ders notları, makaleler ve belgeler genellikle bu kitleye hitap edecek biçimde düzenlenmemiştir. Mevcut çözümler ya çok teknik ya da çok pahalıdır.

---

## Çözüm

LexiaBuddy şu adımları otomatikleştirir:

1. **PDF Yükleme** — Kullanıcı orijinal PDF dosyasını sürükle-bırak veya dosya seçici ile yükler.
2. **AI ile İşleme** — PDF içeriği Groq AI API'sine gönderilir. AI şunları yapar:
   - Metnin dilini ve anlam bütünlüğünü koruyarak **cümle yapısını sadeleştirir**.
   - Uzun ve karmaşık paragrafları **daha kısa, anlaşılır parçalara** böler.
   - Gereksiz jargon ve karmaşık sözcükleri **daha basit eş anlamlılarıyla** değiştirir.
3. **PDF Oluşturma** — İşlenmiş metin, **Google Fonts → Lexend** yazı tipiyle yeni bir PDF'e dönüştürülür. Lexend, disleksi araştırmalarına dayanarak okunabilirlik için optimize edilmiş bir yazı tipidir.
4. **İndirme** — Kullanıcıya dönüştürülmüş PDF sunulur ve tek tıkla indirilebilir.

---

## Hedef Kitle

- Disleksi tanısı almış öğrenciler (ilkokul → üniversite)
- Özel eğitim öğretmenleri ve aileler
- Okuma güçlüğü yaşayan yetişkinler
- Erişilebilir içerik üretmek isteyen eğitimciler

---

## Teknik Mimari

### Frontend
- **Tek sayfalık web uygulaması** (HTML / CSS / JS veya React)
- **Font:** Google Fonts → Lexend (sitenin kendisi de Lexend kullanır)
- **Renk paleti:** Turuncu (#FF8C00 tonu) + Siyah + Açık gri arka plan (RGB 230, 230, 230)
- **Bileşenler:**
  | Kutu | Rol |
  |------|-----|
  | 0 — Üst Bar | Logo: "**Lexia**" (turuncu) + "**Buddy**" (siyah), font 36px, ortalı |
  | 1 — Yükleme Alanı | Drag & drop veya dosya seçici; yalnızca PDF kabul eder |
  | 2 — Dönüştür Butonu | PDF yüklenmeden önce gri/pasif; yüklendikten sonra turuncu/aktif |
  | 3 — Sonuç Alanı | Yüklenirken dönen animasyon; hazır olunca dosya adı + indirme butonu |

### Backend / AI
- **Groq AI API** — Hızlı LLM çıkarımı için (model: llama3 veya benzeri hızlı model)
- **Sistem Promptu (taslak — sonra tamamlanacak):**
  ```
  Sen dislektik öğrenciler için metin sadeleştirme uzmanısın.
  Verilen metni şu kurallara göre yeniden yaz:
  - Kısa, basit cümleler kullan (maks. 15–20 kelime).
  - Sık kullanılmayan kelimeleri daha basit eş anlamlılarıyla değiştir.
  - Anlam bütünlüğünü ve orijinal bilgiyi koru.
  - Paragrafları kısa tut; her paragraf tek bir fikri anlatsın.
  - Madde işaretleri ve numaralı listeler gerektiğinde kullan.
  [PROMPT DETAYLARI SONRA EKLENECEK]
  ```
- **PDF Oluşturma:** İşlenmiş metin → Lexend fontlu PDF (örn. `pdfmake`, `jsPDF` veya sunucu taraflı `reportlab` / `WeasyPrint`)

---

## Kullanıcı Akışı

```
Kullanıcı siteyi açar
    │
    ▼
PDF dosyasını Kutu 1'e yükler
    │
    ▼
"Dönüştür" butonu aktif hale gelir (Kutu 2)
    │
    ▼
Kullanıcı butona tıklar
    │
    ▼
Kutu 3'te yükleme animasyonu başlar
    │
    ▼
PDF → metin çıkarımı yapılır
    │
    ▼
Metin Groq AI API'sine gönderilir
    │
    ▼
AI sadeleştirilmiş metni döndürür
    │
    ▼
Lexend fontlu yeni PDF oluşturulur
    │
    ▼
Kutu 3: "PDF'iniz hazır." + dosya adı + İndir butonu
```

---

## Tasarım İlkeleri

- **Erişilebilirlik önce gelir:** Yüksek kontrast, sade düzen, büyük tıklama alanları.
- **Sürtünmesiz deneyim:** Kullanıcı 3 adımda işi bitirir (Yükle → Dönüştür → İndir).
- **Dürüst geri bildirim:** Her aşamada kullanıcıya ne olduğu görsel olarak bildirilir.
- **Güven:** Yüklenen PDF'ler işlem sonrası saklanmaz (gizlilik).

---

## Gelecek Özellikler (v2+)

- Dil seçeneği (Türkçe / İngilizce metin sadeleştirme)
- Yazı boyutu ve satır aralığı ayarı
- Kelime vurgulama modu (OpenDyslexic alternatif font seçeneği)
- Toplu PDF dönüştürme
- Öğretmen paneli (sınıf için toplu belge yönetimi)

---

## Sonraki Adımlar

- [ ] Groq API sistem promptunu tamamla ve test et
- [ ] PDF → metin çıkarım kütüphanesini seç (PyMuPDF, pdf.js, vb.)
- [ ] Metin → Lexend PDF dönüşüm kütüphanesini seç
- [ ] Frontend prototipini kodla
- [ ] Uçtan uca testi gerçekleştir
- [ ] Dislektik kullanıcılarla kullanılabilirlik testi yap
