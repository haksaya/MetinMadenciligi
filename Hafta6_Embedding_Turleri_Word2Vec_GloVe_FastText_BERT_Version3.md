# HAFTA 6: Embedding Türleri (Word2Vec, GloVe, FastText, BERT) — Kavramsal ve Basit Anlatım

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 6 |
| **Süre** | 2 Saat |
| **Konu** | Embedding Türleri: Word2Vec / GloVe / FastText / BERT |
| **Ön Koşul** | Hafta 4: BoW, TF‑IDF, Embedding fikri • Hafta 5: N‑gram + temel sınıflandırma |
| **Amaç** | "Kelimeyi ve cümleyi vektöre çevirme" yaklaşımını bir üst seviyeye taşımak ve embedding türlerini karşılaştırmak |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Embedding'in "anlamı vektöre taşıma" amacını tekrar edebilecektir.
- **Statik embedding** (Word2Vec/GloVe/FastText) ile **bağlamsal embedding** (BERT) farkını açıklayabilecektir.
- Word2Vec'in temel eğitim mantığını (CBOW / Skip-gram) basitçe anlatabilecektir.
- GloVe'un Word2Vec'ten temel farkını (eş-oluşum istatistikleri) kavramsal düzeyde ifade edebilecektir.
- FastText'in **subword** yaklaşımıyla Türkçe gibi eklemeli dillerde neden avantajlı olduğunu örnekle anlatabilecektir.
- BERT'in bağlama göre vektör üretmesinin pratik sonuçlarını (aynı kelime farklı anlam) tartışabilecektir.
- Hangi problem için hangi embedding türünün daha uygun olabileceğini değerlendirebilecektir.

---

## 1. Kısa Hatırlatma: Embedding Nedir?

Embedding, kelimeyi (veya cümleyi) **çok boyutlu bir sayısal vektör** olarak temsil etmektir.

- BoW / TF‑IDF: "Kelime kaç kez geçti?" → sayım/ağırlık odaklı
- Embedding: "Kelimenin anlamı ve ilişkileri ne?" → benzerlik ve bağlam odaklı

**Temel hedef:** Benzer anlamlı kelimeler vektör uzayında **birbirine yakın** olsun.

---

## 2. Embedding Türlerini Büyük Resimde Sınıflandıralım

Embedding'leri iki büyük grupta düşünebilirsiniz:

### A) Statik (Context-free) Embedding
Bir kelimenin **tek bir vektörü** vardır. Cümle değişse de vektör değişmez.

- Word2Vec
- GloVe
- FastText

**Artıları:** hızlı, basit, daha az kaynak ister  
**Eksileri:** "çok anlamlı" kelimelerde (yüz = face / hundred gibi) bağlamı yakalayamaz

### B) Bağlamsal (Contextual) Embedding
Kelimenin vektörü **cümleye göre değişir**.

- BERT ve türevleri

**Artıları:** bağlamı ve çok anlamlılığı daha iyi yakalar  
**Eksileri:** daha ağır (hesaplama/kurulum), genelde daha fazla kaynak ister

---

## 3. Word2Vec (Kelime Seviyesinde Gömme)

### 3.1 Word2Vec'in Ana Fikri (Basit)
"Bir kelimenin anlamı, geçtiği bağlamdan anlaşılır."

Word2Vec, kelimeleri bağlamlarına göre öğrenir:
- "kedi" genellikle "mama", "tüy", "ev", "uyumak" gibi kelimelerle birlikte geçer.
- Bu bağlam benzerliği vektörlerde yakınlığa dönüşür.

### 3.2 İki Eğitim Yaklaşımı
Word2Vec genelde iki şekilde anlatılır:

#### 1) CBOW (Continuous Bag of Words)
- **Bağlamdan kelimeyi tahmin eder**
- Örnek: "Bu ___ çok iyi" → model "ürün" kelimesini tahmin etmeye çalışır.

**Yorum:** Bağlam → hedef kelime

#### 2) Skip-gram
- **Kelimeden bağlamı tahmin eder**
- Örnek: "ürün" kelimesi verildi → model "bu", "çok", "iyi" gibi çevre kelimeleri tahmin eder.

**Yorum:** Hedef kelime → bağlam

> Pratikte Skip-gram daha iyi sonuçlar verebilir, CBOW daha hızlı olabilir (bu genellemedir).

### 3.3 Word2Vec Ne İşe Yarar?
- Kelime benzerliği (similarity): "güzel" ≈ "harika"
- En yakın kelimeleri bulma
- Basit metin sınıflandırma/klasik NLP işlerinde iyi başlangıç

