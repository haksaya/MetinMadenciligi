# HAFTA 4: Metin Temsil Yöntemleri (Bag of Words, TF‑IDF, Word Embeddings)

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 4 |
| **Süre** | 2 Saat |
| **Konu** | Metin Temsil Yöntemleri: BoW, TF‑IDF, Word Embeddings |
| **Ön Koşul** | Hafta 1–3: Temel kavramlar + ön işleme + Python/NLTK |
| **Kaynak Kitap** | Uygulamalı Metin Madenciliği: Doğal Dil İşleme ve Python ile |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Metinlerin neden sayısallaştırılması gerektiğini açıklayabilecektir.
- **Bag of Words (BoW)** yaklaşımını kurup yorumlayabilecektir.
- **TF‑IDF** mantığını (TF ve IDF parçalarıyla) basit örnekle anlatabilecektir.
- BoW ile TF‑IDF arasındaki farkları hangi durumda hangisinin tercih edileceğiyle birlikte tartışabilecektir.
- **Word Embedding** kavramını, “anlamı vektöre taşıma” fikri üzerinden açıklayabilecektir.
- Python’da küçük bir veri üzerinde BoW/TF‑IDF çıkarımı yapabilecektir.

---

## 1. Neden Metinleri Sayısallaştırıyoruz?

Bilgisayarlar metni “anlam” olarak görmez; matematiksel işlemler yapabilmek için metnin **sayılara** çevrilmesi gerekir.

- İnsan için: “Bu ürün harika!” → net bir duygu taşır.
- Bilgisayar için: Bu cümle sadece karakterlerden oluşan bir dizidir.
- Bu yüzden metni **temsil (representation)** ederek bir **vektöre** dönüştürürüz.

Bu dönüşümün hedefi:
- Makine öğrenmesi modellerinin kullanabileceği bir **özellik matrisi** üretmek (X)
- Eğer etiket varsa (pozitif/negatif gibi), hedef değişkeni (y) ile birlikte model eğitmek

---

## 2. Temel Kavram: Vocabulary (Sözlük)

Bir veri setindeki tüm kelimelerin (veya tokenların) benzersiz listesine genellikle **vocabulary** denir.

Örnek mini korpus (3 doküman):

1. “bu ürün çok iyi”
2. “bu ürün kötü”
3. “kargo çok geç”

Bu korpustan çıkan (örnek) vocabulary:
- [bu, ürün, çok, iyi, kötü, kargo, geç]

> Not: Gerçek projede önce **ön işleme** (küçük harf, noktalama temizliği, stopword vb.) yapılır.

---

## 3. Bag of Words (BoW) Nedir?

### 3.1 Tanım (Basit)
**Bag of Words**, metni “kelime torbası” gibi düşünür:
- Kelimelerin **sırası** önemli değildir.
- Sadece “hangi kelime kaç kez geçti?” bilgisi tutulur.

Bu yaklaşım genelde iki şekilde uygulanır:
1. **Count Vectorizer (Kelime sayımı):** Her kelimenin frekansı
2. **Binary BoW:** Kelime var mı/yok mu (0/1)

### 3.2 BoW’un Güçlü ve Zayıf Yanları
**Avantajlar**
- Uygulaması kolaydır.
- Hızlıdır.
- Küçük veri setlerinde iyi bir başlangıç yöntemidir.

**Dezavantajlar**
- Kelime sırasını kaybeder:  
  “iyi değil” ile “iyi” kelimesi aynı vektörde “iyi”yi taşır, anlam tersine dönebilir.
- Anlam ilişkilerini bilmez: “güzel” ve “harika” birbirine yakın sayılmaz.
- Vocabulary büyüdükçe boyut çok artar (seyrek/sparse matris).

---

## 4. TF‑IDF Nedir?

### 4.1 Neden TF‑IDF?
BoW, “bu”, “ve”, “çok” gibi sık kelimelere de yüksek değer verebilir.  
TF‑IDF ise şunu yapar:

- Bir kelime **dokümanda sık** geçiyorsa önemli olabilir (**TF**)
- Ama kelime **her dokümanda** geçiyorsa ayırt edici değildir (**IDF** düşürür)

### 4.2 TF ve IDF Mantığı (Sınavlık Kısa Açıklama)

