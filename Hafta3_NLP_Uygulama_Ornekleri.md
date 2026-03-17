
# HAFTA 3: Doğal Dil İşleme (NLP) Uygulama Örnekleri

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 3 |
| **Süre** | 2 Saat |
| **Konu** | NLP Temel Kavramlarının Python ile Uygulaması |
| **Ön Koşul** | Python temel bilgisi, NLTK kurulumu |
| **Kaynak Kitap** | Uygulamalı Metin Madenciliği: Doğal Dil İşleme ve Python ile |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Python ortamında NLTK ve Zemberek kütüphanelerini kullanabilecektir.
- Tokenizasyon, stopword çıkarma, stemming ve lemmatization işlemlerini kod ile gerçekleştirebilecektir.
- N-gram oluşturma ve kelime frekans analizi yapabilecektir.
- Varlık Adı Tanıma (NER) uygulaması gerçekleştirebilecektir.
- Gerçek bir veri seti üzerinde uçtan uca ön işleme pipeline'ı kurabilecektir.

---

## Gerekli Kurulumlar

Uygulamalara başlamadan önce aşağıdaki kütüphanelerin kurulu olması gerekmektedir. Terminalde veya Anaconda Prompt'ta şu komutları çalıştırın:

```bash
pip install nltk
pip install pandas
pip install matplotlib
pip install wordcloud
pip install zeyrek
pip install spacy
python -m spacy download en_core_web_sm
```

**Açıklama:**
- **nltk:** Doğal dil işleme için temel Python kütüphanesidir. Tokenizasyon, stemming, stopword listesi gibi birçok araç barındırır.
- **pandas:** Veri analizi ve tablo işlemleri için kullanılır. Veri setlerini okumak ve işlemek için temel kütüphanedir.
- **matplotlib:** Grafik ve görselleştirme kütüphanesidir. Kelime frekanslarını çubuk grafik olarak göstermek için kullanacağız.
- **wordcloud:** Kelime bulutu oluşturmak için kullanılır. Bir metindeki en sık geçen kelimeleri görsel olarak sunar.
- **zeyrek:** Zemberek'in Python arayüzüdür. Türkçe morfolojik analiz ve lemmatization işlemleri için kullanacağız.
- **spacy:** Endüstriyel düzeyde NLP kütüphanesidir. Varlık adı tanıma (NER) uygulaması için kullanacağız.

NLTK'nin alt bileşenlerini de indirmemiz gerekiyor. Python ortamında şu komutu çalıştırın:

```python
import nltk
nltk.download('punkt')
nltk.download('punkt_tab')
nltk.download('stopwords')
nltk.download('averaged_perceptron_tagger')
nltk.download('wordnet')
```

**Açıklama:** NLTK modüler bir yapıdadır. Her bir alt bileşen ayrı olarak indirilir. `punkt` tokenizasyon için, `stopwords` etkisiz kelime listesi için, `averaged_perceptron_tagger` sözcük türü etiketleme için, `wordnet` ise İngilizce lemmatization için gereken sözlük veritabanıdır.

---

## Uygulama 1: Tokenizasyon

### 1.1 Teori Hatırlatması

Tokenizasyon, bir metni daha küçük parçalara (tokenlara) ayırma işlemidir. Bu parçalar kelimeler, cümleler veya alt-kelimeler olabilir. Tokenizasyon, tüm NLP pipeline'larının ilk ve en temel adımıdır. Eğer tokenizasyon hatalı yapılırsa, sonraki tüm adımlar bundan olumsuz etkilenir.

### 1.2 Kelime Tokenizasyonu (Word Tokenization)

```python
from nltk.tokenize import word_tokenize

# Örnek Türkçe metin
metin = "Bugün Ankara'da hava çok güzel ama trafik yorucuydu. Yarın İstanbul'a gideceğim."

# Kelime tokenizasyonu
tokenlar = word_tokenize(metin)

print("Orijinal metin:")
print(metin)
print("\nTokenlar:")
print(tokenlar)
print(f"\nToplam token sayısı: {len(tokenlar)}")
```

**Beklenen Çıktı:**
```
Orijinal metin:
Bugün Ankara'da hava çok güzel ama trafik yorucuydu. Yarın İstanbul'a gideceğim.

Tokenlar:
['Bugün', 'Ankara', "'", 'da', 'hava', 'çok', 'güzel', 'ama', 'trafik',
 'yorucuydu', '.', 'Yarın', 'İstanbul', "'", 'a', 'gideceğim', '.']

Toplam token sayısı: 17
```

**Detaylı Açıklama:**

Bu çıktıda dikkat edilmesi gereken birkaç önemli nokta vardır:

1. **Noktalama ayrımı:** Cümle sonundaki nokta (.) ayrı bir token olarak ayrılmıştır. Bu genellikle istenen bir davranıştır çünkü nokta, kelimenin kendisinin parçası değildir.

2. **Apostrof sorunu:** "Ankara'da" ifadesi üç ayrı tokena bölünmüştür: "Ankara", "'", "da". NLTK'nin İngilizce tabanlı tokenizer'ı Türkçe apostrofu İngilizce'deki sahiplik eki gibi algılamaktadır. Bu durum, Türkçe NLP'nin zorluklarından birini göstermektedir.

3. **Token sayısı:** Orijinal metinde gözle saydığımızda yaklaşık 11 kelime varken, tokenizer 17 token üretmiştir. Bu fark, noktalama işaretlerinin ve apostrof ayrımının sonucudur.

### 1.3 Cümle Tokenizasyonu (Sentence Tokenization)

```python
from nltk.tokenize import sent_tokenize

metin = """Metin madenciliği günümüzde çok önemli bir alandır. Özellikle sosyal medya
verilerinin analizi büyük ilgi görmektedir. Şirketler müşteri yorumlarını analiz
ederek ürünlerini geliştirmektedir. Akademik alanda da metin madenciliği
çalışmaları hızla artmaktadır."""

cumleler = sent_tokenize(metin)

print("Cümle sayısı:", len(cumleler))
print("\nCümleler:")
for i, cumle in enumerate(cumleler, 1):
    print(f"  Cümle {i}: {cumle.strip()}")
```

**Beklenen Çıktı:**
```
Cümle sayısı: 4

Cümleler:
  Cümle 1: Metin madenciliği günümüzde çok önemli bir alandır.
  Cümle 2: Özellikle sosyal medya verilerinin analizi büyük ilgi görmektedir.
  Cümle 3: Şirketler müşteri yorumlarını analiz ederek ürünlerini geliştirmektedir.
  Cümle 4: Akademik alanda da metin madenciliği çalışmaları hızla artmaktadır.
```

**Detaylı Açıklama:**

Cümle tokenizasyonu, metni anlamlı cümle birimlerine ayırır. `sent_tokenize` fonksiyonu nokta, ünlem ve soru işareti gibi cümle sonu işaretlerini baz alarak ayırma yapar.

Ancak cümle tokenizasyonu her zaman mükemmel çalışmaz. Örneğin "Dr. Ahmet" ifadesindeki noktayı cümle sonu olarak algılayabilir veya kısaltmalarda hata yapabilir. Bu tür durumlar için özelleştirilmiş tokenizer'lar gerekebilir.

### 1.4 Türkçe İçin Basit Özel Tokenizer

NLTK'nin varsayılan tokenizer'ı Türkçe'de apostrof sorunları yarattığı için, basit bir özel tokenizer yazmak faydalı olabilir:

```python
import re

def turkce_tokenize(metin):
    """
    Türkçe metinler için basit bir tokenizer.
    Apostroflu kelimeleri (Ankara'da, İstanbul'un vb.) tek parça olarak tutar.
    Noktalama işaretlerini ayrı token olarak ayırır.
    """
    # Apostroflu kelimeleri koruyarak tokenize et
    # \w+ : bir veya daha fazla harf/rakam
    # (?:['\u2019]\w+)* : ardından apostrof ve harf geliyorsa onları da dahil et
    tokenlar = re.findall(r"\w+(?:['\u2019]\w+)*", metin)
    return tokenlar

metin = "Bugün Ankara'da hava çok güzel ama trafik yorucuydu. Yarın İstanbul'a gideceğim."
tokenlar = turkce_tokenize(metin)

print("Özel tokenizer sonucu:")
print(tokenlar)
print(f"Token sayısı: {len(tokenlar)}")
```

**Beklenen Çıktı:**
```
Özel tokenizer sonucu:
['Bugün', "Ankara'da", 'hava', 'çok', 'güzel', 'ama', 'trafik', 'yorucuydu',
 'Yarın', "İstanbul'a", 'gideceğim']
Token sayısı: 11
```

**Detaylı Açıklama:**

Bu özel tokenizer'da `re.findall()` fonksiyonu ile düzenli ifade (regex) kullanıyoruz. Regex deseni şu şekilde çalışır:

- `\w+` : Bir veya daha fazla harf, rakam veya alt çizgi karakterini yakalar. Bu, kelimenin ana gövdesini oluşturur.
- `(?:['\u2019]\w+)*` : Eğer kelimenin ardından bir apostrof ve daha fazla harf geliyorsa, bunları da aynı tokenın parçası olarak dahil eder. `\u2019` karakteri tipografik apostrof (') için kullanılır.

Bu sayede "Ankara'da" tek bir token olarak kalır ve Türkçe'nin yapısına daha uygun bir tokenizasyon elde edilir. Ayrıca noktalama işaretleri otomatik olarak düşer çünkü regex deseni sadece harf ve rakamları yakalar.

---

## Uygulama 2: Stopword (Etkisiz Kelime) Çıkarma

### 2.1 Teori Hatırlatması

Stopword'ler, bir dilde çok sık kullanılan ancak genellikle metin analizi açısından ayırt edici bilgi taşımayan kelimelerdir. "ve", "bir", "bu", "için", "ile" gibi kelimeler Türkçe'deki tipik stopword örnekleridir. Bu kelimeleri çıkarmak, veri boyutunu küçültür ve modelin gerçekten anlamlı kelimelere odaklanmasını sağlar.

### 2.2 NLTK Stopword Listesi

```python
from nltk.corpus import stopwords

# Türkçe stopword listesini yükle
turkce_stopwords = set(stopwords.words('turkish'))

print(f"NLTK Türkçe stopword sayısı: {len(turkce_stopwords)}")
print("\nİlk 30 stopword (alfabetik):")
print(sorted(turkce_stopwords)[:30])
```

**Beklenen Çıktı:**
```
NLTK Türkçe stopword sayısı: 53

İlk 30 stopword (alfabetik):
['acaba', 'ama', 'aslında', 'az', 'bazı', 'belki', 'beni', 'bile', 'bir',
 'birçok', 'biri', 'birkaç', 'birşey', 'biz', 'bize', 'bizden', 'bizi',
 'bizim', 'bu', 'buna', 'bunda', 'bundan', 'bunlar', 'bunları', 'bunların',
 'bunu', 'bunun', 'burada', 'çok', 'çünkü']
```

**Detaylı Açıklama:**

NLTK'nin Türkçe stopword listesi 53 kelime içerir. Bu sayı İngilizce'ye (179) kıyasla oldukça azdır. Bunun nedeni, NLTK'nin Türkçe desteğinin İngilizce kadar kapsamlı olmamasıdır. Pratikte bu listeyi genişletmek gerekebilir.

### 2.3 Genişletilmiş Türkçe Stopword Listesi ve Uygulama

```python
# NLTK listesini genişletilmiş özel liste ile birleştirme
ek_stopwords = {
    'da', 'de', 'mi', 'mu', 'mü', 'mı', 'ki', 'ile',
    'için', 'gibi', 'kadar', 'sonra', 'önce', 'daha',
    'en', 'her', 'hiç', 'hem', 'ya', 'ne', 'nasıl',
    'neden', 'şu', 'o', 'ben', 'sen', 'biz', 'siz',
    'onlar', 'olan', 'olarak', 'var', 'yok', 'ise',
    'değil', 'diğer', 'diye', 'göre', 'veya', 'ancak',
    'fakat', 'lakin', 'hatta', 'üzere', 'rağmen',
    'karşı', 'ayrıca', 'böyle', 'şöyle', 'öyle'
}

# İki listeyi birleştir
tum_stopwords = turkce_stopwords.union(ek_stopwords)
print(f"Genişletilmiş stopword sayısı: {len(tum_stopwords)}")

# Örnek metin üzerinde uygulama
metin = "Bu ürün gerçekten çok iyi bir ürün ama fiyatı biraz yüksek olabilir diye düşünüyorum"
kelimeler = metin.lower().split()

# Stopword olmayan kelimeleri filtrele
temiz_kelimeler = [kelime for kelime in kelimeler if kelime not in tum_stopwords]

print(f"\nOrijinal metin ({len(kelimeler)} kelime):")
print(metin)
print(f"\nStopword çıkarma sonrası ({len(temiz_kelimeler)} kelime):")
print(" ".join(temiz_kelimeler))
print(f"\nÇıkarılan stopword'ler:")
cikarilan = [kelime for kelime in kelimeler if kelime in tum_stopwords]
print(cikarilan)
```

**Beklenen Çıktı:**
```
Genişletilmiş stopword sayısı: 85

Orijinal metin (14 kelime):
Bu ürün gerçekten çok iyi bir ürün ama fiyatı biraz yüksek olabilir diye düşünüyorum

Stopword çıkarma sonrası (7 kelime):
ürün gerçekten iyi ürün fiyatı biraz yüksek

Çıkarılan stopword'ler:
['bu', 'çok', 'bir', 'ama', 'olabilir', 'diye', 'düşünüyorum']
```

**Detaylı Açıklama:**

Bu örnekte birkaç önemli nokta vardır:

1. **`metin.lower()`** ile tüm metin küçük harfe çevrilir. Bu, "Bu" ve "bu" gibi aynı kelimenin farklı yazımlarının eşleşmesini sağlar.

2. **List comprehension** kullanılarak stopword olmayan kelimeler filtrelenir. `[kelime for kelime in kelimeler if kelime not in tum_stopwords]` ifadesi, her kelimeyi kontrol eder ve stopword listesinde olmayanları yeni listeye ekler.

3. **"çok" kelimesinin çıkarılması tartışmalıdır.** "Çok iyi" ifadesinde "çok" bir yoğunluk belirteci olarak anlam taşır. Duygu analizi yapıyorsak "çok" kelimesini çıkarmak bilgi kaybına yol açabilir. Bu yüzden stopword listesi her zaman probleme göre özelleştirilmelidir.

4. **"düşünüyorum" kelimesi** burada stopword olarak çıkarılmamıştır çünkü listede yoktur. Ancak bazı uygulamalarda fiiller de stopword olarak kabul edilebilir.

---

## Uygulama 3: Stemming ve Lemmatization

### 3.1 Teori Hatırlatması

**Stemming:** Kelimenin eklerini basit kurallara göre kırparak kök formuna yakın bir sonuç üretir. Hızlıdır ancak her zaman gerçek bir kelime üretmeyebilir.

**Lemmatization:** Kelimenin dilbilgisi kurallarına ve sözlük bilgisine dayanarak doğru kök formunu (lemma) bulur. Daha yavaştır ancak daha doğru sonuç verir.

### 3.2 İngilizce Stemming Örneği (Porter Stemmer)

Stemming kavramını anlamak için önce İngilizce üzerinde deneyelim:

```python
from nltk.stem import PorterStemmer

stemmer = PorterStemmer()

ingilizce_kelimeler = [
    "running", "runs", "ran", "runner",
    "happily", "happiness", "happy", "happier",
    "studies", "studying", "studied", "student",
    "connection", "connected", "connecting", "connects"
]

print("İngilizce Stemming (Porter Stemmer):")
print("-" * 40)

for kelime in ingilizce_kelimeler:
    kok = stemmer.stem(kelime)
    print(f"  {kelime:15s} → {kok}")
```

**Beklenen Çıktı:**
```
İngilizce Stemming (Porter Stemmer):
----------------------------------------
  running         → run
  runs            → run
  ran             → ran
  runner          → runner
  happily         → happili
  happiness       → happi
  happy           → happi
  happier         → happier
  studies         → studi
  studying        → studi
  studied         → studi
  student         → student
  connection      → connect
  connected       → connect
  connecting      → connect
  connects        → connect
```

**Detaylı Açıklama:**

Stemmer'ın güçlü ve zayıf yönleri bu çıktıda açıkça görülmektedir:

- **Başarılı durumlar:** "running", "runs" → "run"; "connection", "connected", "connecting", "connects" → "connect". Bu kelimelerin hepsi aynı köke doğru şekilde indirgenmiştir.
- **Sorunlu durumlar:** "happily" → "happili" gerçek bir İngilizce kelime değildir. "ran" → "ran" olarak kalmıştır çünkü stemmer düzensiz fiil çekimlerini tanımaz. "student" ve "studies" farklı köklere indirgenmiştir.

Bu sorunlar, stemming'in kural tabanlı ve sözlük bilgisinden bağımsız çalışmasından kaynaklanır.

### 3.3 İngilizce Lemmatization Örneği (WordNet)

```python
from nltk.stem import WordNetLemmatizer

lemmatizer = WordNetLemmatizer()

# Lemmatizer'a sözcük türü (POS) bilgisi vermek sonuçları iyileştirir
# 'v' = fiil, 'n' = isim, 'a' = sıfat, 'r' = zarf
ornekler = [
    ("running", "v"),
    ("runs", "v"),
    ("ran", "v"),
    ("better", "a"),
    ("happily", "r"),
    ("studies", "v"),
    ("studies", "n"),
    ("geese", "n"),
    ("mice", "n"),
    ("feet", "n"),
]

print("İngilizce Lemmatization (WordNet):")
print("-" * 50)

for kelime, tur in ornekler:
    lemma = lemmatizer.lemmatize(kelime, pos=tur)
    tur_adi = {"v": "fiil", "n": "isim", "a": "sıfat", "r": "zarf"}[tur]
    print(f"  {kelime:12s} ({tur_adi:5s}) → {lemma}")
```

**Beklenen Çıktı:**
```
İngilizce Lemmatization (WordNet):
--------------------------------------------------
  running      (fiil ) → run
  runs         (fiil ) → run
  ran          (fiil ) → run
  better       (sıfat) → good
  happily      (zarf ) → happily
  studies      (fiil ) → study
  studies      (isim ) → study
  geese        (isim ) → goose
  mice         (isim ) → mouse
  feet         (isim ) → foot
```

**Detaylı Açıklama:**

Lemmatization ile stemming arasındaki farklar burada çok belirgindir:

- **"ran" → "run":** Lemmatizer düzensiz fiili doğru tanır. Stemmer bu dönüşümü yapamamıştı.
- **"better" → "good":** Düzensiz karşılaştırma sıfatını doğru lemmasına çevirir.
- **"geese" → "goose", "mice" → "mouse", "feet" → "foot":** Düzensiz çoğulları doğru tekil formuna dönüştürür. Stemmer bu dönüşümleri yapamaz.
- **"studies" kelimesi iki kez görünür:** Fiil olarak verildiğinde "study", isim olarak verildiğinde de "study" sonucunu verir. Sözcük türü bilgisi lemmatization'ın doğruluğunu artırır.

### 3.4 Türkçe Morfolojik Analiz ve Lemmatization (Zeyrek / Zemberek)

Zeyrek, Zemberek'in Python arayüzüdür ve Türkçe morfolojik analiz için özelleştirilmiştir:

```python
import zeyrek

analyzer = zeyrek.MorphAnalyzer()

turkce_kelimeler = [
    "gidiyorum",
    "evlerinden",
    "güzelleştirmek",
    "okuduklarımızdan",
    "Ankara'da",
    "yorucuydu",
    "gelebileceklerimizden",
    "çalışkanlıklarından"
]

print("Türkçe Morfolojik Analiz (Zeyrek):")
print("=" * 70)

for kelime in turkce_kelimeler:
    sonuclar = analyzer.lemmatize(kelime)
    print(f"\n  Kelime: {kelime}")

    if sonuclar:
        for sonuc in sonuclar:
            print(f"    Lemma: {sonuc[1]}")
    else:
        print("    Sonuç bulunamadı.")
```

**Beklenen Çıktı (yaklaşık):**
```
Türkçe Morfolojik Analiz (Zeyrek):
======================================================================

  Kelime: gidiyorum
    Lemma: ['gitmek']

  Kelime: evlerinden
    Lemma: ['ev']

  Kelime: güzelleştirmek
    Lemma: ['güzel']

  Kelime: okuduklarımızdan
    Lemma: ['okumak']

  Kelime: Ankara'da
    Lemma: ["Ankara'da"]

  Kelime: yorucuydu
    Lemma: ['yormak']

  Kelime: gelebileceklerimizden
    Lemma: ['gelmek']

  Kelime: çalışkanlıklarından
    Lemma: ['çalışkan']
```

**Detaylı Açıklama:**

Zeyrek'in Türkçe üzerindeki gücü bu örneklerde açıkça görülmektedir:

- **"gidiyorum" → "gitmek":** Türkçe'de "gitmek" fiili çekimlenirken kök değişikliğine uğrar (git → gid). Zeyrek bu düzensizliği doğru şekilde ele alır.
- **"evlerinden" → "ev":** Çoğul eki (-ler), iyelik eki (-i) ve ayrılma hali eki (-nden) peş peşe eklenmiştir. Zeyrek tüm bu ekleri soyarak "ev" köküne ulaşır.
- **"gelebileceklerimizden" → "gelmek":** Bu kelime 6 ayrı ek içerir. Zeyrek bu karmaşık yapıyı çözümleyerek doğru köke ulaşır. Bu tür kelimeler, basit bir stemmer'ın başa çıkamayacağı karmaşıklıktadır.
- **"güzelleştirmek" → "güzel":** Bir sıfattan türetilmiş fiil olmasına rağmen kök sıfat doğru tespit edilir.

---

## Uygulama 4: N-gram Oluşturma ve Analiz

### 4.1 Teori Hatırlatması

N-gramlar, metindeki ardışık n elemanlık kelime dizileridir. Unigram (n=1) tek kelimeleri, bigram (n=2) ardışık kelime çiftlerini, trigram (n=3) ardışık üç kelimelik grupları ifade eder. N-gramlar, kelimelerin birlikte kullanım kalıplarını yakalamak için önemlidir.

### 4.2 N-gram Oluşturma

```python
from nltk.util import ngrams
from collections import Counter

metin = """Türkiye ekonomisi son yıllarda önemli değişimler yaşamaktadır.
Ekonomik büyüme oranları dalgalanma göstermektedir. Son yıllarda enflasyon
oranları yükselmiştir. Merkez bankası para politikasını sıkılaştırmaktadır.
Ekonomik istikrar için yapısal reformlar gerekmektedir. Son yıllarda
ihracat rakamları artış göstermektedir."""

# Ön işleme: küçük harfe çevirme ve tokenizasyon
kelimeler = metin.lower().split()
# Basit noktalama temizliği
kelimeler = [kelime.strip('.,;:!?') for kelime in kelimeler]
# Boş stringleri kaldır
kelimeler = [kelime for kelime in kelimeler if kelime]

# Unigram, Bigram ve Trigram oluşturma
unigramlar = list(ngrams(kelimeler, 1))
bigramlar = list(ngrams(kelimeler, 2))
trigramlar = list(ngrams(kelimeler, 3))

print(f"Toplam kelime sayısı: {len(kelimeler)}")
print(f"Unigram sayısı: {len(unigramlar)}")
print(f"Bigram sayısı: {len(bigramlar)}")
print(f"Trigram sayısı: {len(trigramlar)}")

# En sık geçen bigramlar
print("\nEn sık geçen 5 bigram:")
bigram_frekans = Counter(bigramlar)
for bigram, sayi in bigram_frekans.most_common(5):
    print(f"  {bigram[0]} {bigram[1]} → {sayi} kez")

# En sık geçen trigramlar
print("\nEn sık geçen 5 trigram:")
trigram_frekans = Counter(trigramlar)
for trigram, sayi in trigram_frekans.most_common(5):
    print(f"  {trigram[0]} {trigram[1]} {trigram[2]} → {sayi} kez")
```

**Beklenen Çıktı (yaklaşık):**
```
Toplam kelime sayısı: 30
Unigram sayısı: 30
Bigram sayısı: 29
Trigram sayısı: 28

En sık geçen 5 bigram:
  son yıllarda → 3 kez
  yıllarda enflasyon → 1 kez
  ekonomik büyüme → 1 kez
  ...

En sık geçen 5 trigram:
  son yıllarda enflasyon → 1 kez
  ...
```

**Detaylı Açıklama:**

- **N-gram sayısı formülü:** Bir metinde k kelime varsa, n-gram sayısı = k - n + 1'dir. 30 kelimelik metinde bigram sayısı 29, trigram sayısı 28 olur.
- **Counter sınıfı:** Python'un `collections` modülünden gelen `Counter`, bir listede her elemanın kaç kez geçtiğini sayar. `most_common(5)` en sık geçen 5 elemanı döndürür.
- **"son yıllarda" bigramı** 3 kez geçmektedir. Bu, metinde tekrarlanan bir kalıp olduğunu gösterir. Gerçek dünya uygulamalarında bu tür tekrarlayan kalıplar, metnin konusu hakkında güçlü ipuçları verir.

---

## Uygulama 5: Gerçek Veri Seti ile Çalışma

### 5.1 Veri Seti Tanıtımı

Bu uygulamada bir e-ticaret sitesindeki Türkçe ürün yorumlarını simüle eden bir veri seti oluşturacak ve üzerinde kapsamlı bir ön işleme pipeline'ı çalıştıracağız.

### 5.2 Veri Setinin Oluşturulması

```python
import pandas as pd

# Türkçe ürün yorumları veri seti
veri = {
    "yorum_id": list(range(1, 21)),
    "yorum": [
        "Bu telefon gerçekten harika, kamerası çok iyi ve pil ömrü uzun!",
        "Ürün çok kötü, iki gün sonra bozuldu. Kesinlikle tavsiye etmiyorum.",
        "Fiyatına göre gayet iyi bir ürün. Hızlı kargo için teşekkürler.",
        "Kargo çok geç geldi ama ürün kaliteli görünüyor.",
        "Ekran kalitesi harika ama batarya çok çabuk bitiyor.",
        "Mükemmel bir telefon! Herşeyi çok hızlı, hiç takılma yok.",
        "Kutu hasarlı geldi, ürünü kontrol ettim sorun yok gibi görünüyor.",
        "Bu fiyata bu kadar iyi bir telefon bulamazsınız. Çok memnunum.",
        "Ses kalitesi çok kötü, hoparlör çatırdıyor. İade edeceğim.",
        "Tasarım çok şık ve hafif. Elde tutması çok rahat.",
        "Daha önce aldığım telefondan çok daha iyi. Kesinlikle tavsiye ederim.",
        "Ürün açıklamadaki gibi değil, fotoğraflar yanıltıcı.",
        "Hızlı şarj özelliği gerçekten işe yarıyor. 30 dakikada doluyor.",
        "Yüz tanıma özelliği çok yavaş çalışıyor, parmak izi daha iyi.",
        "Rengi çok güzel, siyah renk gerçekten şık duruyor.",
        "Depolama alanı çok az, sürekli yer açmam gerekiyor.",
        "Müşteri hizmetleri çok ilgisiz, soruma cevap bile vermediler.",
        "Oyun performansı mükemmel, hiç kasma yaşamadım.",
        "Telefon ısınma problemi var, uzun kullanımda çok ısınıyor.",
        "Genel olarak memnunum ama bu fiyata daha iyisi bulunabilir."
    ],
    "puan": [5, 1, 4, 3, 3, 5, 3, 5, 1, 5, 5, 2, 4, 2, 4, 2, 1, 5, 2, 3],
    "duygu": [
        "pozitif", "negatif", "pozitif", "nötr", "nötr",
        "pozitif", "nötr", "pozitif", "negatif", "pozitif",
        "pozitif", "negatif", "pozitif", "negatif", "pozitif",
        "negatif", "negatif", "pozitif", "negatif", "nötr"
    ]
}

df = pd.DataFrame(veri)

print("Veri Seti Genel Bilgileri:")
print("=" * 50)
print(f"Toplam yorum sayısı: {len(df)}")
print(f"\nDuygu dağılımı:")
print(df['duygu'].value_counts())
print(f"\nPuan istatistikleri:")
print(df['puan'].describe())
print(f"\nİlk 5 yorum:")
print(df[['yorum_id', 'yorum', 'puan', 'duygu']].head().to_string(index=False))
```

**Beklenen Çıktı:**
```
Veri Seti Genel Bilgileri:
==================================================
Toplam yorum sayısı: 20

Duygu dağılımı:
pozitif    8
negatif    8
nötr       4
Name: duygu, dtype: int64

Puan istatistikleri:
count    20.000000
mean      3.200000
std       1.436141
min       1.000000
25%       2.000000
50%       3.000000
75%       4.750000
max       5.000000
Name: puan, dtype: float64

İlk 5 yorum:
 yorum_id                                                      yorum  puan    duygu
        1  Bu telefon gerçekten harika, kamerası çok iyi ve pil ömrü uzun!     5  pozitif
        2  Ürün çok kötü, iki gün sonra bozuldu. Kesinlikle tavsiye etmiyorum.     1  negatif
        3  Fiyatına göre gayet iyi bir ürün. Hızlı kargo için teşekkürler.     4  pozitif
        4  Kargo çok geç geldi ama ürün kaliteli görünüyor.     3     nötr
        5  Ekran kalitesi harika ama batarya çok çabuk bitiyor.     3     nötr
```

**Detaylı Açıklama:**

Veri setimiz 20 Türkçe ürün yorumundan oluşmaktadır. Her yorum için bir puan (1-5) ve bir duygu etiketi (pozitif/negatif/nötr) belirlenmiştir. Duygu dağılımı dengeli tutulmuştur: 8 pozitif, 8 negatif ve 4 nötr yorum bulunmaktadır. Bu veri seti küçük olmasına rağmen, NLP ön işleme adımlarını uygulamak ve sonuçları gözlemlemek için yeterlidir. Gerçek projelerde binlerce veya milyonlarca yorum ile çalışılır.

### 5.3 Kapsamlı Ön İşleme Pipeline'ı

```python
import re
from nltk.corpus import stopwords

# Türkçe stopword listesi
turkce_stopwords = set(stopwords.words('turkish'))
ek_stopwords = {
    'da', 'de', 'mi', 'mu', 'mü', 'mı', 'ki', 'ile', 'için',
    'gibi', 'kadar', 'sonra', 'önce', 'daha', 'en', 'her', 'hiç',
    'bir', 'bu', 'şu', 'o', 'çok', 've', 'ama', 'fakat', 'hem'
}
tum_stopwords = turkce_stopwords.union(ek_stopwords)


def on_isleme_pipeline(metin):
    """
    Türkçe metin için kapsamlı ön işleme fonksiyonu.

    Adımlar:
    1. Küçük harfe çevirme
    2. Sayıları kaldırma
    3. Noktalama işaretlerini kaldırma
    4. Fazla boşlukları temizleme
    5. Tokenizasyon
    6. Stopword çıkarma
    7. Kısa kelimeleri (2 karakterden az) çıkarma

    Parametre:
        metin (str): İşlenecek ham metin

    Döndürür:
        str: Ön işlemeden geçmiş temiz metin
    """

    # Adım 1: Küçük harfe çevirme
    metin = metin.lower()

    # Adım 2: Sayıları kaldırma
    metin = re.sub(r'\d+', '', metin)

    # Adım 3: Noktalama işaretlerini kaldırma
    metin = re.sub(r'[^\w\s]', '', metin)

    # Adım 4: Fazla boşlukları tek boşluğa indirgeme
    metin = re.sub(r'\s+', ' ', metin).strip()

    # Adım 5: Tokenizasyon (basit split)
    kelimeler = metin.split()

    # Adım 6: Stopword çıkarma
    kelimeler = [k for k in kelimeler if k not in tum_stopwords]

    # Adım 7: 2 karakterden kısa kelimeleri çıkarma
    kelimeler = [k for k in kelimeler if len(k) > 2]

    return " ".join(kelimeler)


# Pipeline'ı tek bir yorum üzerinde adım adım gösterme
ornek_yorum = df.iloc[0]['yorum']
print("Ön İşleme Adımları (Detaylı Gösterim):")
print("=" * 60)
print(f"Ham metin       : {ornek_yorum}")
print(f"Küçük harf      : {ornek_yorum.lower()}")
print(f"Sayı temizleme  : {re.sub(r'[0-9]+', '', ornek_yorum.lower())}")
print(f"Noktalama temiz : {re.sub(r'[^a-zA-ZğüşıöçĞÜŞİÖÇ ]', '', ornek_yorum.lower())}")
print(f"Son hali        : {on_isleme_pipeline(ornek_yorum)}")

# Tüm veri setine uygulama
df['temiz_yorum'] = df['yorum'].apply(on_isleme_pipeline)

print("\n\nÖn İşleme Sonuçları (İlk 10 Yorum):")
print("=" * 80)
for _, satir in df.head(10).iterrows():
    print(f"\n  Ham    : {satir['yorum']}")
    print(f"  Temiz  : {satir['temiz_yorum']}")
    print(f"  Duygu  : {satir['duygu']}")
```

**Detaylı Açıklama:**

Bu pipeline fonksiyonu, bir ham metni adım adım temizler:

1. **Küçük harfe çevirme:** "Bu Telefon" ve "bu telefon" aynı şey olarak değerlendirilir. Aksi takdirde model bunları iki farklı kelime olarak algılar.

2. **Sayıları kaldırma:** "2 gün sonra" → "gün sonra". Sayılar çoğu metin sınıflandırma probleminde ayırt edici değildir. Ancak bazı alanlarda (fiyat analizi gibi) sayılar önemli olabilir.

3. **Noktalama işaretlerini kaldırma:** Virgül, nokta, ünlem gibi işaretler çıkarılır. `[^\w\s]` regex deseni, harf, rakam ve boşluk dışındaki her karakteri eşleştirir.

4. **Fazla boşlukları temizleme:** Önceki adımlarda oluşabilecek çoklu boşluklar tek boşluğa indirgenir.

5. **Tokenizasyon:** Basit `split()` kullanılarak metin kelimelere ayrılır.

6. **Stopword çıkarma:** Daha önce oluşturduğumuz genişletilmiş stopword listesi kullanılır.

7. **Kısa kelimeleri çıkarma:** Tek harfli kelimeler genellikle anlam taşımaz.

`df['yorum'].apply(on_isleme_pipeline)` ifadesi, pipeline fonksiyonunu veri setindeki tüm yorumlara tek satırda uygular. Bu, pandas'ın güçlü bir özelliğidir.

### 5.4 Kelime Frekans Analizi ve Görselleştirme

```python
from collections import Counter
import matplotlib.pyplot as plt

# Tüm temiz yorumları birleştir ve kelimelere ayır
tum_kelimeler = " ".join(df['temiz_yorum']).split()

print(f"Toplam kelime sayısı (ön işleme sonrası): {len(tum_kelimeler)}")
print(f"Benzersiz kelime sayısı: {len(set(tum_kelimeler))}")

# En sık geçen 15 kelime
frekanslar = Counter(tum_kelimeler)
en_sik_15 = frekanslar.most_common(15)

print("\nEn Sık Geçen 15 Kelime:")
print("-" * 30)
for kelime, sayi in en_sik_15:
    bar = "█" * sayi
    print(f"  {kelime:15s} {sayi:3d} {bar}")

# Grafik çizimi
kelime_listesi = [k for k, s in en_sik_15]
sayi_listesi = [s for k, s in en_sik_15]

plt.figure(figsize=(12, 6))
plt.barh(kelime_listesi[::-1], sayi_listesi[::-1], color='steelblue')
plt.xlabel('Frekans', fontsize=12)
plt.ylabel('Kelime', fontsize=12)
plt.title('En Sık Geçen 15 Kelime', fontsize=14)
plt.tight_layout()
plt.savefig('kelime_frekanslari.png', dpi=150)
plt.show()
print("\nGrafik 'kelime_frekanslari.png' olarak kaydedildi.")
```

**Detaylı Açıklama:**

Kelime frekans analizi, bir metindeki en sık geçen kelimeleri belirlememizi sağlar. Bu analiz, metnin genel teması hakkında hızlı bir fikir verir.

- **Counter** nesnesi kelimeleri sayar ve `most_common(15)` en sık 15 kelimeyi sıralı şekilde döndürür.
- **Yatay çubuk grafik** (`barh`) okunabilirlik açısından tercih edilir çünkü kelimeler dikey eksende rahatça okunabilir.
- **`[::-1]`** ifadesi listeyi tersine çevirir, böylece en sık kelime grafiğin en üstünde görünür.

### 5.5 Duygu Bazlı Kelime Karşılaştırması

```python
# Pozitif ve negatif yorumlardaki kelimeleri ayrı ayrı analiz etme
pozitif_kelimeler = " ".join(df[df['duygu'] == 'pozitif']['temiz_yorum']).split()
negatif_kelimeler = " ".join(df[df['duygu'] == 'negatif']['temiz_yorum']).split()

pozitif_frekans = Counter(pozitif_kelimeler)
negatif_frekans = Counter(negatif_kelimeler)

print("Pozitif Yorumlarda En Sık 10 Kelime:")
print("-" * 35)
for kelime, sayi in pozitif_frekans.most_common(10):
    print(f"  {kelime:15s} → {sayi} kez")

print("\nNegatif Yorumlarda En Sık 10 Kelime:")
print("-" * 35)
for kelime, sayi in negatif_frekans.most_common(10):
    print(f"  {kelime:15s} → {sayi} kez")

# Sadece pozitif veya sadece negatif yorumlarda geçen kelimeler
sadece_pozitif = set(pozitif_kelimeler) - set(negatif_kelimeler)
sadece_negatif = set(negatif_kelimeler) - set(pozitif_kelimeler)

print(f"\nSadece pozitif yorumlarda geçen kelimeler ({len(sadece_pozitif)} adet):")
print(f"  {sadece_pozitif}")
print(f"\nSadece negatif yorumlarda geçen kelimeler ({len(sadece_negatif)} adet):")
print(f"  {sadece_negatif}")
```

**Detaylı Açıklama:**

Bu analiz, pozitif ve negatif yorumları ayırt eden anahtar kelimeleri bulmayı amaçlar. Küme farkı (set difference) işlemi ile bir duygu sınıfına özel kelimeler tespit edilir.

Beklenen sonuçlarda "harika", "mükemmel", "memnunum" gibi kelimelerin pozitif yorumlara; "kötü", "bozuldu", "iade" gibi kelimelerin negatif yorumlara özgü olması muhtemeldir. Bu tür bir analiz, basit bir duygu sözlüğü oluşturmanın temelini oluşturur.

---

## Uygulama 6: Kelime Bulutu (Word Cloud)

```python
from wordcloud import WordCloud
import matplotlib.pyplot as plt

# Tüm temiz yorumları birleştir
tum_metin = " ".join(df['temiz_yorum'])

# Kelime bulutu oluştur
wordcloud = WordCloud(
    width=800,
    height=400,
    background_color='white',
    max_words=50,
    colormap='viridis',
    font_path=None,       # Türkçe karakter desteği için Türkçe font yolu verilebilir
    collocations=False     # Tekrarlayan kelime çiftlerini engelle
).generate(tum_metin)

plt.figure(figsize=(14, 7))
plt.imshow(wordcloud, interpolation='bilinear')
plt.axis('off')
plt.title('Ürün Yorumları - Kelime Bulutu', fontsize=16)
plt.tight_layout()
plt.savefig('kelime_bulutu.png', dpi=150)
plt.show()
print("Kelime bulutu 'kelime_bulutu.png' olarak kaydedildi.")

# Pozitif ve Negatif için ayrı kelime bulutları
fig, axes = plt.subplots(1, 2, figsize=(18, 7))

pozitif_metin = " ".join(df[df['duygu'] == 'pozitif']['temiz_yorum'])
negatif_metin = " ".join(df[df['duygu'] == 'negatif']['temiz_yorum'])

wc_pozitif = WordCloud(
    width=800, height=400, background_color='white',
    max_words=30, colormap='Greens', collocations=False
).generate(pozitif_metin)

wc_negatif = WordCloud(
    width=800, height=400, background_color='white',
    max_words=30, colormap='Reds', collocations=False
).generate(negatif_metin)

axes[0].imshow(wc_pozitif, interpolation='bilinear')
axes[0].set_title('Pozitif Yorumlar', fontsize=14, color='green')
axes[0].axis('off')

axes[1].imshow(wc_negatif, interpolation='bilinear')
axes[1].set_title('Negatif Yorumlar', fontsize=14, color='red')
axes[1].axis('off')

plt.tight_layout()
plt.savefig('duygu_kelime_bulutlari.png', dpi=150)
plt.show()
print("Duygu bazlı kelime bulutları 'duygu_kelime_bulutlari.png' olarak kaydedildi.")
```

**Detaylı Açıklama:**

Kelime bulutu, bir metindeki kelime frekanslarını görsel olarak sunmanın etkili bir yoludur. Sık geçen kelimeler daha büyük, nadir kelimeler daha küçük görünür.

- **`width` ve `height`**: Bulutun piksel cinsinden boyutunu belirler.
- **`max_words=50`**: En fazla 50 kelime gösterilir. Bu sayı artırılabilir ancak çok fazla kelime görselin okunabilirliğini azaltır.
- **`colormap`**: Renk paletini belirler. 'viridis' genel kullanım, 'Greens' pozitif, 'Reds' negatif duygular için seçilmiştir.
- **`collocations=False`**: Aynı kelime çiftinin tekrarlanmasını engeller.

İki ayrı kelime bulutu oluşturarak pozitif ve negatif yorumlar arasındaki farkı görsel olarak karşılaştırmak mümkün olur. Bu, sunumlarda ve raporlarda çok etkili bir görselleştirme yöntemidir.

---

## Uygulama 7: Varlık Adı Tanıma (NER)

### 7.1 Teori Hatırlatması

Varlık Adı Tanıma (Named Entity Recognition - NER), metin içindeki kişi adları, kurum adları, yer adları, tarihler, miktarlar gibi özel varlıkları otomatik olarak tespit etme görevidir.

### 7.2 İngilizce NER Uygulaması (spaCy)

Türkçe NER modelleri henüz İngilizce kadar yaygın olmadığı için, önce kavramı İngilizce üzerinde göstereceğiz:

```python
import spacy

# İngilizce NLP modelini yükle
nlp = spacy.load("en_core_web_sm")

metin_en = """Apple Inc. was founded by Steve Jobs in Cupertino, California in 1976.
The company announced a revenue of $394 billion in 2022. Tim Cook has been
serving as the CEO since August 2011. Their headquarters is located at
One Apple Park Way, Cupertino."""

# NER uygulama
doc = nlp(metin_en)

print("Varlık Adı Tanıma (NER) Sonuçları:")
print("=" * 55)
print(f"{'Varlık':<25s} {'Etiket':<12s} {'Açıklama'}")
print("-" * 55)

for varlik in doc.ents:
    print(f"  {varlik.text:<25s} {varlik.label_:<12s} {spacy.explain(varlik.label_)}")
```

**Beklenen Çıktı:**
```
Varlık Adı Tanıma (NER) Sonuçları:
=======================================================
Varlık                    Etiket       Açıklama
-------------------------------------------------------
  Apple Inc.                ORG          Companies, agencies, institutions, etc.
  Steve Jobs                PERSON       People, including fictional
  Cupertino                 GPE          Countries, cities, states
  California                GPE          Countries, cities, states
  1976                      DATE         Absolute or relative dates or periods
  $394 billion              MONEY        Monetary values, including unit
  2022                      DATE         Absolute or relative dates or periods
  Tim Cook                  PERSON       People, including fictional
  August 2011               DATE         Absolute or relative dates or periods
  One Apple Park Way        FAC          Buildings, airports, highways, bridges, etc.
  Cupertino                 GPE          Countries, cities, states
```

**Detaylı Açıklama:**

spaCy'nin NER modeli metindeki varlıkları otomatik olarak tespit eder ve sınıflandırır:

- **ORG (Organization):** Şirket, kurum, kuruluş adları. "Apple Inc." doğru şekilde kurum olarak tespit edilmiştir.
- **PERSON:** Kişi adları. "Steve Jobs" ve "Tim Cook" kişi olarak tanınmıştır.
- **GPE (Geo-Political Entity):** Ülke, şehir, eyalet adları. "Cupertino" ve "California" yer olarak etiketlenmiştir.
- **DATE:** Tarih ifadeleri. "1976", "2022", "August 2011" tarih olarak algılanmıştır.
- **MONEY:** Parasal ifadeler. "$394 billion" doğru şekilde tespit edilmiştir.
- **FAC (Facility):** Bina, havalimanı, köprü gibi yapılar. "One Apple Park Way" adres/bina olarak etiketlenmiştir.

### 7.3 Türkçe NER Uygulaması (Basit Kural Tabanlı)

Türkçe için gelişmiş NER modelleri (örneğin BERTurk tabanlı) mevcuttur ancak kurulumu daha karmaşıktır. Burada kavramı anlamak için basit bir kural tabanlı yaklaşım gösteriyoruz:

```python
import re

def basit_turkce_ner(metin):
    """
    Basit kural tabanlı Türkçe NER.
    Büyük harfle başlayan kelimeleri ve belirli desenleri tespit eder.

    NOT: Bu gerçek bir NER sistemi değildir, kavramsal gösterim amaçlıdır.
    Gerçek projelerde BERTurk veya benzeri modeller kullanılmalıdır.
    """
    varliklar = []

    # Büyük harfle başlayan kelimeleri bul (cümle başı hariç)
    # Cümle başı kontrolü: noktadan sonra gelen kelimeleri atla
    cumleler = metin.split('.')

    for cumle in cumleler:
        cumle = cumle.strip()
        if not cumle:
            continue

        kelimeler = cumle.split()

        for i, kelime in enumerate(kelimeler):
            temiz_kelime = kelime.strip('.,;:!?()"')

            # Cümle başındaki kelimeyi atla (i == 0)
            if i == 0:
                continue

            # Büyük harfle başlayan kelimeler → olası özel isim
            if temiz_kelime and temiz_kelime[0].isupper():
                varliklar.append({
                    'metin': temiz_kelime,
                    'etiket': 'OLASI_VARLIK',
                    'pozisyon': i
                })

    # Tarih desenleri
    tarih_deseni = r'\d{1,2}\s+(?:Ocak|Şubat|Mart|Nisan|Mayıs|Haziran|Temmuz|Ağustos|Eylül|Ekim|Kasım|Aralık)\s+\d{4}'
    tarihler = re.findall(tarih_deseni, metin)
    for tarih in tarihler:
        varliklar.append({'metin': tarih, 'etiket': 'TARİH', 'pozisyon': -1})

    # Parasal ifadeler
    para_deseni = r'\d+[\.,]?\d*\s*(?:TL|lira|dolar|euro|USD|EUR)'
    paralar = re.findall(para_deseni, metin, re.IGNORECASE)
    for para in paralar:
        varliklar.append({'metin': para, 'etiket': 'PARA', 'pozisyon': -1})

    return varliklar


# Test metni
test_metni = """Ahmet Yılmaz 15 Ocak 2024 tarihinde İstanbul'dan Ankara'ya gitti.
Toplantıda Mehmet Bey ve Ayşe Hanım da vardı. Proje bütçesi 500.000 TL olarak
belirlendi. Türkiye Bilimsel ve Teknolojik Araştırma Kurumu bu projeyi destekliyor."""

sonuclar = basit_turkce_ner(test_metni)

print("Basit Kural Tabanlı Türkçe NER Sonuçları:")
print("=" * 50)

for sonuc in sonuclar:
    print(f"  {sonuc['metin']:<30s} → {sonuc['etiket']}")

print("\n⚠ UYARI: Bu basit bir kural tabanlı yaklaşımdır.")
print("  Gerçek projelerde BERTurk tabanlı NER modelleri kullanılmalıdır.")
```

**Detaylı Açıklama:**

Bu basit NER sistemi üç temel kurala dayanır:

1. **Büyük harfle başlayan kelimeler:** Türkçe'de özel isimler büyük harfle yazılır. Cümle başındaki kelimeyi atlayarak, cümle ortasında büyük harfle başlayan kelimeleri "olası varlık" olarak işaretleriz. Bu yöntem basittir ancak her büyük harfli kelime özel isim olmayabilir.

2. **Tarih desenleri:** "15 Ocak 2024" gibi tarih ifadelerini regex ile tespit ederiz. Türkçe ay adlarını desene dahil ettik.

3. **Parasal ifadeler:** "500.000 TL" gibi ifadeleri regex ile yakalarız.

Bu sistem gerçek bir NER modelinin çok gerisindedir. Gerçek projelerde derin öğrenme tabanlı modeller (BERTurk gibi) kullanılır. Ancak bu örnek, NER mantığını anlamak ve kuralların sınırlarını görmek açısından öğreticidir.

---

## Uygulama 8: Uçtan Uca Mini Proje

Bu uygulamada, önceki tüm adımları birleştirerek bir haber metnini analiz edeceğiz.

### 8.1 Haber Metni Veri Seti

```python
import pandas as pd

haberler = {
    "baslik": [
        "Merkez Bankası faiz kararını açıkladı",
        "Galatasaray deplasmanda 3-1 kazandı",
        "İstanbul'da şiddetli yağmur hayatı felç etti",
        "Yapay zeka sağlık sektöründe devrim yaratıyor",
        "Dolar kuru yeni rekor kırdı",
        "Türk bilim insanları uzay teknolojisinde başarı elde etti",
        "Eğitimde dijital dönüşüm hızlanıyor",
        "İhracat rakamları beklentilerin üzerinde geldi",
        "Fenerbahçe yeni transferini açıkladı",
        "Deprem bölgesinde yeniden yapılanma sürüyor"
    ],
    "metin": [
        "Merkez Bankası politika faizini yüzde 45'te sabit tutma kararı aldı. Piyasalar bu kararı bekliyordu. Enflasyonla mücadele kapsamında sıkı para politikası devam edecek.",
        "Galatasaray, Süper Lig'in 20. haftasında deplasmanda rakibini 3-1 mağlup etti. Maçın yıldızı iki gol atan Icardi oldu. Sarı-kırmızılılar puanını 48'e yükseltti.",
        "İstanbul'da etkili olan sağanak yağış nedeniyle birçok noktada su baskınları yaşandı. Trafik felç olurken, bazı metro istasyonları da su altında kaldı. Valilik uyarılarını sürdürüyor.",
        "Yapay zeka teknolojileri sağlık alanında tanı koymadan tedavi planlamaya kadar pek çok noktada kullanılmaya başlandı. Özellikle görüntü analizi ve erken teşhis alanlarında büyük ilerleme kaydedildi.",
        "Dolar kuru güne yükselişle başlayarak yeni bir rekor seviyeye ulaştı. Ekonomistler kur üzerindeki baskının devam edeceğini öngörüyor. Merkez Bankası piyasayı yakından takip ediyor.",
        "Türk bilim insanlarının geliştirdiği uydu teknolojisi uluslararası arenada büyük ilgi gördü. Proje ekibi uzay ajansı ile ortak çalışmalar yürütüyor.",
        "Milli Eğitim Bakanlığı okulların dijital altyapısını güçlendirmek için kapsamlı bir program başlattı. Tablet dağıtımı ve öğretmen eğitimleri hızla devam ediyor.",
        "Türkiye'nin ihracatı geçen ay bir önceki yılın aynı dönemine göre yüzde 12 artış gösterdi. En fazla ihracat otomotiv sektöründen yapıldı.",
        "Fenerbahçe, dünyaca ünlü yıldız oyuncuyu kadrosuna kattığını açıkladı. Transfer bedeli 15 milyon euro olarak belirtildi.",
        "Deprem bölgesinde kalıcı konut yapımı devam ediyor. Şimdiye kadar 50 bin konut teslim edildi. Afetzedeler yeni evlerine taşınmaya başladı."
    ],
    "kategori": [
        "ekonomi", "spor", "gündem", "teknoloji", "ekonomi",
        "bilim", "eğitim", "ekonomi", "spor", "gündem"
    ]
}

df_haber = pd.DataFrame(haberler)

print("Haber Veri Seti:")
print("=" * 60)
print(f"Toplam haber sayısı: {len(df_haber)}")
print(f"\nKategori dağılımı:")
print(df_haber['kategori'].value_counts())
print(f"\nİlk 3 haber:")
for _, satir in df_haber.head(3).iterrows():
    print(f"\n  Başlık   : {satir['baslik']}")
    print(f"  Kategori : {satir['kategori']}")
    print(f"  Metin    : {satir['metin'][:80]}...")
```

### 8.2 Haberlere Ön İşleme ve Analiz Uygulama

```python
from collections import Counter

# Daha önce tanımladığımız ön işleme fonksiyonunu haberlere uygulayalım
df_haber['temiz_metin'] = df_haber['metin'].apply(on_isleme_pipeline)

# Her kategori için en sık geçen kelimeler
print("Kategori Bazlı Kelime Analizi:")
print("=" * 60)

for kategori in df_haber['kategori'].unique():
    kategori_metinleri = " ".join(
        df_haber[df_haber['kategori'] == kategori]['temiz_metin']
    )
    kelimeler = kategori_metinleri.split()
    frekans = Counter(kelimeler)

    print(f"\n📌 {kategori.upper()} ({len(kelimeler)} kelime)")
    print(f"   En sık 5 kelime:")
    for kelime, sayi in frekans.most_common(5):
        print(f"     {kelime:<20s} → {sayi} kez")

# Genel istatistikler
print("\n\nGenel İstatistikler:")
print("=" * 60)

# Her haber için kelime sayısı
df_haber['ham_kelime_sayisi'] = df_haber['metin'].apply(lambda x: len(x.split()))
df_haber['temiz_kelime_sayisi'] = df_haber['temiz_metin'].apply(lambda x: len(x.split()))
df_haber['cikarilan_kelime_orani'] = round(
    (1 - df_haber['temiz_kelime_sayisi'] / df_haber['ham_kelime_sayisi']) * 100, 1
)

print(f"\nOrtalama ham kelime sayısı    : {df_haber['ham_kelime_sayisi'].mean():.1f}")
print(f"Ortalama temiz kelime sayısı  : {df_haber['temiz_kelime_sayisi'].mean():.1f}")
print(f"Ortalama çıkarma oranı       : %{df_haber['cikarilan_kelime_orani'].mean():.1f}")

print("\nHaber bazlı detay:")
for _, satir in df_haber.iterrows():
    print(f"  {satir['baslik'][:40]:<42s} | "
          f"Ham: {satir['ham_kelime_sayisi']:2d} | "
          f"Temiz: {satir['temiz_kelime_sayisi']:2d} | "
          f"Çıkarma: %{satir['cikarilan_kelime_orani']}")
```

**Detaylı Açıklama:**

Bu mini proje, dersteki tüm kavramları birleştiren bir uygulamadır:

1. **Veri seti oluşturma:** 10 haber metni, başlık ve kategori bilgisiyle birlikte pandas DataFrame olarak yapılandırılmıştır.

2. **Ön işleme pipeline'ı:** Daha önce yazdığımız `on_isleme_pipeline` fonksiyonu tüm haberlere uygulanmıştır.

3. **Kategori bazlı analiz:** Her haber kategorisi için en sık geçen kelimeler bulunmuştur. Bu analiz, farklı kategorilerin kendine özgü kelime dağarcığına sahip olduğunu gösterir. Örneğin ekonomi haberlerinde "faiz", "enflasyon", "kur" gibi kelimelerin; spor haberlerinde "gol", "maç", "lig" gibi kelimelerin öne çıkması beklenir.

4. **Kelime çıkarma oranı:** Ön işleme sonrasında her haberden ne kadar kelime çıkarıldığı hesaplanmıştır. Bu oran genellikle %30-50 arasında olur ve ön işlemenin veri boyutunu ne kadar küçülttüğünü gösterir.

---

## Haftalık Ödev

### Ödev 1: Kendi Veri Setinizi Oluşturun ve Analiz Edin

Twitter, ekşi sözlük, hepsiburada veya benzeri bir kaynaktan en az 30 adet Türkçe metin toplayın. Bu metinleri bir CSV dosyasına kaydedin ve şu analizleri yapın:

1. Ön işleme pipeline'ını uygulayın
2. En sık geçen 20 kelimeyi bulun ve grafik olarak çizin
3. Kelime bulutu oluşturun
4. Bigram analizi yapın ve en sık 10 bigramı raporlayın

### Ödev 2: Stemming ve Lemmatization Karşılaştırması

En az 20 farklı Türkçe kelime seçin (farklı ek yapılarına sahip olmasına dikkat edin) ve şunları yapın:

1. Zeyrek ile lemmatization uygulayın
2. Sonuçları bir tabloda karşılaştırın
3. Hangi kelimelerde hata veya beklenmedik sonuç oluştuğunu raporlayın

### Ödev Teslim Formatı

- Jupyter Notebook (.ipynb) veya Python script (.py) olarak teslim edin
- Kodların yanı sıra açıklama hücreleri (markdown) ekleyin
- Grafik çıktılarını notebook içinde veya ayrı dosya olarak ekleyin

---

## Özet

Bu derste Hafta 2'de öğrendiğimiz NLP temel kavramlarını Python ile pratiğe döktük. Tokenizasyon, stopword çıkarma, stemming, lemmatization, N-gram analizi, kelime frekans analizi, kelime bulutu ve varlık adı tanıma konularında uygulamalar gerçekleştirdik. Gerçek bir veri seti üzerinde uçtan uca ön işleme pipeline'ı kurduk ve sonuçları görselleştirdik. Önümüzdeki hafta metin temsil yöntemlerini (Bag of Words, TF-IDF, Word Embeddings) ele alacağız.