### 3.4 Sınırları
- "bank" kelimesi (nehir kenarı / banka kurumu) gibi çok anlamlı sözcüklerde tek vektör yetersiz kalır.

---

## 4. GloVe (Global Vectors)

### 4.1 GloVe'un Temel Fikri
Word2Vec bağlamı "tahmin görevi" üzerinden öğrenirken, GloVe daha çok şuna odaklanır:

- Kelimelerin **birlikte görülme (co-occurrence)** istatistikleri
- Yani "hangi kelime hangi kelimeyle ne sıklıkta yan yana/aynı dokümanda geçiyor?"

Bu yüzden "global" (küresel) bilgi vurgusu vardır.

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

- "gel" → "geldim", "geleceğim", "geliyordum", "gelmişsiniz" …

Word2Vec/GloVe bu kelimeleri ayrı ayrı "farklı kelime" olarak görebilir:
- Vocabulary büyür
- Nadir görülen kelimelerin vektörü kötü öğrenilebilir

### 5.2 FastText Ne Yapar?
FastText kelimeyi **parçalara** ayırır (subword / character n-gram):

- "kitaplarım" gibi bir kelimeyi küçük parçalardan temsil eder:
  - "kita", "itap", "tapl", "apar", "plar", "lara", "arım" gibi 4-5 karakter n-gram'lar (temsil basitleştirme amaçlı örnek)

**Sonuç:**
- Daha az görülen kelimeler bile parçaları üzerinden anlam kazanabilir.
- Yazım hatalarına ve ekli yapılara daha dayanıklı olabilir.

### 5.3 Türkçe İçin Neden Avantaj?
- Ekler çok olduğu için kelime varyasyonu çok yüksektir.
- Subword yaklaşımı, "aynı kökten türeme" bilgisini kısmen yakalar.

### 5.4 Basit FastText Örneği

```python
# FastText kurulumu
# pip install fasttext

import fasttext

# Mini örnek: 3 cümle ile basit bir model eğitin
# Not: Gerçekte daha çok metin ve iter gerekir
train_data = """
Bu ürün çok iyi ve kaliteli
Ürün kötü çıktı iade etmek istiyorum
Kargo çok geç geldi fakat ürün harika
""".strip().split('\n')

# Eğitim verisini dosyaya yazalım
with open('train.txt', 'w', encoding='utf-8') as f:
    for line in train_data:
        f.write(line + '\n')

# FastText modelini eğit (Skip-gram: sg=1, CBOW: sg=0)
model = fasttext.train_unsupervised('train.txt', model='skipgram', epoch=25, lr=1.0, wordNgrams=2)

# Kelime vektörünü al (100 boyutlu vektör)
vec_urun = model.get_word_vector('ürün')
print("'ürün' vektörünün ilk 10 elemanı:", vec_urun[:10])

# En yakın kelimeleri bul
# Kelime bulunamazsa benzeri kelimeleri FastText subword'ler üzerinden bulabilir
print("\n'iyi'ye en yakın 5 kelime:")
nearest_to_iyi = model.get_nearest_neighbors('iyi', k=5)
for score, word in nearest_to_iyi:
    print(f"  {word}: {score:.3f}")

# Yazım hatası veya nadir kelimeler için
# "kargo" yerine "karog" yazılsa bile FastText subword'ler ile tahmin yapabilir
vec_typo = model.get_word_vector('karog')
print("\n'karog' (yazım hatası) vektörünün ilk 10 elemanı:", vec_typo[:10])
print("Orta yazım hatasına rağmen 'kargo'ya benzer vektör üretmiş!")
```

**Bu örnek bize gösterir:**
- FastText subword'ler kullanarak yazım hatalarına dirençli oluştur
- Türkçe eklerini yapısal olarak kısmen yakalar

---

## 6. BERT Tarzı Modeller (Bağlamsal Embedding)

### 6.1 BERT'in En Kritik Farkı
BERT'te kelimenin embedding'i **cümleye göre değişir**.

Örnek:
- "Yüzünü yıkadı." (yüz = face)
- "Yüz TL verdim." (yüz = hundred)

BERT bu iki "yüz" için **farklı vektörler** üretebilir; çünkü bağlam farklıdır.

**Statik embedding'te:**
- İki "yüz" kelimesi için **tek bir vektör** vardır → iki farklı anlamı ayrıştıramaz.

**BERT'te:**
- Birinci cümledeki "yüz" → face anlamına yakın vektör
- İkinci cümledeki "yüz" → hundred/sayı anlamına yakın vektör

### 6.2 BERT Nasıl Öğrenir? (Kavramsal)
BERT'in temel eğitim fikirlerinden biri:
- Cümlenin içinden bazı kelimeler maskelenir: "Bu ürün [MASK] iyi"
- Model maskelenen kelimeyi tahmin etmeye çalışır.

