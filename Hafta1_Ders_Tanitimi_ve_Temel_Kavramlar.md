
# HAFTA 1: Ders Tanıtımı ve Temel Kavramlar

## Dersin Genel Bilgileri

| Bilgi | Açıklama |
|---|---|
| **Ders Adı** | Metin Madenciliği |
| **Hafta** | 1 |
| **Süre** | 2 Saat |
| **Konu** | Ders Tanıtımı ve Temel Kavramlar |
| **Kaynak Kitap** | Uygulamalı Metin Madenciliği: Doğal Dil İşleme ve Python ile |

---

## Öğrenme Hedefleri

Bu dersin sonunda öğrenciler:

- Dersin işleyişini, değerlendirme yöntemlerini ve dönem projesi beklentisini kavrayacaktır.
- Metin madenciliğinin ne olduğunu kendi cümleleriyle açıklayabilecektir.
- Metin madenciliğinin temel problem tiplerini tanıyacaktır.
- Bir metin madenciliği projesinin uçtan uca iş akışını genel hatlarıyla çizebilecektir.
- Farklı sektörlerdeki gerçek hayat kullanım alanlarını örneklerle eşleştirebilecektir.

---

## 1. Dersin Yapısı ve Değerlendirme

### 1.1 Dersin İşleyişi

Bu ders, 14 hafta boyunca **teori ve uygulama** birlikteliğiyle işlenecektir. Her hafta belirli bir konu ele alınacak, konuyla ilgili temel kavramlar açıklandıktan sonra gerçek dünya örnekleri üzerinden tartışmalar yapılacaktır. Dersin son haftası dönem projesi sunumlarına ayrılmıştır.

Haftalık yapı şu şekilde planlanmıştır:
- **Teorik anlatım:** Kavramlar, yöntemler ve yaklaşımlar
- **Tartışma:** Gerçek dünya örnekleri üzerinden sınıf içi tartışma
- **Uygulama:** Basit örneklerle kavramların pekiştirilmesi
- **Okuma görevi:** Bir sonraki haftaya hazırlık için kaynak okuma

### 1.2 Değerlendirme Yöntemi

| Değerlendirme Ölçütü | Ağırlık |
|---|---|
| Ödevler ve Mini Laboratuvar Çalışmaları | %30 |
| Ara Sınav veya Quiz | %20 |
| Dönem Projesi | %40 |
| Derse Katılım | %10 |

### 1.3 Dönem Projesi Hakkında

Dönem projesi, öğrencilerin derste öğrendikleri bilgileri gerçek bir problem üzerinde uygulamalarını amaçlar. Proje bireysel veya 2 kişilik gruplar halinde yapılabilir.

**Örnek proje konuları:**
- Türkçe ürün yorumlarında duygu analizi
- Haber metinlerinin otomatik sınıflandırılması
- Sosyal medya paylaşımlarından konu çıkarımı
- Müşteri şikayetlerinin otomatik kategorize edilmesi
- Türkçe metinlerin otomatik özetlenmesi

---

## 2. Metin Madenciliği Nedir?

### 2.1 Tanım

Metin madenciliği, **yapılandırılmamış (unstructured) metin verilerinden anlamlı bilgi, örüntü ve içgörü çıkarma** sürecidir. Günümüzde üretilen verinin büyük çoğunluğu metin formatındadır: e-postalar, sosyal medya paylaşımları, haberler, raporlar, yorumlar ve daha fazlası. Bu devasa metin yığınından elle bilgi çıkarmak neredeyse imkansızdır. İşte metin madenciliği tam da bu noktada devreye girer.

Metin madenciliği; istatistik, makine öğrenmesi, doğal dil işleme ve bilgi bilimi gibi disiplinlerin kesişim noktasında yer alır. Amaç, büyük miktarda metni otomatik olarak analiz ederek içindeki gizli bilgileri, eğilimleri ve kalıpları ortaya çıkarmaktır.

### 2.2 Neden Metin Madenciliğine İhtiyaç Duyuyoruz?

Günlük hayatta karşılaştığımız birkaç senaryo üzerinden düşünelim:

- Bir e-ticaret sitesinde binlerce ürün yorumu var. Hangi ürünler beğeniliyor, hangileri beğenilmiyor? Hepsini tek tek okumak mümkün mü?
- Bir banka, müşteri şikayetlerini analiz etmek istiyor. Binlerce şikayet metnini elle sınıflandırmak ne kadar sürer?
- Bir haber sitesi, günde yüzlerce haber yayınlıyor. Bu haberleri otomatik olarak "spor", "ekonomi", "siyaset" gibi kategorilere ayırmak mümkün mü?

Bu soruların hepsine metin madenciliği yöntemleriyle cevap verilebilir.

### 2.3 Veri Kaynakları

Metin madenciliğinde kullanılan başlıca veri kaynakları şunlardır:

- **Sosyal medya:** Twitter/X paylaşımları, Instagram yorumları, Facebook gönderileri
- **E-ticaret platformları:** Ürün yorumları, satıcı değerlendirmeleri
- **Çağrı merkezi kayıtları:** Müşteri görüşme notları, şikayet kayıtları
- **Haber siteleri:** Haber metinleri, köşe yazıları
- **Hukuki belgeler:** Mahkeme kararları, sözleşmeler, kanun metinleri
- **Tıbbi dokümanlar:** Hasta notları, klinik raporlar, tıbbi literatür
- **Akademik yayınlar:** Bilimsel makaleler, tezler
- **Forum ve bloglar:** Kullanıcı tartışmaları, kişisel yazılar

---

## 3. Temel Kavramlar

### 3.1 Doküman

Metin madenciliğinde işlenen en temel birimdir. Bir doküman; tek bir tweet, bir ürün yorumu, bir haber metni veya bir kitap bölümü olabilir. Bağlama göre dokümanın boyutu değişir.

**Örnek:** "Bu telefon gerçekten harika, kamerası çok iyi!" → Bu tek bir dokümandır.

### 3.2 Korpus (Derlem)

Birden fazla dokümanın bir araya gelmesiyle oluşan veri kümesidir. Bir metin madenciliği projesinde genellikle bir korpus üzerinde çalışılır.

**Örnek:** Bir e-ticaret sitesinden toplanan 10.000 ürün yorumu bir korpus oluşturur.

### 3.3 Token

Bir metnin en küçük anlamlı parçasıdır. Genellikle kelimeler token olarak kabul edilir, ancak noktalama işaretleri ve sayılar da birer token olabilir.

**Örnek:** "Hava bugün çok güzel." → Tokenlar: [Hava, bugün, çok, güzel, .]

### 3.4 Etiket (Label)

Bir dokümanın ait olduğu sınıf veya kategoridir. Özellikle sınıflandırma problemlerinde kullanılır.

**Örnek:** Bir yorum "pozitif" veya "negatif" etiketine sahip olabilir.

### 3.5 Özellik (Feature)

Bir modelin karar verirken kullandığı sayısal değerlerdir. Metin verisi doğrudan sayısal olmadığı için, metinlerin önce sayısal özelliklere dönüştürülmesi gerekir.

**Örnek:** "güzel" kelimesinin bir metinde 3 kez geçmesi, o metne ait bir özelliktir.

### 3.6 Temsil (Representation)

Metnin sayısal forma dönüştürülmüş halidir. Bilgisayarlar metni doğrudan anlayamaz; bu yüzden metin, vektörler veya matrisler gibi sayısal yapılara çevrilmelidir. Bu dönüşüm işlemine "temsil" denir.

---

## 4. Metin Madenciliği İş Akışı (Pipeline)

Bir metin madenciliği projesi genellikle aşağıdaki adımları takip eder. Bu adımlar zinciri "pipeline" olarak adlandırılır:

### Adım 1: Problem Tanımı
Hangi soruya cevap arıyoruz? Ne tür bir çıktı bekliyoruz? Örneğin: "Müşteri yorumlarının olumlu mu olumsuz mu olduğunu otomatik belirlemek istiyoruz."

### Adım 2: Veri Toplama
Probleme uygun metin verilerinin toplanması. Web kazıma (web scraping), API kullanımı veya hazır veri setleri bu adımda kullanılabilir.

### Adım 3: Temizleme ve Ön İşleme
Ham metin verisi genellikle gürültülüdür. Bu adımda gereksiz karakterler temizlenir, kelimeler küçük harfe çevrilir, anlam taşımayan kelimeler (stopwords) çıkarılır ve metinler standart bir forma getirilir.

### Adım 4: Temsil (Sayısallaştırma)
Temizlenen metinler sayısal formlara dönüştürülür. Bu aşamada Bag of Words, TF-IDF veya Word Embedding gibi yöntemler kullanılır.

### Adım 5: Modelleme
Sayısallaştırılmış veriler üzerinde makine öğrenmesi veya derin öğrenme algoritmaları uygulanır.

### Adım 6: Değerlendirme
Modelin başarısı çeşitli metriklerle ölçülür. Doğruluk (accuracy), hassasiyet (precision), duyarlılık (recall) ve F1 skoru en yaygın kullanılan metriklerdir.

### Adım 7: Hata Analizi ve İyileştirme
Modelin yanlış yaptığı örnekler incelenir. Neden hata yaptığı anlaşılmaya çalışılır ve modelde iyileştirmeler yapılır.

### Adım 8: Raporlama veya Dağıtım
Sonuçlar raporlanır veya model bir uygulamaya entegre edilir.

---

## 5. Problem Tipleri

Metin madenciliğinde karşılaşılan başlıca problem tipleri şunlardır:

### 5.1 Metin Sınıflandırma
Bir metni önceden belirlenmiş kategorilerden birine atama işlemidir. En yaygın metin madenciliği problemidir.

**Örnekler:**
- E-posta spam tespiti (spam / spam değil)
- Duygu analizi (pozitif / negatif / nötr)
- Haber kategorileme (spor / ekonomi / siyaset / magazin)

### 5.2 Bilgi Çıkarımı (Information Extraction)
Metin içinden yapılandırılmış bilgi parçalarının çıkarılmasıdır.

**Örnekler:**
- Bir haber metninden kişi adları, kurum adları ve yer adlarının tespiti (Varlık Adı Tanıma - NER)
- Bir sözleşmeden tarih, miktar ve taraf bilgilerinin çıkarılması

### 5.3 Kümeleme (Clustering)
Benzer metinlerin otomatik olarak gruplandırılmasıdır. Sınıflandırmadan farkı, önceden belirlenmiş kategorilerin olmamasıdır. Sistem, benzer metinleri kendisi gruplar.

**Örnekler:**
- Benzer haberlerin gruplanması
- Müşteri şikayetlerinin konu bazlı kümelenmesi

### 5.4 Konu Modelleme (Topic Modeling)
Büyük bir metin koleksiyonunda gizli konu yapılarının keşfedilmesidir.

**Örnekler:**
- Binlerce akademik makalenin ana konularının belirlenmesi
- Sosyal medyada en çok konuşulan 10 konunun tespiti

### 5.5 Metin Özetleme
Uzun bir metnin anlamını koruyarak kısaltılmasıdır.

**Örnekler:**
- Bir haber metninin 2-3 cümleyle özetlenmesi
- Uzun bir raporun yönetici özeti haline getirilmesi

### 5.6 Metin Benzerliği ve Öneri
İki metin arasındaki benzerliğin ölçülmesi ve buna dayalı öneriler sunulmasıdır.

**Örnekler:**
- Benzer iş ilanlarının önerilmesi
- "Bu ürünü alan kişiler şunları da aldı" sistemleri

---

## 6. Kullanım Alanları (Sektörel Örnekler)

### Finans
Finansal haberler analiz edilerek piyasa duyarlılığı ölçülebilir. Örneğin, bir şirketle ilgili olumsuz haber akışı, hisse senedi fiyatındaki düşüşün erken sinyali olabilir. Ayrıca müşteri şikayetlerinin analizi ile risk değerlendirmesi yapılabilir.

### Sağlık
Hasta notları ve klinik raporlar analiz edilerek hastalık örüntüleri tespit edilebilir. Tıbbi literatürde otomatik arama ve özetleme yapılabilir. Ancak bu alanda **hasta mahremiyeti ve etik kurallar** büyük önem taşır.

### Hukuk
Mahkeme kararları arasında benzerlik araması yapılabilir, içtihat tarama otomatikleştirilebilir. Sözleşmelerdeki riskli maddeler otomatik olarak tespit edilebilir.

### İnsan Kaynakları
Özgeçmişler (CV) otomatik olarak taranarak pozisyona en uygun adaylar belirlenebilir. Ancak bu tür sistemlerde **önyargı (bias) riski** göz önünde bulundurulmalıdır. Eğitim verisindeki önyargılar modele yansıyabilir.

### E-Ticaret
Ürün yorumları analiz edilerek müşteri memnuniyeti ölçülebilir, en çok şikayet edilen konular belirlenebilir ve ürün önerileri iyileştirilebilir.

### Kamu
Vatandaş dilekçeleri ve şikayetleri otomatik olarak kategorize edilebilir. Sosyal medya üzerinden kamuoyu duyarlılığı izlenebilir.

### Eğitim
Öğrenci ödevlerinde intihal tespiti yapılabilir. Açık uçlu sınav cevapları otomatik olarak değerlendirilebilir.

---

## Haftalık Ödev

**Görev:**
1. İnternetten 10-20 adet kısa Türkçe metin toplayın (ürün yorumları, haber başlıkları, tweetler vb.)
2. Bu metinleri bir metin dosyasına kaydedin.
3. Aşağıdaki soruları cevaplayarak 1 sayfalık bir problem tanımı yazın:
   - Bu veri setinde çözmek istediğiniz problem nedir?
   - Bu problem hangi problem tipine girer? (sınıflandırma, kümeleme, özetleme vb.)
   - Bu problemin çözülmesi kime fayda sağlar?

---

## Özet

Bu derste metin madenciliğinin ne olduğunu, neden önemli olduğunu, temel kavramları, iş akışını, problem tiplerini ve farklı sektörlerdeki kullanım alanlarını öğrendik. Önümüzdeki haftalarda bu temelin üzerine doğal dil işleme teknikleri ve uygulama yöntemlerini ekleyeceğiz.
