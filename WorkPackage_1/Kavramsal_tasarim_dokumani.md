# COLLABREEN: Federe Öğrenme ile Karbon Ayak İzi Tahminleme ve Yeşil Üretim Çözümleri

## Kavramsal Tasarım Dokümanı

**Proje No:** 181553  
**Hazırlayan:** Digitanol Yazılım A.Ş.  
**Akademik Danışman:** Prof. Dr. Turgay Tugay Bilgin, Bursa Teknik Üniversitesi  
**Versiyon:** 1.0  
**Tarih:** [GG.AA.2025]  

---

## Doküman Revizyonu

| Versiyon | Tarih | Değişiklik Açıklaması | Hazırlayan |
|----------|-------|----------------------|------------|
| 1.0 | [Tarih] | İlk sürüm | [İsim] |

---

## 1. Giriş ve Amaç

### 1.1. Dokümanın Amacı ve Kapsamı

> **[NOT:]** Bu dokümanın İP1 (Proje Yönetimi ve İş Paketleri Takibinin Yapılması) ara çıktısı olarak üretildiğini belirtin. Dokümanın amacı, projenin kavramsal çerçevesini çizmek, kullanılacak yöntemlerin ana hatlarını belirlemek ve İP2'ye (Gereksinim Analizi ve Mimari Tasarım) yol gösterici bir referans oluşturmaktır. Teknik detaylar (model hiperparametreleri, kesin mimari kararları vb.) İP2 ve sonraki iş paketlerinde ele alınacaktır. 

### 1.2. Projenin Kısa Özeti

> **[NOT:]** 1-2 paragrafla projenin ne yaptığını, kimin için yaptığını ve neden yaptığını özetleyin. Proje önerisinin tekrarı olmasın; daha çok "bu kavramsal tasarımla projenin yol haritasını nasıl çiziyoruz" perspektifinden yazılsın.

---

## 2. Problem Tanımı ve Projenin Gerekçesi

> **[NOT:]** Bu bölüm kısa ve öz olmalı. Proje önerisindeki bilgileri tekrarlamak yerine, kavramsal tasarımı neden yaptığınızı bağlama oturtan bir giriş niteliğinde olmalı.

### 2.1. Endüstriyel Karbon Ayak İzi Takibindeki Boşluk

> **[NOT:]** 1-2 paragraf: Türk ihracatçılarının AB SKDM (Sınırda Karbon Düzenleme Mekanizması) baskısıyla karşı karşıya olduğunu, mevcut çözümlerin merkezi veri toplama gerektirdiğini ve gerçek zamanlı tahmin yapamadığını özetleyin. Collabreen'in bu boşluğu federe öğrenme yaklaşımıyla dolduracağını belirtin.

### 2.2. Federe Öğrenme Tercihinin Gerekçesi

> **[NOT:]** 1 paragraf: Neden merkezi ML yerine FL tercih edildiğini kısaca açıklayın (veri gizliliği, KVKK/GDPR, ticari sır, çoklu tesis). Detaylı teknik karşılaştırma İP2'de yapılacak; burada sadece stratejik gerekçe yeterli.

---

## 3. Kullanılacak Yöntemlerin Ana Hatları

> **[NOT - Firma için:]** Bu bölüm İP1'in "Kullanılacak Yöntemlerin Ana Hatlarının Belirlenmesi" faaliyetinin doğrudan çıktısıdır. Teknik derinliğe girmeden, projede izlenecek metodolojik yaklaşımı üst düzeyde tanımlayın. Hakem burada "ekip ne yapacağını biliyor, yol haritası net" mesajı aramalı.

### 3.1. Teknik Yaklaşımın Ana Bileşenleri

> **[NOT:]** Her bileşeni 2-3 cümleyle özetleyin. Detaya girmeyin, sadece "ne yapılacak ve neden" sorusuna cevap verin:

> **a) SARIMA Tabanlı Kümeleme:** Farklı üretim tesislerinin/hatlarının enerji tüketim ve emisyon profillerinin zaman serisi karakteristiklerine göre kümelenmesi. Benzer profildeki tesisler aynı FL kümesinde eğitilerek model doğruluğu artırılacaktır. Parametre belirleme stratejisi İP4'te detaylandırılacaktır.

> **b) Bi-LSTM / Hibrit Tahmin Modeli:** Zaman serisi tabanlı karbon emisyon tahmini için derin öğrenme yaklaşımı. XGBoost/Random Forest ile ensemble yapısı değerlendirilecektir. Model mimarisi ve hiperparametre arama stratejisi İP4'te belirlenecektir.

> **c) Federe Öğrenme Altyapısı:** Dağıtık model eğitimi için sunucu-istemci mimarisi. Senkron (FedAvg) ve asenkron (FedAsync) yaklaşımlar değerlendirilecektir. Framework seçimi ve mimari detaylar İP2 ve İP3'te kesinleşecektir.