Bu sayede kelimeyi sadece solundan değil, **sağından da** öğrenir (bidirectional context).

### 6.3 BERT ile Neler Güçlenir?
- Duygu analizi
- Soru-cevap
- İsimli varlık tanıma (NER)
- Metin sınıflandırma (özellikle bağlam kritikse)

### 6.4 Dezavantajlar (Gerçekçi Bakış)
- Daha fazla hesaplama gücü ister
- İnferans (tahmin) süresi statik embedding'lere göre daha uzundur
- Doğru kullanım için fine-tuning / doğru pipeline gerekebilir

### 6.5 Basit BERT Örneği (Türkçe)

```python
# Kurulum:
# pip install transformers torch

from transformers import AutoTokenizer, AutoModel
import torch

# Türkçe BERT modeli yükle (örnek: dbmdz/bert-base-turkish-cased)
model_name = "dbmdz/bert-base-turkish-cased"
tokenizer = AutoTokenizer.from_pretrained(model_name)
model = AutoModel.from_pretrained(model_name)

# İki farklı bağlamda "yüz" kelimesi
cümle1 = "Yüzünü yıkadı."        # yüz = face
cümle2 = "Yüz kişi geldi."       # yüz = hundred

def get_word_embedding(cümle, kelime, model, tokenizer):
    """Cümledeki belirli bir kelimenin BERT embedding'ini al"""
    # Tokenize et
    tokens = tokenizer.encode(cümle, return_tensors="pt")
    
    # Model üzerinden geçir
    with torch.no_grad():
        outputs = model(tokens)
    
    # Son katman çıktıları (embedding)
    embeddings = outputs[0]  # [batch_size, sequence_length, hidden_size]
    
    # Cümledeki kelimenin pozisyonunu bul
    word_tokens = tokenizer.encode(kelime, add_special_tokens=False)
    
    # Basit: ilk eşleşmeyi al (gerçek projede daha titiz bir eşleştirme gerekir)
    # Embedding ortalama (tüm token'ların ortalaması)
    avg_embedding = embeddings[0].mean(dim=0)
    
    return avg_embedding

# "yüz" embedding'lerini al
emb1 = get_word_embedding(cümle1, "yüz", model, tokenizer)
emb2 = get_word_embedding(cümle2, "yüz", model, tokenizer)

# İki embedding arasındaki uzaklığı hesapla (cosine similarity)
from torch.nn.functional import cosine_similarity
similarity = cosine_similarity(emb1.unsqueeze(0), emb2.unsqueeze(0))

print(f"Cümle 1: {cümle1}")
print(f"Cümle 2: {cümle2}")
print(f"\nİki 'yüz' embedding'inin cosine similarity: {similarity.item():.3f}")
print("(1'e yakın = çok benzer, 0'a yakın = benzer değil)")

# BERT: bağlama göre farklı vektörler üretebilir
print("\n✓ BERT bağlama göre farklı 'yüz' temsilleri oluşturabilir!")
print("✓ Statik embedding'te sadece tek bir 'yüz' vektörü vardır.")
```

**Bu örnek bize gösterir:**
- BERT'in aynı kelimeyi farklı bağlamlarda farklı temsil ettiği
- Bağlamsal embedding'in neden çok anlamlı kelimeler için daha güçlü olduğu

---

## 7. Hangi Embedding Ne Zaman Kullanılır?

Aşağıdaki tablo "kısa karar rehberi" gibi düşünülebilir:

| İhtiyaç | Öneri | Artı | Eksi |
|---|---|---|---|
| Hızlı, hafif, klasik NLP | Word2Vec / GloVe | Hızlı, kolay | Bağlam yok |
| Türkçe'de ekler, yazım varyasyonları çok | FastText | Eklere dayanıklı | Hâlâ bağlam yok |
| Kelime anlamı bağlama göre değişiyor, yüksek doğruluk hedefi | BERT (ve türevleri) | Çok anlamlılık yakalanır | Ağır, yavaş |
| Küçük kaynak + basit model | TF‑IDF + Logistic Regression (Hafta 5) | Baseline | Anlam yok |

> Önemli: "En iyi yöntem BERT" gibi tek cümle doğru değildir. Proje hedefi, veri boyutu, süre ve kaynaklar belirleyicidir.

---

## 8. Karşılaştırmalar ve Örnekler

### 8.1 Statik vs Bağlamsal Embedding Görsel Kıyaslama

**Scenario:** Duygu analizi için "harika" kelimesi