- **TF (Term Frequency):** Kelimenin dokümandaki frekansı (kaç kez geçtiği)
- **IDF (Inverse Document Frequency):** Kelimenin tüm dokümanlar içinde ne kadar “nadir” olduğu  
  Nadir kelime → daha ayırt edici → daha yüksek IDF

Basit yorum:
- “ürün” her yorumda geçiyorsa ayırt edici değildir → TF‑IDF düşük olur.
- “bozuldu”, “iade”, “mükemmel” gibi kelimeler bazı yorumlarda geçer → daha ayırt edicidir → TF‑IDF yüksek olabilir.

### 4.3 TF‑IDF’in Güçlü ve Zayıf Yanları
**Avantajlar**
- Ayırt edici kelimeleri daha iyi öne çıkarır.
- BoW’a göre birçok sınıflandırma probleminde daha iyi başlangıç verir.

**Dezavantajlar**
- Hâlâ sıralamayı ve bağlamı bilmez.
- “güzel” ve “harika”nın yakınlığını doğrudan vermez.

---

## 5. Word Embeddings (Kelime Gömmeleri) Nedir?

### 5.1 Temel Fikir
Word Embedding, kelimeyi “tek bir sayı sayımı” gibi değil, **anlam taşıyan yoğun (dense) bir vektör** olarak temsil etmeye çalışır.

Örneğin:
- “kral” ve “kraliçe” vektörleri birbirine yakın olabilir.
- “Ankara” ve “İstanbul” (şehir isimleri) benzer bir bağlamda geçtiği için yakın olabilir.

### 5.2 BoW/TF‑IDF ile Embedding Arasındaki Basit Fark
- BoW/TF‑IDF: “Kelime geçti mi, kaç kez geçti?” (istatistiksel ağırlık)
- Embedding: “Kelime ne anlama geliyor, hangi kelimelere benziyor?” (dağıtımsal anlam)

### 5.3 Embedding Türleri (Bu derste kavramsal)
- **Word2Vec / GloVe:** Kelime seviyesinde gömme
- **FastText:** Alt-kelime (subword) kullandığı için Türkçe gibi eklemeli dillerde avantaj sağlayabilir
- **BERT tarzı modeller:** Bağlama göre embedding üretir (aynı kelime farklı cümlede farklı vektör)

> Bu hafta hedef: Embedding’in mantığını anlamak. Uygulamalı derin embedding çalışmasını ilerleyen haftaya bırakacağız.

---

## 6. Python Uygulaması: BoW ve TF‑IDF (Basit ve Açıklamalı)

Aşağıdaki örnek, küçük bir Türkçe metin listesi üzerinde BoW ve TF‑IDF çıkarır.

> Not: Bu örnek için `scikit-learn` gerekir.

### 6.1 Kurulum
```bash
pip install scikit-learn
```

### 6.2 Örnek Kod (BoW + TF‑IDF)

