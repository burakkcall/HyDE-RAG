

# HyDE RAG
## **Hipotetik(VARSAYIMSAL) Doküman Gömme (Hypothetical Document Embeddings)** ile Bilgi Getirimini Güçlendirme

* **Açıklama** - HyDE RAG  — Hipotetik veya VARSAYIMSAL Doküman Gömmelerinin geleneksel RAG'e kıyasla bilgi getirimini nasıl iyileştirdiğini öğrenin ve adım adım eksiksiz HyDE mimarisini uygulayın.


---

$$\text{Sorgu} \;\xrightarrow{\;gömme\;}\; \text{Arama Motoru (Top-}k\text{)} \;\xrightarrow{\;bağlamı\;sorguyla\;birleştir\;}\; \text{LLM} \;\longrightarrow\; \text{Nihai Yanıt}$$

**Sorgu bir vektöre dönüştürülür (embed edilir)**, arama motoru en yakın doküman parçalarını (chunks) bulur ve dil modeli bu parçaları bağlam olarak kullanarak bir yanıt üretir.

---

## Geleneksel RAG Nerelerde Zayıf Kalır?

En zayıf halka **sorgu gömme (query embedding)** adımıdır.

* Kullanıcı sorguları genellikle kısadır, belirsizdir veya dokümanların yazım dilinden çok farklı bir dille yazılmıştır.
* Örnek: *“PINNs kararlılık sorunları”*. Bu sorgunun vektör temsili seyrektir ve *“Diferansiyel denklem kalıntılarının sertliğinden kaynaklanan Fizik Bilgili Yapay Sinir Ağları eğitim kararsızlıkları”* şeklinde yazılmış bir metin parçasıyla tam eşleşmeyebilir.
* Arama başarısız olursa, LLM yanıtını bir kaynağa dayandıramaz ve uydurmaya (halüsinasyon görmeye) başlayabilir.

Özetle sorun: **çöp girer → çöp çıkar**. Bilgi getirimi zayıf olduğunda, tüm mimari olumsuz etkilenir.

---

## Bir Çözüm Olarak HyDE

**HyDE (Hypothetical Document Embeddings - Varsayımsal Doküman Gömmeleri)** zekice bir çözüm sunar. Ham sorguyu doğrudan vektöre dönüştürmek yerine, önce LLM'den soruya **kısa ve gerçekçi görünen varsayımsal bir yanıt** üretmesini (halüsinasyon görmesini) isteriz.

Örneğin soru şu olsun: *‘PINN'lerde sıkıştırılamazlık nasıl sağlanır?’*, varsayımsal yanıt şöyle olabilir:

*‘Sıkıştırılamaz akışlar için Fizik Bilgili Yapay Sinir Ağlarında, ıraksama olmaması (divergence-free) kısıtlamaları, genellikle otomatik türev kullanılarak kayıp fonksiyonundaki hız alanının ıraksamasını cezalandırarak sağlanır.’*

Bu "varsayımsal yanıt", veri kümesindeki gerçek bir pasaja çok benzer: tam cümleler, teknik terimler ve açıklayıcı ifadeler içerir. Bu nedenle, vektör temsili gerçek ilgili parçalara çok daha yakındır.

HyDE ile iş akışının değişimi şu şekildedir:

$$\text{Sorgu} \;\xrightarrow{\;LLM\;üretir\;}\; \text{Varsayımsal Yanıt} \;\xrightarrow{\;gömme\;}\; \text{Arama Motoru (Top-}k\text{)} \;\xrightarrow{\;bağlamı\;sorguyla\;birleştir\;}\; \text{LLM} \;\longrightarrow\; \text{Nihai Yanıt}$$

Dolayısıyla tek değişiklik başlangıçtaki ekstra adımdır: **sorgu → varsayımsal yanıt → gömme**.

---

## HyDE Neden Daha İyi Çalışır?

* Varsayımsal yanıt, ham sorguya göre anlamsal (semantik) olarak çok daha zengindir.
* Dokümanların yazım tarzını yansıtır, böylece vektör benzerliği çok daha iyi hizalanır.
* Özellikle teknik veya uzmanlık gerektiren alanlarda bilgi getirim başarısı (recall) artar.

Özetle HyDE, geleneksel RAG'in en zayıf halkasını (sorgu gömmeleri) ek bir karşılık LLM çağrısı ile güçlendirir.

