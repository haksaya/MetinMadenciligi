
# HAFTA 2: Doğal Dil İşlemeye Giriş (NLP)

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 2 |
| **Süre** | 2 Saat |
| **Konu** | Doğal Dil İşlemeye Giriş |
| **Kaynak Kitap** | Uygulamalı Metin Madenciliği: Doğal Dil İşleme ve Python ile |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Doğal Dil İşleme'nin (NLP) ne olduğunu ve metin madenciliğindeki rolünü açıklayabilecektir.
- Dil işlemenin farklı seviyelerini (karakter, kelime, cümle, doküman) tanıyacaktır.
- Tokenizasyon, stemming, lemmatization gibi temel kavramları örneklerle gösterebilecektir.
- Türkçe gibi eklemeli dillerde karşılaşılan zorlukları fark edecektir.
- NLP'nin tarihsel gelişimini ana hatlarıyla bilecektir.

---

## 1. Doğal Dil İşleme (NLP) Nedir?

### 1.1 Tanım

Doğal Dil İşleme (Natural Language Processing - NLP), insan dilini bilgisayarların anlayabilmesi, işleyebilmesi ve üretebilmesi için geliştirilen yöntemler bütünüdür.

İnsanlar birbirleriyle doğal dil aracılığıyla iletişim kurar: Türkçe, İngilizce, Almanca gibi diller bunlara örnektir. Ancak bilgisayarlar doğal dili doğrudan anlayamaz. Bilgisayarlar sayılarla çalışır. NLP, bu iki dünya arasında köprü kurarak insan dilini bilgisayarların işleyebileceği bir forma dönüştürmeyi amaçlar.

Günlük hayatta farkında olmadan birçok NLP uygulaması kullanırız:
- Akıllı telefondaki otomatik düzeltme
- Arama motorlarında yazdığımız sorguların anlaşılması
- Sesli asistanlar (Siri, Google Asistan)
- E-posta spam filtreleri
- Çeviri uygulamaları (Google Translate)
- Chatbot'lar ve müşteri destek sistemleri

### 1.2 NLP'nin Metin Madenciliğindeki Yeri

Bir önceki hafta metin madenciliği iş akışını (pipeline) öğrenmiştik. NLP, bu pipeline'ın neredeyse her aşamasında karşımıza çıkar:

- **Ön işleme aşamasında:** Tokenizasyon, kök bulma, stopword çıkarma gibi işlemler NLP teknikleridir.
- **Temsil aşamasında:** Kelimelerin ve cümlelerin sayısal vektörlere dönüştürülmesi NLP yöntemleriyle yapılır.
- **Modelleme aşamasında:** Dil modelleri, duygu analizi, varlık adı tanıma gibi görevler NLP'nin uzmanlık alanıdır.

Kısacası metin madenciliği, NLP tekniklerini kullanarak metinden bilgi çıkarma sürecidir.

### 1.3 Kural Tabanlı ve İstatistiksel Yaklaşımlar

NLP'de iki temel yaklaşım vardır:

**Kural Tabanlı Yaklaşım:** İnsan uzmanlar tarafından elle yazılan dilbilgisi kurallarına dayanır. Örneğin, "eğer cümlede 'harika' kelimesi geçiyorsa, bu cümle pozitiftir" gibi bir kural yazılabilir. Bu yaklaşım basit problemlerde işe yarar ancak dilin karmaşıklığı arttıkça yetersiz kalır.

**İstatistiksel / Öğrenen Yaklaşım:** Büyük miktarda veriden otomatik olarak örüntü öğrenen yaklaşımdır. Makine öğrenmesi ve derin öğrenme bu kategoriye girer. Günümüzde en yaygın kullanılan yaklaşımdır çünkü elle kural yazmak yerine, modelin veriyi görerek kendi kurallarını öğrenmesi sağlanır.

---

## 2. Dil İşleme Seviyeleri

Doğal dil, farklı seviyelerde analiz edilebilir. Her seviye, dilin farklı bir yönünü ele alır:

### 2.1 Karakter Düzeyi

Dilin en küçük birimi olan harfler ve karakterlerle ilgilenir. Karakter düzeyinde işleme, özellikle yazım hatası düzeltme, dil tespiti ve karakter tabanlı modellerde önemlidir.

**Örnek:** "mrhaba" → "merhaba" (yazım düzeltme)

### 2.2 Kelime / Token Düzeyi