```python
# sklearn içinden metni sayısal vektörlere çevirmek için kullanılan iki aracı içe aktarır:
# - CountVectorizer: Bag of Words (kelime sayımı) üretir
# - TfidfVectorizer: TF-IDF ağırlıklı kelime matrisi üretir
from sklearn.feature_extraction.text import CountVectorizer, TfidfVectorizer

# Sonuçları tablo (DataFrame) şeklinde rahat görmek için pandas'ı içe aktarır
import pandas as pd

# 6 adet örnek metin (doküman) listesi oluşturur.
# Her bir string, tek bir "doküman" gibi düşünülür (ürün yorumu vb.)
docs = [
    "Bu ürün çok iyi ve kaliteli",              # 1. doküman
    "Ürün kötü çıktı iade etmek istiyorum",     # 2. doküman
    "Kargo çok geç geldi",                      # 3. doküman
    "Kalitesi çok iyi ama fiyatı yüksek",       # 4. doküman
    "İade süreci çok zor",                      # 5. doküman
    "Bu ürün harika çok memnunum"               # 6. doküman
]

# -----------------------
# 1) Bag of Words (Count)
# -----------------------

# CountVectorizer nesnesi oluşturur.
# lowercase=True: metinleri otomatik olarak küçük harfe çevirir (ör: "Bu" -> "bu").
# Ama dikkat: Türkçe 'İ/I' dönüşümlerinde bazen beklenmedik sonuçlar olabilir.
count_vec = CountVectorizer(lowercase=True)

# fit_transform iki işi birden yapar:
# 1) fit: docs içindeki tüm kelimelerden sözlük (vocabulary) çıkarır
# 2) transform: her dokümanı bu sözlüğe göre sayısal vektöre çevirir (kelime sayımı)
# X_count sonucu genelde "sparse matrix"tir (seyrek matris), çünkü çoğu hücre 0 olur.
X_count = count_vec.fit_transform(docs)

# Oluşturulan sözlükteki kelimeleri (özellik adlarını) alır.
vocab_count = count_vec.get_feature_names_out()

# Seyrek matrisi normal dense (tam) matrise çevirir: toarray()
# Sonra bu matrisi kolon isimleri vocab_count olacak şekilde DataFrame'e çevirir.
df_count = pd.DataFrame(X_count.toarray(), columns=vocab_count)

# Başlık yazdırır
print("=== BoW (Count) Matrisi ===")

# BoW sayım matrisini yazdırır.
print(df_count)

# -----------------------
# 2) TF-IDF
# -----------------------

# TfidfVectorizer nesnesi oluşturur.
# Varsayılan ayarlarla kelime token’larını çıkarır ve TF-IDF hesaplar.
tfidf_vec = TfidfVectorizer(lowercase=True)

# fit_transform: sözlüğü öğrenir ve dokümanları TF-IDF vektörlerine çevirir.
X_tfidf = tfidf_vec.fit_transform(docs)

# TF-IDF sözlüğündeki kelimeleri (özellik adlarını) alır.
vocab_tfidf = tfidf_vec.get_feature_names_out()

# TF-IDF matrisini dense matrise çevirir ve DataFrame'e aktarır.
df_tfidf = pd.DataFrame(X_tfidf.toarray(), columns=vocab_tfidf)

# TF-IDF başlığı yazdırır.
print("\n=== TF-IDF Matrisi (Yuvarlatılmış) ===")

# TF-IDF değerlerini daha okunur yapmak için 2 basamağa yuvarlar ve yazdırır.
print(df_tfidf.round(2))
```

---

## 7. Sık Yapılan Hatalar / Dikkat Edilecek Noktalar

1. **Ön işleme yapmadan temsil çıkarmak**  
   Büyük/küçük harf, noktalama ve yazım farkları vocabulary’yi şişirir.

2. **Stopword listesini körü körüne uygulamak**  
   Duygu analizinde “çok” gibi kelimeler önemli olabilir.

3. **Aşırı büyük vocabulary**  
   Çok seyrek (sparse) matris oluşur; model ve bellek maliyeti artar.

4. **BoW/TF‑IDF ile bağlam beklemek**  
   Bu yöntemler kelime sırası ve bağlamı taşımaz; bunun için embedding/transformer gerekir.

---

## 8. Mini Tartışma Soruları (Sınıf İçinde)

- BoW ile TF‑IDF arasında seçim yaparken hangi kriterlere bakarsınız?
- “iyi değil” ifadesinde BoW neden zorlanır? Ne tür ek özellikler eklenebilir? (n‑gram gibi)
- Türkçe’de eklerden dolayı vocabulary büyümesini azaltmak için ne yapılabilir? (lemmatization, subword, FastText)

---

## Haftalık Ödev

**Görev:**
1. En az 30 Türkçe kısa metin toplayın (yorum, tweet, haber başlığı vb.)
2. Basit bir ön işleme yapın (küçük harf + noktalama temizliği)
3. Aynı veri üzerinde:
   - BoW (CountVectorizer) çıkarın
   - TF‑IDF çıkarın
4. Aşağıdaki soruları 1 sayfalık kısa raporda cevaplayın:
   - TF‑IDF’de hangi kelimeler daha yüksek çıktı, neden?
   - BoW ile TF‑IDF sonuçları arasında hangi farkları gözlemlediniz?
   - Bu temsilleri hangi problemde kullanmak istersiniz? (sınıflandırma, kümeleme vb.)

---

## Özet

Bu derste metinlerin makine öğrenmesiyle çalışabilmesi için sayısallaştırılması gerektiğini gördük. Bag of Words yaklaşımıyla kelime frekansına dayalı temsil ürettik, TF‑IDF ile ayırt edici kelimeleri daha fazla ağırlıklandırmayı öğrendik. Son olarak Word Embedding kavramıyla “kelimenin anlamını vektöre taşıma” fikrine giriş yaptık. Bir sonraki haftalarda bu temsillerle sınıflandırma/kümeleme gibi modelleri daha sistematik şekilde kuracağız.