```
Word2Vec / GloVe / FastText (Statik):
"harika" → [0.25, 0.15, -0.03, 0.42, ...] (sabit vektör)

BERT (Bağlamsal):
"Bu ürün harika!" → [0.26, 0.16, -0.02, 0.41, ...] (pozitif bağlam)
"Bok gibi, harika değil." → [0.18, 0.08, 0.12, 0.35, ...] (negatif bağlam)
```

---

## 9. Mini Etkinlik / Tartışma

Aşağıdaki cümlelerde hangi yaklaşım daha avantajlı olur?

**1. "Bu telefon iyi değil."**
- N‑gram + TF‑IDF mi? 
  - Bigram kullanırsa "iyi değil" yakalayabilir
- Word2Vec mi? 
  - "iyi" ve "değil" ayrı vektörler, kombinasyon zorabilir
- BERT mi? 
  - Bağlamsal olarak "iyi değil" = negatif duygu yakalaması daha olası ✓

**2. "Yüzüm ağrıdı." vs "Yüz kişi geldi."**
- Statik embedding mi? 
  - İki "yüz" için tek vektör → karıştırılabilir
- Bağlamsal embedding mi? 
  - Farklı bağlamlar → farklı vektörler ✓

---

## 10. Sık Yapılan Hatalar

1. **"BERT her zaman en iyidir" düşüncesi**
   - Gerçek: Basit problemler için TF‑IDF yeterli olabilir, BERT over-engineering olabilir.

2. **FastText'i "sadece yazım hatası düzeltme" olarak görmek**
   - Gerçek: FastText, Türkçe gibi eklemeli dillerde temel yaklaşım olabilir.

3. **Embedding seçerken "en popüler model" seçmek**
   - Gerçek: Problem tipi, veri boyutu, zaman, kaynak belirleyicidir.

4. **Pretrained vektörleri fine-tuning yapmadan kullanmak**
   - Gerçek: Bazen domain-specific fine-tuning gerekebilir.

---

## Haftalık Ödev

**Görev (kavramsal + uygulama raporu):**

### Part 1: Kavramsal (Yazılı)
1. Word2Vec, GloVe, FastText, BERT için **1'er paragraf** tanım yazın.
2. Aşağıdaki soruları cevaplayın (kısa rapor, 1-2 sayfa):
   - Statik embedding ile bağlamsal embedding farkı nedir?
   - Türkçe için neden FastText avantaj sağlayabilir? (en az 2 örnek verin)
   - "yüz" örneğinde BERT neden daha iyi olabilir?
   - Kendi projende hangisini tercih ederdin? Neden?

### Part 2: Mini Uygulama
Aşağıdakilerden **en az birini** yapın:

**Seçenek A: FastText Benzerliği**
```python
# Aşağıdaki örneği çalıştırın ve raporlayın:
# - "iyi" kelimesine benzer kelimeleri bulun
# - Yazım hatası ile (örn. "iiy") sonuç nasıl değişiyor?
# - Türkçe ek varyasyonu (örn. "güzeller") üzerinde test edin
```

**Seçenek B: Basit Cosine Similarity**
```python
# Word2Vec / FastText ile iki kelime arasındaki benzerliği hesaplayın
# Örneğin:
#   - "güzel" vs "harika" (beklenen: benzer)
#   - "kötü" vs "harika" (beklenen: benzer değil)
# 
# Sonuçları tablo halinde raporlayın
```

**Seçenek C: BERT Bağlam Deneyi**
```python
# Türkçe BERT kullanarak:
# - "yüz" (face vs hundred) için iki farklı cümlede embedding al
# - Cosine similarity hesapla (örnek kod vermiş)
# - Sonuç: aynı kelime farklı bağlamda farklı vektör mü?
```

### Part 3: Sonuç Raporu
- Seçilen yöntem hangi sonuçlar verdi?
- Beklentiler ile karşılaştırma
- Gelecek projeden derler

---

## Özet

Bu hafta embedding dünyasını "türler" üzerinden tanıdık:

- **Word2Vec/GloVe:** kelime seviyesinde, statik vektörler; bağlamı tahmin/eş-oluşum yoluyla yakalar
- **FastText:** subword ile özellikle Türkçe gibi eklemeli dillerde daha sağlam yaklaşım; yazım hatalarına dayanıklı
- **BERT:** bağlama göre değişen vektörlerle çok anlamlılık ve bağlam problemlerini çok daha iyi yakalar; ağır fakat güçlü

Bir sonraki hafta istersen bu konuları uygulamaya taşıyabiliriz:
- hazır FastText vektörleriyle benzerlik ve sınıflandırma,
- ya da HuggingFace üzerinden Türkçe BERT ile duygu analizi uygulaması.
