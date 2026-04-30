# HAFTA 5: N‑gram Özellikler + Temel Metin Sınıflandırma (Duygu Analizi Örneği)

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 5 |
| **Süre** | 2 Saat |
| **Konu** | N‑gram (unigram/bigram/trigram) + TF‑IDF ile temel sınıflandırma (duygu analizi) |
| **Ön Koşul** | Hafta 4: BoW, TF‑IDF, temsil yöntemleri |
| **Amaç** | Metni vektöre çevirip basit bir ML modeliyle sınıflandırma yapmak |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- **N‑gram** kavramını (unigram/bigram/trigram) örnekle açıklayabilecektir.
- N‑gram’lerin **neden önemli** olduğunu (özellikle “iyi değil” gibi ifadeler) yorumlayabilecektir.
- `CountVectorizer` ve `TfidfVectorizer` içinde **ngram_range** kullanımını uygulayabilecektir.
- Basit bir metin sınıflandırma akışını kurabilecektir:  
  **Veri → Ön işleme → Vektörleştirme → Model → Değerlendirme**
- **Accuracy, Precision, Recall, F1** metriklerini temel düzeyde yorumlayabilecektir.
- Basit bir duygu analizi örneğinde model çıktısını tartışabilecektir.

---

## 1. N‑gram Nedir? (Çok Basit Anlatım)

**N‑gram**, metindeki art arda gelen **N tane kelimelik** parçadır.

- **1‑gram (unigram):** tek kelime  
  “çok iyi” → ["çok", "iyi"]
- **2‑gram (bigram):** 2’li kelime grubu  
  “çok iyi” → ["çok iyi"]
- **3‑gram (trigram):** 3’lü kelime grubu  
  “bu ürün iyi” → ["bu ürün iyi"]

### N‑gram neden lazım?
Çünkü bazı anlamlar **kelime grubu** halinde ortaya çıkar:

- “iyi” (pozitif)
- “iyi değil” (negatif)

Unigram kullanırsak “iyi değil” içinde “iyi” kelimesini görüp yanlış yorum yapabiliriz.  
Bigram kullanırsak “iyi değil” ifadesi tek özellik olur ve model bunu öğrenebilir.

---

## 2. BoW/TF‑IDF + N‑gram Birlikte Nasıl Çalışır?

Hafta 4’te:
- BoW → kelime sayımı
- TF‑IDF → ağırlıklı kelime önemi

Bu hafta:
- Aynı yöntemleri **kelime gruplarına** da uygularız.

Örnek:
- Unigram TF‑IDF: “iyi”, “kötü”
- Bigram TF‑IDF: “iyi değil”, “çok kötü”, “çok iyi”

> Pratikte en yaygın seçim: **TF‑IDF + (1,2) ngram_range**  
> (yani unigram + bigram)

---

## 3. Basit Metin Sınıflandırma Akışı (Dersin Ana Şeması)

### 3.1 Problem
Elimizde ürün yorumları var ve her yorumun etiketi var:

- `1` → pozitif
- `0` → negatif

### 3.2 Akış
1. **Metni al**
2. **Temizle / ön işle**
3. **Vektöre çevir (TF‑IDF, n‑gram)**
4. **Model seç (Lojistik Regresyon gibi)**
5. **Eğit**
6. **Test et**
7. **Metrikleri incele**

---

## 4. Değerlendirme Metrikleri (Basit Tanım)

Bir sınıflandırma modelini sadece “kaçını doğru bildi?” ile değerlendirmek bazen yetmez.

- **Accuracy:** Toplam doğruluk oranı  
- **Precision:** Model “pozitif” dediğinde ne kadar doğru?
- **Recall:** Gerçek pozitiflerin ne kadarını yakaladı?
- **F1:** Precision ve Recall’un dengeli ortalaması

> Duygu analizinde (özellikle dengesiz veri varsa) F1 genelde daha anlamlıdır.

---

## 5. Python Uygulaması (Basit ve Bol Açıklamalı)

Aşağıdaki örnek “mini veri” ile uçtan uca sınıflandırma gösterir.  
Gerçek projelerde veri daha büyük olur ama mantık aynı.

### 5.1 Kurulum
```bash
pip install scikit-learn pandas
```

### 5.2 Örnek Kod: TF‑IDF (Unigram+Bigram) + Lojistik Regresyon