Kelimeler veya tokenlar üzerinde yapılan işlemlerdir. Bir kelimenin kökünü bulma, sözcük türünü belirleme, kelimenin anlamını çıkarma gibi işlemler bu seviyede gerçekleşir.

**Örnek:** "koşuyordum" → kök: "koş", tür: fiil, zaman: geçmiş sürekli

### 2.3 Cümle Düzeyi

Cümlenin sözdizimsel (syntax) yapısının ve anlamının (semantics) analiz edilmesidir. Öznesi kim, nesnesi ne, fiilin ne olduğu gibi sorular bu seviyede cevaplanır.

**Örnek:** "Ali okula gitti." → Özne: Ali, Yer tamlayıcısı: okula, Yüklem: gitti

### 2.4 Doküman Düzeyi

Bir dokümanın genel temasının, duygusunun veya konusunun belirlenmesidir.

**Örnek:** Bir ürün yorumunun genel olarak "pozitif" veya "negatif" olarak sınıflandırılması.

---

## 3. NLP Görevleri Haritası

NLP alanında birçok farklı görev tanımlanmıştır. Her görev, dilin farklı bir yönünü otomatikleştirmeyi hedefler:

### 3.1 Dil Tespiti

Verilen bir metnin hangi dilde yazıldığını belirleme görevidir.

**Örnek Girdi:** "Bugün hava çok güzel"
**Çıktı:** Türkçe

Bu görev özellikle çok dilli sistemlerde önemlidir. Sosyal medyadan toplanan veriler farklı dillerde olabilir ve her dil için farklı işleme adımları gerekebilir.

### 3.2 Tokenizasyon (Belirteç Ayrıştırma)

Metni daha küçük parçalara (tokenlara) ayırma işlemidir. En temel NLP adımlarından biridir.

