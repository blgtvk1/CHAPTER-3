# LexiaBuddy — Proje Fikir Belgesi

---

## 1. Problem: Ne Çözüyorum?

Dislektik öğrenciler her gün standart formatta hazırlanmış PDF belgelerle karşılaşır: ders notları, makale, sınav metni, ödev talimatı. Bu belgeler uzun cümleler, karmaşık kelimeler ve standart yazı tipleriyle dolu. Disleksi bu metinlerin **kod çözme yükünü** artırır; öğrenci içeriği anlamak yerine kelimelerle boğuşmak zorunda kalır.

Mevcut yardımlar iki uçta takılı:
- **Seslendirme araçları** (Speechify, NaturalReader): Metni okur ama sadeleştirmez. Karmaşık bir cümle sesli okunduğunda yine karmaşık kalır.
- **Uzman eğitim programları** (Barton, Wilson): Okumayı öğretir ama yıllar sürer ve pahalıdır. Öğrenci bu metni *bugün* anlamak zorunda.

**LexiaBuddy bu iki boşluğu kapatır:** Kullanıcının elindeki PDF'i alır, metnin dilini ve bilgisini koruyarak yeniden yazar — daha kısa cümlelerle, daha basit kelimelerle — ve disleksi okumaya optimize edilmiş Lexend fontuyla yeni bir PDF üretir. Öğrenci bu dosyayı indirir ve okur.

---

## 2. Kullanıcı: Bu Uygulamayı Kim Kullanacak?

**Birincil kullanıcı:** Disleksi tanısı almış ya da okuma güçlüğü yaşayan öğrenciler (ilkokul üstü, üniversiteye kadar). Elinde bir PDF var, onu kendi seviyesinde okumak istiyor. Teknik bilgiye ihtiyaç duymadan, tek başına kullanabilmeli.

**İkincil kullanıcılar:**
- **Öğretmenler ve özel eğitim uzmanları:** Ders materyallerini sınıftaki dislektik öğrenciler için hızla uyarlamak isteyenler.
- **Ebeveynler:** Çocuklarının okul belgelerini evde erişilebilir hale getirmek isteyenler.
- **Yetişkin dislektikler:** İş hayatında karşılaştıkları ağır raporları ya da sözleşmeleri kendi okuma hızlarına uydurmak isteyenler.

**Ortak özellik:** Hepsi teknik değil. Araç sade, adım sayısı az, açıklama gerektirmemeli.

---

## 3. AI'ın Rolü: Yapay Zeka Bu Çözümde Ne Yapıyor?

Bu projede AI, basit bir metin aktar-dönüştür işlemi değil, **aktif bir sadeleştirme editörü** rolündedir.

**AI şunları yapar:**

| Görev | Açıklama |
|-------|-----------|
| Cümle sadeleştirme | Uzun, iç içe cümleleri kısa ve net yapılara böler (maks. ~15 kelime) |
| Kelime değiştirme | Nadir veya teknik kelimeleri, anlamı bozmadan daha yaygın eş anlamlılarıyla değiştirir |
| Paragraf yeniden yapılandırma | Her paragrafın tek bir fikir taşımasını sağlar |
| Anlam koruma | Orijinal bilgiyi, argümanı ve sırayı değiştirmez |
| Biçim önerisi | Uygun yerlerde madde işareti veya numaralı liste önerir |

**Kullanılan servis:** Groq AI API — düşük gecikme süresiyle hızlı LLM çıkarımı için.
**Model:** llama3 veya benzeri hızlı, instruction-tuned model.
**Sistem promptu:** *(Sonraki aşamada tamamlanacak — dil: Türkçe/İngilizce, ton: sade, hedef kitle: dislektik öğrenci)*

---

## 4. Rakip Durum: Benzer Çözümler Var mı? Benimki Nasıl Farklı?

Disleksi destek araçları pazarında birkaç büyük kategori var:

**Metin-sese çevirme araçları** (Speechify, NaturalReader, Microsoft Immersive Reader):
Metni sesli okur. Sadeleştirmez, fontu değiştirmez, indirilebilir çıktı vermez.

**Yazı tipi tarayıcı eklentileri** (OpenDyslexic for Chrome, Helperbird):
Web sayfalarını disleksi fontuna çevirir. PDF'leri işleyemez, yeni dosya üretmez.

**Kapsamlı okuma platformları** (Kurzweil 3000, Read&Write):
Güçlü ama pahalı, abonelik gerektirir, kurulum ister, kurumsal lisans mantığıyla çalışır.

**Genel AI özetleme araçları** (Mindgrasp, ChatGPT):
Metni özetler ama disleksi odaklı değil, PDF çıktısı vermez, fontu optimize etmez.

**LexiaBuddy'nin farkı:**

| Özellik | Rakipler | LexiaBuddy |
|---------|----------|------------|
| PDF girdi → PDF çıktı | ✗ | ✓ |
| Lexend font ile indirilebilir belge | ✗ | ✓ |
| AI ile metin sadeleştirme (özetleme değil) | Kısmen | ✓ |
| Kurulum yok, abonelik yok | Nadiren | ✓ |
| Disleksi odaklı tasarım | Kısmen | ✓ |

**Kısaca:** Mevcut araçlar metni *erişilebilir kılar* (seslendirme, font overlay). LexiaBuddy metni *yeniden yazar ve belge olarak teslim eder*. Bu, özellikle sınav ortamlarında ve bağlantısız kullanımda kritik bir farktır.

---

## 5. Başarı Kriteri: Bu Proje Başarılı Olursa Ne Değişecek?

**Kullanıcı seviyesinde:**
- Dislektik bir öğrenci, bugün anlayamadığı bir ders notunu dönüştürüp kendi hızında okuyabilir hale gelir.
- Bir öğretmen, tüm sınıf için hazırladığı PDF'i dakikalar içinde disleksi dostu versiyona çevirebilir.
- Ebeveyn, okul belgesini eve getirir ve çocuğu için hemen uyarlar.

**Ürün seviyesinde:**
- Kullanıcı, uygulamayı açıklamaya gerek duymadan ilk denemede tamamlayabilmeli (sıfır öğrenme eğrisi).
- Dönüşüm sonucu kullanıcı tarafından "daha kolay okunuyor" olarak değerlendirilebilmeli.
- Uygulama, kullanıcıdan herhangi bir kişisel bilgi almadan çalışabilmeli.

**Daha büyük resimde:**
Disleksi, dünyada her 5 kişiden 1'ini etkiliyor. Mevcut destek araçlarının çoğu ya pahalı ya da kurumsal. LexiaBuddy, bu boşluğu ücretsiz, tarayıcı tabanlı ve sıfır kurulumlu bir araçla doldurmayı hedefliyor. Başarı, bir öğrencinin "bunu ben de yapabilirim" demesiyle başlar.

---

*Sonraki adım: Groq sistem promptunu yaz → Frontend kodlamasına geç*
