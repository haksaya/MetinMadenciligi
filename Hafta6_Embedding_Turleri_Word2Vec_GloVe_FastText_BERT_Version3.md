# HAFTA 6: Embedding Türleri (Word2Vec, GloVe, FastText, BERT) — Kavramsal ve Basit Anlatım

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 6 |
| **Süre** | 2 Saat |
| **Konu** | Embedding Türleri: Word2Vec / GloVe / FastText / BERT |
| **Ön Koşul** | Hafta 4: BoW, TF‑IDF, Embedding fikri • Hafta 5: N‑gram + temel sınıflandırma |
| **Amaç** | “Kelimeyi ve cümleyi vektöre çevirme” yaklaşımını bir üst seviyeye taşımak ve embedding türlerini karşılaştırmak |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Embedding’in “anlamı vektöre taşıma” amacını tekrar edebilecektir.
- **Statik embedding** (Word2Vec/GloVe/FastText) ile **bağlamsal embedding** (BERT) fark��nı açıklayabilecektir.
- Word2Vec’in temel eğitim mantığını (CBOW / Skip-gram) basitçe anlatabilecektir.
- GloVe’un Word2Vec’ten temel farkını (eş-oluşum istatistikleri) kavramsal düzeyde ifade edebilecektir.
- FastText’in **subword** yaklaşımıyla Türkçe gibi eklemeli dillerde neden avantajlı olduğunu örnekle anlatabilecektir.
- BERT’in bağlama göre vektör üretmesinin pratik sonuçlarını (aynı kelime farklı anlam) tartışabilecektir.
- Hangi problem için hangi embedding türünün daha uygun olabileceğini değerlendirebilecektir.

---

## 1. Kısa Hatırlatma: Embedding Nedir?

Embedding, kelimeyi (veya cümleyi) **çok boyutlu bir sayısal vektör** olarak temsil etmektir.

- BoW / TF‑IDF: “Kelime kaç kez geçti?” → sayım/ağırlık odaklı
- Embedding: “Kelimenin anlamı ve ilişkileri ne?” → benzerlik ve bağlam odaklı

**Temel hedef:** Benzer anlamlı kelimeler vektör uzayında **birbirine yakın** olsun.

---

## 2. Embedding Türlerini Büyük Resimde Sınıflandıralım

Embedding’leri iki büyük grupta düşünebilirsiniz:

### A) Statik (Context-free) Embedding
Bir kelimenin **tek bir vektörü** vardır. Cümle değişse de vektör değişmez.

- Word2Vec
- GloVe
- FastText

**Artıları:** hızlı, basit, daha az kaynak ister  
**Eksileri:** “çok anlamlı” kelimelerde (yüz = face / hundred gibi) bağlamı yakalayamaz

### B) Bağlamsal (Contextual) Embedding
Kelimenin vektörü **cümleye göre değişir**.

- BERT ve türevleri

**Artıları:** bağlamı ve çok anlamlılığı daha iyi yakalar  
**Eksileri:** daha ağır (hesaplama/kurulum), genelde daha fazla kaynak ister

---

## 3. Word2Vec (Kelime Seviyesinde Gömme)

### 3.1 Word2Vec’in Ana Fikri (Basit)
“Bir kelimenin anlamı, geçtiği bağlamdan anlaşılır.”

Word2Vec, kelimeleri bağlamlarına göre öğrenir:
- “kedi” genellikle “mama”, “tüy”, “ev”, “uyumak” gibi kelimelerle birlikte geçer.
- Bu bağlam benzerliği vektörlerde yakınlığa dönüşür.

### 3.2 İki Eğitim Yaklaşımı
Word2Vec genelde iki şekilde anlatılır:

#### 1) CBOW (Continuous Bag of Words)
- **Bağlamdan kelimeyi tahmin eder**
- Örnek: “Bu ___ çok iyi” → model “ürün” kelimesini tahmin etmeye çalışır.

**Yorum:** Bağlam → hedef kelime

#### 2) Skip-gram
- **Kelimeden bağlamı tahmin eder**
- Örnek: “ürün” kelimesi verildi → model “bu”, “çok”, “iyi” gibi çevre kelimeleri tahmin eder.

**Yorum:** Hedef kelime → bağlam

> Pratikte Skip-gram daha iyi sonuçlar verebilir, CBOW daha hızlı olabilir (bu genellemedir).

### 3.3 Word2Vec Ne İşe Yarar?
- Kelime benzerliği (similarity): “güzel” ≈ “harika”
- En yakın kelimeleri bulma
- Basit metin sınıflandırma/klasik NLP işlerinde iyi başlangıç

### 3.4 Sınırları
- “bank” kelimesi (nehir kenarı / banka kurumu) gibi çok anlamlı sözcüklerde tek vektör yetersiz kalır.

---

## 4. GloVe (Global Vectors)

### 4.1 GloVe’un Temel Fikri
Word2Vec bağlamı “tahmin görevi” üzerinden öğrenirken, GloVe daha çok şuna odaklanır:

- Kelimelerin **birlikte görülme (co-occurrence)** istatistikleri
- Yani “hangi kelime hangi kelimeyle ne sıklıkta yan yana/aynı dokümanda geçiyor?”

Bu yüzden “global” (küresel) bilgi vurgusu vardır.

### 4.2 Word2Vec vs GloVe (Basit Karşılaştırma)
- **Word2Vec:** tahmin temelli öğrenme (prediction-based)
- **GloVe:** eş-oluşum temelli öğrenme (count/co-occurrence-based)