> **d) Güvenlik ve Gizlilik Katmanı:** Secure Aggregation, Diferansiyel Gizlilik ve homomorfik şifreleme yöntemleri değerlendirilecektir. Uygulama detayları İP3'te ele alınacaktır.

> **e) Emisyon Faktörü Entegrasyonu:** ISO 14064 ve GHG Protocol kapsamında Scope 1, Scope 2 ve kısmen Scope 3 emisyon faktörlerinin modele entegrasyonu. Detaylar İP4'te çalışılacaktır.

> **f) Gerçek Zamanlı İşleme ve Dashboard:** Kafka/Spark Streaming tabanlı veri akışı ve karar destek mekanizması. İP5'te geliştirilecektir.

### 3.2. Başarı Ölçütleri (Hedef Değerler)

> **[NOT:]** Proje önerisinde taahhüt edilen 4 başarı ölçütünü tablo halinde hatırlatın:

> | Başarı Ölçütü | Hedeflenen Değer | Ölçüleceği İş Paketi |
> |---------------|------------------|---------------------|
> | Federe Öğrenme Platformu Prototipi | %85+ sistem uptime | İP3, İP6 |
> | Karbon Ayak İzi Tahmin Modeli | R² > 0.85, MAE < %10 | İP4, İP6 |
> | Veri Gizliliği ve Güvenlik | %80+ gizlilik, <%10 kayıp | İP3, İP6 |
> | Gerçek Zamanlı İşleme Hızı | <5 sn gecikme, min 10.000 kayıt/dk | İP5, İP6 |

> Bu hedeflerin nasıl ölçüleceği İP6 (Test, Entegrasyon ve Validasyon) kapsamında detaylandırılacaktır.

---

## 4. Kavramsal Sistem Mimarisi (Üst Düzey)

> **[NOT - Firma için:]** Bu bölüm teknik derinlik değil, büyük resim sunmalı. Basit bir blok diyagram yeterli. Amaç hakeme "sistem neye benzeyecek" sorusunun cevabını vermek. Detaylı UML ve DFD diyagramları İP2'nin çıktısı olacak.

### 4.1. Sistem Genel Görünümü

> **[NOT:]** Tüm sistemin üst düzey blok diyagramını çizin. Karmaşık olmasın, 5-6 ana blok ve aralarındaki veri akışı okları yeterli:
>
> ```
> [Veri Kaynakları] --> [Veri Toplama / ETL] --> [Federe Öğrenme Katmanı] --> [Tahmin Modeli] --> [Dashboard / Karar Destek]
>       |                                              |
>       |                                    [Güvenlik Katmanı]
>       |
> (MES, IoT, Enerji Ölçer)
> ```
>
> Bu diyagramı draw.io veya Visio ile temiz bir şekilde çizin. Detaylı mimari diyagramlar (bileşen, dağıtım, sıralama) İP2'de üretilecektir.

### 4.2. Katmanların Kısa Açıklaması

> **[NOT:]** Her katmanı 2-3 cümleyle tanımlayın:
> - **Veri Kaynakları:** SAP MES, IoT sensörleri, enerji ölçerler. Veri tipleri ve formatları İP2'de pilot tesisle netleşecek.
> - **Veri Toplama / Ön İşleme:** ETL pipeline, normalizasyon, eksik veri yönetimi. Pipeline detay tasarımı İP2'de yapılacak.
> - **Federe Öğrenme:** Sunucu-istemci yapısı, model aggregation. Mimari kararlar İP2'de, geliştirme İP3'te.
> - **Tahmin Modeli:** SARIMA kümeleme + Bi-LSTM hibrit. Model geliştirme İP4'te.
> - **Güvenlik:** DP, Secure Aggregation. Detaylı tasarım İP3'te.
> - **Dashboard / Karar Destek:** Web/mobil arayüz, what-if analizi. Geliştirme İP5'te.

---

## 5. Proje Yönetim Yaklaşımı

> **[NOT - Firma için:]** Bu bölüm dokümanın en ağırlıklı bölümü olmalı. İP1'in ana faaliyet alanı proje yönetimi ve organizasyondur. Hakem burada sistematik bir yönetim yaklaşımı görmek ister.

### 5.1. Agile/Scrum Uygulama Planı

> **[NOT:]** Detaylı olarak açıklayın:
> - 2 haftalık sprint döngüsü: sprint planlama, daily standup (ne sıklıkla, hangi araçla), sprint review, retrospective
> - Sprint döngülerinin iş paketleriyle uyumu: her iş paketi kaç sprint'e denk geliyor
> - Product backlog yönetimi: user story formatı, priority sıralaması, acceptance criteria tanımlama standardı
> - Jira board yapısı: hangi sütunlar (To Do, In Progress, Review, Done), workflow kuralları
> - Confluence kullanımı: sayfa yapısı, şablon standardları, bilgi tabanı organizasyonu
> - Burndown chart ve velocity tracking: nasıl raporlanacak