**Örnek Girdi:** "Bugün Ankara'da hava çok güzel."
**Çıktı:** [Bugün, Ankara'da, hava, çok, güzel, .]

Basit görünse de tokenizasyon her dilde aynı değildir. Türkçe'de "Ankara'da" tek token mı yoksa "Ankara" ve "'da" olarak iki token mı olmalı? Bu tür sorular tokenizasyonu karmaşık hale getirir.

### 3.3 Sözcük Türü Etiketleme (POS Tagging)

Her kelimenin dilbilgisel türünün (isim, fiil, sıfat, zarf vb.) belirlenmesidir.

**Örnek:**
- "Bugün" → Zarf
- "hava" → İsim
- "güzel" → Sıfat
- "gitti" → Fiil

Bu bilgi, cümlenin yapısını anlamak ve daha ileri düzey analizler yapmak için gereklidir.

### 3.4 Varlık Adı Tanıma (Named Entity Recognition - NER)

Metin içindeki kişi adları, kurum adları, yer adları, tarihler, miktarlar gibi özel varlıkların tespit edilmesidir.

**Örnek Girdi:** "Ahmet Bey, 15 Ocak'ta İstanbul'dan Ankara'ya uçtu."
**Çıktı:**
- Ahmet Bey → KİŞİ
- 15 Ocak → TARİH
- İstanbul → YER
- Ankara → YER

NER, bilgi çıkarımının temel taşlarından biridir ve haber analizi, hukuki metin işleme gibi alanlarda yaygın kullanılır.

### 3.5 Duygu Analizi (Sentiment Analysis)

Bir metnin olumlu, olumsuz veya nötr duygu içerip içermediğini belirleme görevidir.

**Örnekler:**
- "Bu ürün harika, çok memnunum!" → Pozitif
- "Kargo çok geç geldi, hiç memnun değilim." → Negatif
- "Ürün dün teslim edildi." → Nötr

### 3.6 Makine Çevirisi

Bir dildeki metni başka bir dile otomatik çevirme görevidir.

**Örnek:** "Bugün hava güzel" → "The weather is nice today"

Bu görev, NLP'nin en eski ve en zorlu alanlarından biridir. Günümüzde Transformer tabanlı modeller (Google Translate gibi) oldukça başarılı sonuçlar vermektedir.

### 3.7 Otomatik Özetleme

Uzun bir metni, ana fikirlerini koruyarak kısaltma görevidir. İki temel yaklaşım vardır:

- **Çıkarımcı (Extractive):** Metindeki en önemli cümleler seçilerek özet oluşturulur.
- **Üretici (Abstractive):** Model, metni anlayarak kendi cümleleriyle yeni bir özet üretir.

### 3.8 Soru-Cevap Sistemleri

Doğal dilde sorulan bir soruya, bir metin kaynağından cevap bulma görevidir.

**Örnek:**
- Kaynak metin: "Türkiye Cumhuriyeti'nin başkenti Ankara'dır."
- Soru: "Türkiye'nin başkenti neresidir?"
- Cevap: "Ankara"

---

## 4. Temel NLP Kavramları (Örneklerle)

Bu bölümde, NLP'nin temel kavramlarını tek bir örnek cümle üzerinden adım adım açıklayacağız.

**Örnek cümle:**
> "Bugün Ankara'da hava çok güzel ama trafik yorucuydu."

### 4.1 Tokenization (Belirteç Ayrıştırma)

Cümleyi tokenlara ayırdığımızda:

```
[Bugün, Ankara'da, hava, çok, güzel, ama, trafik, yorucuydu, .]
```

Toplam 9 token elde edilir. Dikkat edilmesi gereken noktalar:
- Noktalama işareti (.) ayrı bir token olarak kabul edilir.
- "Ankara'da" tek parça mı kalmalı, yoksa "Ankara" ve "'da" olarak ayrılmalı mı? Bu, kullanılan tokenizer'a ve probleme bağlıdır.

### 4.2 Stopword (Etkisiz Kelimeler)

Dilde çok sık geçen ancak genellikle anlam analizi açısından belirleyici olmayan kelimelerdir.

Türkçe'deki yaygın stopword örnekleri: "ve", "bir", "bu", "ama", "için", "ile", "da", "de", "çok"

Örnek cümlemizde "ama" ve "çok" stopword olarak kabul edilebilir.

**Önemli uyarı:** Stopword çıkarma her zaman doğru bir karar değildir. Örneğin duygu analizinde "çok güzel" ile "güzel" arasında anlam farkı vardır. "Çok" kelimesini çıkarmak, duygu yoğunluğu bilgisini kaybetmemize neden olur. Bu yüzden stopword listesi probleme göre özelleştirilmelidir.

### 4.3 Stemming (Kök Bulma)

Kelimenin eklerini basit kurallarla kırparak köküne ulaşmaya çalışan yöntemdir. Hızlıdır ancak her zaman doğru sonuç vermez.

**Örnekler:**
- "yorucuydu" → "yorucu" → "yor"
- "güzel" → "güzel" (kök zaten kendisi)
- "Ankara'da" → "Ankara"

Stemming bazen hatalı sonuç verebilir. Örneğin "araba" ve "arap" kelimeleri aynı köke indirgenebilir, bu da yanlış bir sonuçtur.

### 4.4 Lemmatization (Sözlük Kökü Bulma)

Stemming'den farklı olarak, kelimenin dilbilgisi kurallarına göre doğru sözlük formunu bulan yöntemdir. Daha yavaştır ancak daha doğru sonuç verir.

**Örnekler:**
- "yorucuydu" → "yorucu" (sıfat)
- "gidiyorum" → "gitmek" (fiil)
- "evlerin" → "ev" (isim)

Lemmatization, kelimenin sözcük türünü de dikkate alır. Bu yüzden daha güvenilir sonuçlar üretir.

### 4.5 Morfolojik Analiz

Bir kelimenin iç yapısının (kök + ekler) ayrıntılı olarak çözümlenmesidir. Türkçe gibi eklemeli dillerde büyük önem taşır.

**Örnekler:**
- "Ankara'da" → Ankara + LOC (bulunma hali eki, -da)
- "yorucuydu" → yorucu + i- (ek fiil) + -di (geçmiş zaman) + ∅ (3. tekil kişi)
- "gelebileceklerimizden" → gel + e + bil + ecek + ler + imiz + den

Morfolojik analiz, Türkçe NLP'nin en temel ve en kritik adımlarından biridir.

### 4.6 N-gram

Metindeki ardışık n elemanlık (kelime veya karakter) dizileridir.

Örnek cümlemiz üzerinden:

**Unigram (n=1):** Her kelime tek başına
- [Bugün], [Ankara'da], [hava], [çok], [güzel], [ama], [trafik], [yorucuydu]

**Bigram (n=2):** Ardışık iki kelimelik gruplar
- [Bugün Ankara'da], [Ankara'da hava], [hava çok], [çok güzel], [güzel ama], [ama trafik], [trafik yorucuydu]

**Trigram (n=3):** Ardışık üç kelimelik gruplar
- [Bugün Ankara'da hava], [Ankara'da hava çok], [hava çok güzel], ...

N-gramlar, kelimelerin bağlamını yakalamak için kullanılır. Özellikle "çok güzel" gibi birlikte anlam taşıyan kelime gruplarını tespit etmede faydalıdır.

---

## 5. Türkçe'nin NLP Açısından Zorlukları

Türkçe, Ural-Altay dil ailesine ait **eklemeli (aglütinatif)** bir dildir. Bu özellik, NLP açısından kendine özgü zorluklar doğurur:

### 5.1 Ek Yığılması

Türkçe'de tek bir kelimeye çok sayıda ek getirilebilir. Bu durum, kelime haznesini (vocabulary) çok büyütür ve modellerin öğrenmesini zorlaştırır.

**Örnek:** "gelebileceklerimizden" → gel + e + bil + ecek + ler + imiz + den
Bu tek kelime, İngilizce'de "from those of us who can come" şeklinde birden fazla kelimeyle ifade edilir.

### 5.2 Belirsizlik (Ambiguity)

Aynı kelime farklı anlamlara gelebilir. Bu durum her dilde vardır ancak Türkçe'de özellikle yaygındır.

**Örnek:** "yüz" kelimesi:
- Sayı: yüz (100)
- Organ: yüz (face)
- Fiil: yüz (swim)

Hangi anlamda kullanıldığı ancak bağlamdan anlaşılabilir.

### 5.3 Serbest Sözcük Dizimi

Türkçe'de cümle içindeki kelimelerin sırası oldukça esnektir. Anlam büyük ölçüde korunur:

- "Ali okula gitti."
- "Okula Ali gitti."
- "Gitti Ali okula."

Bu üç cümle aynı temel anlamı taşır ancak vurgu farklıdır. Bu esneklik, sözdizimsel analizi zorlaştırır.

### 5.4 Sesli Uyum (Ünlü Uyumu)

Türkçe'de ekler, kök kelimedeki ünlülere göre değişir:

- ev + ler → evler
- araba + lar → arabalar
- göz + ler → gözler
- kutu + lar → kutular

Bu kural, ek tahmini ve morfolojik analiz için dikkate alınmalıdır.

### 5.5 Kaynak Azlığı

İngilizce ile karşılaştırıldığında Türkçe NLP için:
- Daha az etiketli veri seti mevcuttur.
- Daha az önceden eğitilmiş model bulunmaktadır.
- Araç ve kütüphane desteği daha sınırlıdır.

Ancak son yıllarda bu durum hızla değişmektedir. BERTurk, Zemberek gibi araçlar Türkçe NLP topluluğuna büyük katkı sağlamıştır.

### 5.6 Bitişik ve Ayrık Yazım Sorunları

Özellikle sosyal medya ve günlük yazışmalarda sık karşılaşılan bir sorundur:
- "her şey" mi, "herşey" mi?
- "hiçbir" mi, "hiç bir" mi?

Bu tür yazım tutarsızlıkları, metin ön işleme aşamasında ek zorluklar yaratır.

---

## 6. NLP'nin Tarihsel Gelişimi

NLP alanının gelişimini ana hatlarıyla bilmek, günümüzdeki yöntemlerin neden tercih edildiğini anlamak açısından önemlidir:

### 1950–1970: Kural Tabanlı Dönem
- Alan Turing'in "Makineler düşünebilir mi?" sorusu (1950)
- Noam Chomsky'nin dilbilgisi kuramları
- ELIZA chatbot'u (1966): Basit kural eşleştirmesiyle çalışan ilk sohbet robotu
- Bu dönemde NLP, elle yazılan kurallarla çalışıyordu ve çok sınırlı alanlarda başarılıydı.

### 1980–1990: İstatistiksel Yöntemlerin Yükselişi
- Büyük metin derlelerinin (korpus) oluşturulması
- İstatistiksel dil modellerinin ortaya çıkışı
- Olasılık tabanlı yöntemler kural tabanlı yaklaşımların yerini almaya başladı.

### 2000–2012: Makine Öğrenmesi Dönemi
- Naive Bayes, Destek Vektör Makineleri (SVM) gibi algoritmaların metin üzerinde kullanılması
- Elle özellik çıkarımı (feature engineering) hâlâ çok önemliydi.
- TF-IDF gibi temsil yöntemleri yaygınlaştı.

### 2013–2017: Kelime Gömmeleri (Word Embeddings)
- Word2Vec (2013): Kelimelerin yoğun vektörlerle temsil edilmesi
- GloVe: Global düzeyde kelime ilişkilerini yakalayan yöntem
- Bu dönemde kelimeler artık sadece sayılar değil, "anlamlı vektörler" haline geldi.
- "kral" - "erkek" + "kadın" ≈ "kraliçe" gibi ilişkiler matematiksel olarak ifade edilebildi.

### 2018–Günümüz: Transformer ve Büyük Dil Modelleri
- Transformer mimarisi (2017, "Attention is All You Need" makalesi)
- BERT (2018): Çift yönlü bağlam anlama yeteneği
- GPT serisi (2018–2024): Metin üretme konusunda devrim
- Bu dönemde NLP, insan seviyesine yakın performanslara ulaştı.

### Türkçe'ye Özel Gelişmeler
- **Zemberek:** Türkçe morfolojik analiz aracı
- **BERTurk:** Türkçe metinler üzerinde eğitilmiş BERT modeli
- **ITU NLP araçları:** İstanbul Teknik Üniversitesi'nin geliştirdiği Türkçe NLP araçları
- Türkçe NLP topluluğunun büyümesi ve açık kaynak katkıları

---

## 7. Uygulama Örnekleri ve Tartışma

Bu bölümde, öğrendiğimiz kavramları gerçek dünya uygulamalarıyla ilişkilendireceğiz.

### 7.1 Günlük Hayattan NLP Örnekleri

**Arama motorları:** Google'a bir soru yazdığınızda, arama motoru sorunuzu anlamak için NLP kullanır. "En yakın eczane nerede?" sorgusu, konum bilgisi + eczane kavramı + soru tipi olarak ayrıştırılır.

**Sesli asistanlar:** Siri, Google Asistan veya Alexa'ya konuştuğunuzda, önce konuşma metne çevrilir (Speech-to-Text), ardından NLP teknikleriyle metin analiz edilir ve uygun yanıt üretilir.

**E-posta filtreleme:** Gmail'in spam filtresi, gelen e-postaların içeriğini NLP ile analiz ederek spam olup olmadığını belirler.

**Otomatik düzeltme:** Telefonda yazarken çalışan otomatik düzeltme, karakter düzeyinde NLP ve dil modeli kullanır.

### 7.2 Tartışma Soruları

Sınıfta tartışılabilecek sorular:
- Bir arama motoru NLP'nin hangi alt alanlarını kullanıyor?
- Siri/Alexa gibi sesli asistanlar hangi NLP görevlerini bir arada kullanıyor?
- ChatGPT gibi büyük dil modelleri NLP'nin neresinde duruyor?
- Türkçe NLP neden İngilizce NLP'den daha zordur? Sadece kaynak azlığı mı, yoksa dilin yapısı da etkili mi?
- Otomatik çeviri sistemleri Türkçe'de neden bazen hatalı sonuç veriyor?

---

## Haftalık Ödev

**Görev 1: Elle NLP denemesi**
Aşağıdaki Türkçe cümleleri kağıt üzerinde (veya bir metin dosyasında) analiz edin:

Cümleler:
1. "İstanbul'da dün akşam şiddetli yağmur yağdı."
2. "Bu filmi çok beğendim, herkese tavsiye ederim."
3. "Mağazadan aldığım ürün bozuk çıktı, iade etmek istiyorum."

Her cümle için:
- Tokenlarına ayırın
- Stopword'leri belirleyin
- Köklerini bulmaya çalışın (stemming)
- Varsa varlık adlarını (NER) tespit edin
- Cümlenin duygusunu belirleyin (pozitif/negatif/nötr)

**Görev 2: Teknik hazırlık**
- Bilgisayarınıza Python kurun (Anaconda veya pip ile)
- NLTK kütüphanesini yükleyin
- Kısa rapor: "Kurulum sırasında karşılaştığınız sorunlar neler?" (yarım sayfa)

---

## Özet

Bu derste Doğal Dil İşleme'nin ne olduğunu, temel kavramlarını, görev haritasını, Türkçe'nin NLP açısından özelliklerini ve alanın tarihsel gelişimini öğrendik. Özellikle tokenizasyon, stopword, stemming, lemmatization ve morfolojik analiz kavramlarını örneklerle pekiştirdik. Önümüzdeki hafta Python ile metin işleme uygulamalarına geçeceğiz.
