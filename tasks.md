# LexiaBuddy — Görev Listesi

Geliştirme sırası: Her aşamayı tamamlamadan bir sonrakine geçme.
Her görev tamamlandığında `[ ]` → `[x]` yap.

---

## Aşama 1 — Proje Kurulumu

- [ ] `lexiabuddy` adında bir klasör oluştur
- [ ] Klasörün içine boş bir `index.html` dosyası oluştur
- [ ] VS Code'u kur ve klasörü VS Code'da aç
- [ ] Groq API anahtarını [console.groq.com](https://console.groq.com) adresinden oluştur ve kopyala
- [ ] `index.html` içine temel HTML iskeletini yaz (`<!DOCTYPE html>`, `<head>`, `<body>`)

---

## Aşama 2 — Dış Kütüphaneleri Dahil Et

- [ ] Google Fonts → Lexend'i `<head>` içine `<link>` tag ile ekle
- [ ] pdf.js CDN script tag'ini `<head>` veya `<body>` sonuna ekle
- [ ] jsPDF CDN script tag'ini ekle
- [ ] Tüm kütüphanelerin yüklendiğini tarayıcı konsolunda doğrula (hata yok mu?)

---

## Aşama 3 — Kullanıcı Arayüzü (HTML + CSS)

- [ ] **Box 0** — Üst bar: "LexiaBuddy" başlığını ekle
  - [ ] "Lexia" kelimesini turuncu yap
  - [ ] "Buddy" kelimesini siyah yap
  - [ ] Başlığı ortala, font boyutu 36px
- [ ] **Box 1** — Yükleme alanı: sürükle-bırak kutusunu oluştur
  - [ ] "Upload Your PDF…" placeholder metnini soluk turuncu renkte ekle
- [ ] **Box 2** — Convert butonu: başlangıçta gri ve pasif olarak oluştur
- [ ] **Box 3** — Sonuç kutusu: boş olarak oluştur
- [ ] Tüm kutulara CSS uygula:
  - [ ] Arka plan rengi: `rgb(230, 230, 230)`
  - [ ] Siyah kenar: `1px solid black`
  - [ ] Yuvarlak köşeler: `border-radius`
  - [ ] Hafif gölge: `box-shadow`
- [ ] Sayfanın arka planını ve genel font'u ayarla (Lexend, tüm sayfa)
- [ ] Tarayıcıda açıp görsel tasarımın doğru göründüğünü kontrol et

---

## Aşama 4 — PDF Yükleme Mantığı (JavaScript)

- [ ] Box 1'e tıklanınca gizli `<input type="file" accept=".pdf">` tetiklensin
- [ ] Sürükle-bırak olaylarını dinle: `dragover`, `dragleave`, `drop`
- [ ] Yüklenen dosyanın `.pdf` uzantılı olup olmadığını kontrol et
  - [ ] PDF değilse: kutu içinde hata mesajı göster ("Please upload a PDF file.")
  - [ ] PDF ise: dosya adını Box 1 içinde göster
- [ ] Geçerli PDF yüklendiğinde Convert butonunu (Box 2) aktif hale getir (turuncu, tıklanabilir)
- [ ] Test et: PDF yükle → buton aktif oldu mu? PDF olmayan dosya → hata mesajı çıktı mı?

---

## Aşama 5 — PDF'den Metin Çıkarma (pdf.js)

- [ ] `extractTextFromPDF(file)` fonksiyonunu yaz
- [ ] Fonksiyon tüm sayfaları döngüyle gezip metni birleştirsin
- [ ] Çıkarılan metni tarayıcı konsolunda `console.log` ile görüntüle
- [ ] Test et: farklı PDF'ler yükle, konsolda metin çıkıyor mu?
- [ ] Boş/taranmış PDF durumunu kontrol et → hata mesajı: "This PDF contains no readable text."

---

## Aşama 6 — Groq API Entegrasyonu

- [ ] Dosyanın en üstüne API anahtarı değişkenini ekle:
  ```javascript
  const GROQ_API_KEY = "YOUR_API_KEY_HERE";
  ```
- [ ] Groq API anahtarını buraya yapıştır
- [ ] `simplifyWithGroq(extractedText)` fonksiyonunu yaz
- [ ] Prompt içine `${extractedText}` ile dinamik metin yerleştirmeyi uygula
- [ ] `fetch()` ile Groq API'ye istek gönder (model: `openai/gpt-oss-120b`, max_tokens: 65536)
- [ ] Gelen yanıttan sadeleştirilmiş metni çek: `data.choices[0].message.content`
- [ ] Sadeleştirilmiş metni konsolda görüntüle
- [ ] Test et: küçük bir metin gönder, Groq yanıt veriyor mu?
- [ ] API hatası durumunda hata mesajı göster: "Something went wrong. Please try again."

---

## Aşama 7 — PDF Oluşturma (jsPDF)

- [ ] `generatePDF(simplifiedText, originalFilename)` fonksiyonunu yaz
- [ ] jsPDF dökümanı oluştur
- [ ] Font: Lexend, boyut: 18pt, renk: siyah olarak ayarla
- [ ] Metni sayfa genişliğine göre satırlara böl (`splitTextToSize`)
- [ ] Sayfa dolunca otomatik yeni sayfa ekle
- [ ] Çıktı dosya adını belirle: `orijinal_ad_lexiabuddy.pdf`
- [ ] Test et: sadeleştirilmiş metin PDF olarak oluşturuluyor ve indiriliyor mu?
- [ ] PDF açıldığında Lexend fontunu ve 18pt boyutu doğrula

---

## Aşama 8 — Convert Akışını Birleştir

- [ ] Convert butonuna tıklanınca şu sırayı çalıştır:
  1. Box 3'te dönen yükleme animasyonu (spinner) göster
  2. `extractTextFromPDF()` çağır
  3. `simplifyWithGroq()` çağır
  4. `generatePDF()` çağır
  5. Spinner'ı gizle
  6. Box 3'te sonucu göster: "Your PDF is ready." + dosya adı + turuncu Download butonu
- [ ] Download butonuna tıklanınca PDF indirilsin
- [ ] Convert butonunu işlem süresince devre dışı bırak (çift tıklamayı önle)
- [ ] Uçtan uca test: yükle → convert → indir

---

## Aşama 9 — Hata Yönetimi

- [ ] Tüm hata senaryolarını test et ve mesajların doğru göründüğünü doğrula:
  - [ ] PDF olmayan dosya → "Please upload a PDF file."
  - [ ] Boş/taranmış PDF → "This PDF contains no readable text."
  - [ ] Groq API hatası → "Something went wrong. Please try again."
  - [ ] Groq boş yanıt → "The AI returned an empty response. Please try again."
  - [ ] PDF oluşturulamadı → "Could not create the PDF. Please try again."
- [ ] Hata durumlarında spinner durmalı ve hata mesajı Box 3'te görünmeli

---

## Aşama 10 — Son Kontroller

- [ ] Tüm metinlerin Lexend fontuyla göründüğünü doğrula
- [ ] Renk kontrolü: turuncu doğru yerlerde mi? (başlık, buton, download, placeholder)
- [ ] Farklı boyutlarda PDF ile test et (kısa, uzun, çok sayfalı)
- [ ] Chrome ve Firefox'ta test et
- [ ] API anahtarının `index.html` içinde açıkta olduğunu unutma — bu v1 için kabul edilebilir ama paylaşırken dikkat et

---

## Özet

| Aşama | Konu | Durum |
|-------|------|-------|
| 1 | Proje kurulumu | [ ] |
| 2 | Kütüphaneler | [ ] |
| 3 | HTML + CSS arayüzü | [ ] |
| 4 | PDF yükleme mantığı | [ ] |
| 5 | pdf.js metin çıkarma | [ ] |
| 6 | Groq API entegrasyonu | [ ] |
| 7 | jsPDF ile PDF oluşturma | [ ] |
| 8 | Tam akışı birleştirme | [ ] |
| 9 | Hata yönetimi | [ ] |
| 10 | Son kontroller | [ ] |
