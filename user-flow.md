# LexiaBuddy — Kullanıcı Akışı

## Normal Akış

**Adım 1 — Uygulamayı açar**
Kullanıcı siteyi tarayıcıda açar. Karşısında 4 kutu görür: üstte "LexiaBuddy" başlığı, altında yükleme alanı, gri bir "Convert" butonu ve boş bir sonuç kutusu.

**Adım 2 — PDF yükler**
Kullanıcı PDF dosyasını yükleme kutusuna sürükleyip bırakır — ya da kutuya tıklayıp dosya seçiciyi açar. PDF olmayan bir dosya yüklemeye çalışırsa, kutu onu reddeder ve uyarı gösterir.

**Adım 3 — Dosya kabul edilir**
Geçerli bir PDF yüklendiğinde yükleme kutusunda dosyanın adı görünür. "Convert" butonu griden turuncuya döner ve tıklanabilir hale gelir.

**Adım 4 — Convert'e tıklar**
Kullanıcı turuncu "Convert" butonuna basar. Sonuç kutusunda dönen yuvarlak bir yükleme animasyonu başlar.

**Adım 5 — Arka planda işlem gerçekleşir**
Kullanıcı beklerken sırayla şunlar olur:
- PDF'den metin çıkarılır (pdf.js)
- Metin ve sadeleştirme promptu Groq API'ye gönderilir
- Groq metni disleksi dostu hale getirir
- Sadeleştirilmiş metin Lexend 18pt siyah fontuyla yeni bir PDF'e dönüştürülür (jsPDF)

**Adım 6 — Sonuç hazır**
Yükleme animasyonu kaybolur. Sonuç kutusunda şunlar görünür:
- "Your PDF is ready." yazısı
- Dönüştürülmüş dosyanın adı
- Turuncu bir "Download" butonu

**Adım 7 — İndirir**
Kullanıcı "Download" butonuna tıklar. Yeni PDF cihazına kaydedilir. İşlem tamamdır.

---

## Hata Durumları

| Durum | Kullanıcının gördüğü |
|-------|----------------------|
| PDF olmayan dosya yüklendi | Yükleme kutusunda uyarı: "Please upload a PDF file." |
| PDF içinde okunabilir metin yok | "This PDF contains no readable text. Please try a text-based PDF." |
| Groq API hatası veya zaman aşımı | "Something went wrong. Please try again." |
| Groq boş yanıt döndürdü | "The AI returned an empty response. Please try again." |
| PDF oluşturulamadı | "Could not create the PDF. Please try again." |

---

## Akış Şeması

```
Kullanıcı siteyi açar
        │
        ▼
PDF sürükle-bırak veya dosya seçici
        │
        ├── PDF değil → Hata mesajı → Tekrar dene
        │
        ▼
Dosya kabul edildi → "Convert" butonu aktif (turuncu)
        │
        ▼
Kullanıcı "Convert"e tıklar
        │
        ▼
Sonuç kutusunda yükleme animasyonu başlar
        │
        ▼
pdf.js → metin çıkar
        │
        ▼
Groq API → metni sadeleştir
        │
        ├── Hata → Animasyon durur, hata mesajı gösterilir
        │
        ▼
jsPDF → Lexend 18pt siyah → yeni PDF oluştur
        │
        ▼
"Your PDF is ready." + dosya adı + Download butonu
        │
        ▼
Kullanıcı indirir ✓
```