> İkisi de statik embedding üretir: kelimenin tek vektörü vardır.

### 4.3 Ne Zaman GloVe?
- Büyük ve dengeli korpuslarda güçlü sonuçlar
- Hazır (pretrained) vektörleri kullanmak yaygındır

---

## 5. FastText (Subword / Alt-kelime Yaklaşımı)

### 5.1 Neden FastText?
Türkçe gibi eklemeli dillerde aynı kelimenin çok farklı biçimleri vardır:

- “gel” → “geldim”, “geleceğim”, “geliyordum”, “gelmişsiniz” …

Word2Vec/GloVe bu kelimeleri ayrı ayrı “farklı kelime” olarak görebilir:
- Vocabulary büyür
- Nadir görülen kelimelerin vektörü kötü öğrenilebilir

### 5.2 FastText Ne Yapar?
FastText kelimeyi **parçalara** ayırır (subword / character n-gram):

- “kitaplarım” gibi bir kelimeyi küçük parçalardan temsil eder:
  - “kita”, “itab”, “tapl”, … gibi karakter n-gram’lar (temsil basitleştirme amaçlı örnek)

**Sonuç:**
- Daha az görülen kelimeler bile parçaları üzerinden anlam kazanabilir.
- Yazım hatalarına ve ekli yapılara daha dayanıklı olabilir.

### 5.3 Türkçe İçin Neden Avantaj?
- Ekler çok olduğu için kelime varyasyonu çok yüksektir.
- Subword yaklaşımı, “aynı kökten türeme” bilgisini kısmen yakalar.

---

## 6. BERT Tarzı Modeller (Bağlamsal Embedding)

### 6.1 BERT’in En Kritik Farkı
BERT’te kelimenin embedding’i **cümleye göre değişir**.

Örnek:
- “Yüzünü yıkadı.” (yüz = face)
- “Yüz TL verdim.” (yüz = hundred)

BERT bu iki “yüz” için farklı vektörler üretebilir; çünkü bağlam farklıdır.

### 6.2 BERT Nasıl Öğrenir? (Kavramsal)
BERT’in temel eğitim fikirlerinden biri:
- Cümlenin içinden bazı kelimeler maskelenir: “Bu ürün [MASK] iyi”
- Model maskelenen kelimeyi tahmin etmeye çalışır.

Bu sayede kelimeyi sadece solundan değil, **sağından da** öğrenir (bidirectional context).

### 6.3 BERT ile Neler Güçlenir?
- Duygu analizi
- Soru-cevap
- İsimli varlık tanıma (NER)
- Metin sınıflandırma (özellikle bağlam kritikse)

### 6.4 Dezavantajlar (Gerçekçi Bakış)
- Daha fazla hesaplama gücü ister
- İnferans (tahmin) süresi statik embedding’lere göre daha uzundur
- Doğru kullanım için fine-tuning / doğru pipeline gerekebilir

---

## 7. Hangi Embedding Ne Zaman Kullanılır?

Aşağıdaki tablo “kısa karar rehberi” gibi düşünülebilir:

| İhtiyaç | Öneri |
|---|---|
| Hızlı, hafif, klasik NLP | Word2Vec / GloVe |
| Türkçe’de ekler, yazım varyasyonları çok | FastText |
| Kelime anlamı bağlama göre değişiyor, yüksek doğruluk hedefi | BERT (ve türevleri) |
| Küçük kaynak + basit model | TF‑IDF + Logistic Regression (Hafta 5) hâlâ güçlü bir baseline olabilir |

> Önemli: “En iyi yöntem BERT” gibi tek cümle doğru değildir. Proje hedefi, veri boyutu, süre ve kaynaklar belirleyicidir.

---

## 8. Mini Etkinlik / Tartışma

Aşağıdaki cümlelerde hangi yaklaşım daha avantajlı olur?

1. “Bu telefon **iyi değil**.”  
   - N‑gram + TF‑IDF mi? Word2Vec mi? BERT mi?
2. “Yüzüm ağrıdı.” vs “Yüz kişi geldi.”  
   - Statik embedding mi, bağlamsal embedding mi?

---

## Haftalık Ödev

**Görev (kavramsal + küçük uygulama raporu):**
1. Word2Vec, GloVe, FastText, BERT için **1’er paragraf** tanım yazın.
2. Aşağıdaki soruları cevaplayın (kısa rapor):
   - Statik embedding ile bağlamsal embedding farkı nedir?
   - Türkçe için neden FastText avantaj sağlayabilir?
   - “yüz” örneğinde BERT neden daha iyi olabilir?
3. (İsteğe bağlı) 10 cümlelik mini bir veri seçin ve:
   - TF‑IDF + Logistic Regression ile baseline sonuç yazın (Hafta 5 mantığı)
   - BERT kullanılsa ne beklenir? (kavramsal tahmin)

---

## Özet

Bu hafta embedding dünyasını “türler” üzerinden tanıdık:

- **Word2Vec/GloVe:** kelime seviyesinde, statik vektörler
- **FastText:** subword ile özellikle Türkçe gibi eklemeli dillerde daha sağlam yaklaşım
- **BERT:** bağlama göre değişen vektörlerle çok anlamlılık ve bağlam problemlerini daha iyi yakalar

Bir sonraki hafta istersen bu konuyu uygulamaya taşıyabiliriz:
- hazır FastText vektörleriyle benzerlik,
- ya da HuggingFace üzerinden Türkçe BERT ile basit sınıflandırma.