```python
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import classification_report, confusion_matrix, accuracy_score

# -----------------------------
# 1) Mini veri seti (örnek)
# -----------------------------
data = [
    ("Bu ürün çok iyi, tavsiye ederim", 1),
    ("Kalitesi harika, çok memnun kaldım", 1),
    ("Kargo hızlı geldi, ürün güzel", 1),
    ("Ürün kötü çıktı, hiç beğenmedim", 0),
    ("Çok geç geldi ve kalitesi kötü", 0),
    ("İade süreci berbat, pişman oldum", 0),
    ("İyi değil, beklentimi karşılamadı", 0),
    ("Fiyatına göre çok iyi", 1),
    ("Kesinlikle tavsiye etmiyorum, kötü", 0),
    ("Mükemmel ürün, çok başarılı", 1),
]

df = pd.DataFrame(data, columns=["text", "label"])

# -----------------------------
# 2) Train/Test ayırma
# -----------------------------
X_train, X_test, y_train, y_test = train_test_split(
    df["text"], df["label"], test_size=0.3, random_state=42, stratify=df["label"]
)

# -----------------------------
# 3) TF-IDF vektörleştirme
#    ngram_range=(1,2) => unigram + bigram
# -----------------------------
vectorizer = TfidfVectorizer(
    lowercase=True,
    ngram_range=(1, 2)  # bu satır dersin ana konusu
)

X_train_vec = vectorizer.fit_transform(X_train)
X_test_vec = vectorizer.transform(X_test)

# -----------------------------
# 4) Model: Logistic Regression
# -----------------------------
model = LogisticRegression(max_iter=1000)
model.fit(X_train_vec, y_train)

# -----------------------------
# 5) Tahmin ve değerlendirme
# -----------------------------
y_pred = model.predict(X_test_vec)

print("Accuracy:", accuracy_score(y_test, y_pred))
print("\nConfusion Matrix:\n", confusion_matrix(y_test, y_pred))
print("\nClassification Report:\n", classification_report(y_test, y_pred))

# -----------------------------
# 6) En önemli özelliklere bakma (yorumlama)
#    Model hangi kelimeleri pozitif/negatif görüyor?
# -----------------------------
feature_names = vectorizer.get_feature_names_out()
coef = model.coef_[0]

top_pos_idx = coef.argsort()[-10:]  # pozitif tarafa itenler
top_neg_idx = coef.argsort()[:10]   # negatif tarafa itenler

print("\nPozitif sınıfa iten en güçlü 10 özellik:")
for i in reversed(top_pos_idx):
    print(f"{feature_names[i]} -> {coef[i]:.3f}")

print("\nNegatif sınıfa iten en güçlü 10 özellik:")
for i in top_neg_idx:
    print(f"{feature_names[i]} -> {coef[i]:.3f}")
```

### 5.3 Bu Kodun Öğrettiği Şey Ne?

- `ngram_range=(1,2)` sayesinde model hem **tek kelimeler** hem de **ikili ifadeler** görür.
- Modelin katsayılarıyla hangi kelimelerin/ifadelerin pozitif/negatif etkilediğini anlayabiliriz.
- Bu, “modeli yorumlama” için çok faydalıdır.

---

## 6. N‑gram Seçerken Pratik İpuçları

- **Sadece unigram**: hızlı ve basit ama bağlam zayıf olabilir.
- **Unigram + bigram**: çoğu zaman en iyi “denge” (performans vs. boyut).
- **Trigram**: veri büyüdükçe anlamlı olabilir; küçük veride çok seyrek kalabilir.

> N‑gram arttıkça özellik sayısı artar → model daha zor öğrenebilir ve bellek maliyeti büyür.

---

## 7. Sık Yapılan Hatalar

1. **Eğitim verisine fit edip testte yeniden fit etmek**  
   - Doğrusu: `fit_transform` sadece train’de, `transform` testte.

2. **Çok küçük veriyle kesin sonuç beklemek**  
   - Mini veri eğitim amaçlıdır; gerçek performans büyük veride anlaşılır.

3. **Stopword’leri yanlış yönetmek**
   - “değil” gibi kelimeleri silmek duygu analizinde ciddi hata yaptırır.

4. **Veri dengesizliğini görmezden gelmek**
   - Çok fazla pozitif, az negatif varsa accuracy yanıltabilir.

---

## 8. Sınıf İçi Mini Etkinlik

Aşağıdaki iki cümlede unigram ve bigram farkını tartışın:

1. “Ürün iyi değil”
2. “Ürün iyi”

- Unigram ile hangi kelime ortak?
- Bigram ile hangi ifade ayrıştırıcı?

---

## Haftalık Ödev

**Görev:**
1. En az **50 Türkçe yorum** toplayın (pozitif/negatif etiketli veya siz etiketleyin).
2. İki farklı vektörleştirme deneyin:
   - TF‑IDF `ngram_range=(1,1)`
   - TF‑IDF `ngram_range=(1,2)`
3. Aynı modelle (Logistic Regression) iki sonucu kıyaslayın:
   - Accuracy
   - F1-score (macro veya weighted)
4. 1 sayfalık raporda cevaplayın:
   - Bigram eklemek performansı artırdı mı? Neden?
   - Modelin “negatif” için en güçlü 5 özelliği ne çıktı?
   - “değil” kelimesi silinseydi ne olurdu?

---

## Özet

Bu hafta metin temsilini bir adım ileri taşıdık:
- N‑gram ile kelime gruplarını özellik yaptık,
- TF‑IDF ile bu özellikleri ağırlıklandırdık,
- Basit bir sınıflandırma modeliyle duygu analizi akışını kurduk,
- Modelin çıktısını metriklerle değerlendirmeyi ve basitçe yorumlamayı öğrendik.

Bir sonraki hafta genelde **kümeleme (clustering)** veya **konu modelleme (topic modeling)** gibi “etiketsiz” yöntemlere geçilir.