### 5.2. Ekip Yapısı ve Sorumluluk Matrisi (RACI)

> **[NOT:]** Tüm ekip üyelerinin projedeki rollerini ve iş paketlerine dağılımını RACI matrisi ile gösterin:

> | Faaliyet / İş Paketi | M.M. Bozkır | Sermet Ö. | Feyza C. | Şevval B. | Beyza D. | İdris C. E. | Prof. Dr. Bilgin |
> |----------------------|-------------|-----------|----------|-----------|----------|-------------|-----------------|
> | İP1 - Proje Yönetimi | R/A | I | I | I | I | I | C |
> | İP2 - Gereksinim Analizi | A | R | R | R | I | R | C |
> | İP3 - FL Altyapısı | A | R | I | R | R | R | C |
> | İP4 - Tahmin Modelleri | A | R | R | R | I | R | C |
> | İP5 - Dashboard | A | R | I | R | R | R | C |
> | İP6 - Test/Validasyon | A | I | R | I | I | R | C |
>
> R: Responsible (Sorumlu), A: Accountable (Hesap Verebilir), C: Consulted (Danışılan), I: Informed (Bilgilendirilen)
>
> Her ekip üyesinin adam/ay oranlarını ve hangi aylarda hangi iş paketinde aktif olduğunu gösteren bir personel yük dağılımı tablosu da ekleyin.

### 5.3. Dokümantasyon ve Versiyon Yönetimi

> **[NOT:]** ISO 9001 ve ISO/IEC 27001 uyumlu dokümantasyon süreçlerini tanımlayın:
> - Doküman şablonları: her iş paketi çıktısı için standart şablonlar
> - Versiyonlama kuralları: major.minor.patch, değişiklik takibi
> - Onay akışı: kim hazırlıyor, kim review ediyor, kim onaylıyor
> - Confluence sayfa yapısı ve bilgi tabanı organizasyonu
> - Kod versiyonlama: Git branching stratejisi (GitFlow veya trunk-based)

### 5.4. İletişim Planı

> **[NOT:]** Proje içi ve dışı iletişim yapısını belirleyin:
> - Ekip içi: daily standup, sprint toplantıları, Slack/Teams kanalları
> - Akademik danışman: toplantı takvimi ve iletişim kanalı
> - Pilot tesis (HP Pelzer Pimsa): iletişim sorumlusu, toplantı sıklığı
> - TÜBİTAK izleyici: raporlama takvimi, iletişim protokolü
> - Karar toplantıları: ne zaman, kim katılır, nasıl kayıt altına alınır

---

## 6. Pilot Tesis Stratejisi

> **[NOT - Firma için:]** Hakem "gerçek saha ile temas var mı" sorusunu sorar. Kısa ama somut olsun.

### 6.1. HP Pelzer Pimsa İşbirliği

> **[NOT:]** Kısaca:
> - Pilot tesisi tanıtın (sektör, genel profil)
> - Niyet mektubunun alındığını belirtin
> - İP2'de yapılacak ilk tesis ziyareti ve gereksinim toplama planını özetleyin
> - İletişim sorumlusunun kim olduğunu ve koordinasyon yöntemini belirtin

### 6.2. Pilot Uygulama Yol Haritası

> **[NOT:]** Pilot uygulamanın hangi iş paketlerinde hangi aşamalarda gerçekleşeceğini üst düzeyde gösterin:
> - İP2: Gereksinim toplama ve veri erişim planlaması
> - İP3: FL istemcisinin tesis ortamına kavramsal konumlandırılması
> - İP4: Örnek veri üzerinde model eğitimi
> - İP5: Dashboard pilot kullanımı
> - İP6: Saha validasyonu ve kullanıcı kabul testleri

---

## 7. Standartlara Uyum Stratejisi

> **[NOT:]** Kısa bir tablo halinde, projenin uyum sağlaması gereken standartları ve bunların hangi iş paketinde ele alınacağını gösterin:

> | Standart | Kapsam | İlgili İP |
> |----------|--------|-----------|
> | ISO 14064 | Sera gazı hesaplama ve raporlama | İP4 |
> | GHG Protocol | Scope 1/2/3 emisyon sınıflandırması | İP4 |
> | ISO/IEC 27001 | Bilgi güvenliği yönetimi | İP3, Proje geneli |
> | ISO/IEC 27701 | Gizlilik yönetimi | İP3 |
> | ISO/IEC 25010 | Yazılım ürün kalitesi | İP6 |
> | OPC-UA / MQTT | Endüstriyel veri iletişim protokolleri | İP2 |
> | KVKK / GDPR | Kişisel veri koruma | Proje geneli |

---


