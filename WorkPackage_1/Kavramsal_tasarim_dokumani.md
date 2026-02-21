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

### 3.1. Referans Çalışma ve Uyarlama Yaklaşımı

> **[NOT:]** Cui et al. (2023) makalesinin temel yaklaşımını 1 paragrafla özetleyin: SARIMA tabanlı kümeleme + federe Bi-LSTM. Ardından aşağıdaki tabloyla endüstriyel ortama uyarlama farklılıklarını ana hatlarıyla gösterin. Detaylı uyarlama analizi İP2 ve İP4'te yapılacak.

> | Konu | Makaledeki Yaklaşım | Collabreen'de Öngörülen Yaklaşım |
> |------|---------------------|----------------------------------|
> | Veri kaynağı | Ulusal karbon istatistikleri | SAP MES, IoT sensörleri, enerji ölçerler |
> | Veri frekansı | Yıllık/aylık | Gerçek zamanlı (dakika/saniye bazında) |
> | İstemci tanımı | Ülke bölgeleri | Üretim tesisleri/hatları |
> | Gizlilik | Yok | Secure Aggregation, Diferansiyel Gizlilik |
> | Gerçek zamanlılık | Offline | Stream processing (Kafka/Spark) |

> Bu tablo kavramsal düzeyde bir yol haritasıdır; kesin teknik kararlar İP2 (mimari tasarım) ve İP4 (model geliştirme) iş paketlerinde verilecektir.

### 3.2. Teknik Yaklaşımın Ana Bileşenleri

> **[NOT:]** Her bileşeni 2-3 cümleyle özetleyin. Detaya girmeyin, sadece "ne yapılacak ve neden" sorusuna cevap verin:

> **a) SARIMA Tabanlı Kümeleme:** Farklı üretim tesislerinin/hatlarının enerji tüketim ve emisyon profillerinin zaman serisi karakteristiklerine göre kümelenmesi. Benzer profildeki tesisler aynı FL kümesinde eğitilerek model doğruluğu artırılacaktır. Parametre belirleme stratejisi İP4'te detaylandırılacaktır.

> **b) Bi-LSTM / Hibrit Tahmin Modeli:** Zaman serisi tabanlı karbon emisyon tahmini için derin öğrenme yaklaşımı. XGBoost/Random Forest ile ensemble yapısı değerlendirilecektir. Model mimarisi ve hiperparametre arama stratejisi İP4'te belirlenecektir.

> **c) Federe Öğrenme Altyapısı:** Dağıtık model eğitimi için sunucu-istemci mimarisi. Senkron (FedAvg) ve asenkron (FedAsync) yaklaşımlar değerlendirilecektir. Framework seçimi ve mimari detaylar İP2 ve İP3'te kesinleşecektir.

> **d) Güvenlik ve Gizlilik Katmanı:** Secure Aggregation, Diferansiyel Gizlilik ve homomorfik şifreleme yöntemleri değerlendirilecektir. Uygulama detayları İP3'te ele alınacaktır.

> **e) Emisyon Faktörü Entegrasyonu:** ISO 14064 ve GHG Protocol kapsamında Scope 1, Scope 2 ve kısmen Scope 3 emisyon faktörlerinin modele entegrasyonu. Detaylar İP4'te çalışılacaktır.

> **f) Gerçek Zamanlı İşleme ve Dashboard:** Kafka/Spark Streaming tabanlı veri akışı ve karar destek mekanizması. İP5'te geliştirilecektir.

### 3.3. Başarı Ölçütleri (Hedef Değerler)

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

### 5.2. İş Paketleri Özet Planı ve Zaman Çizelgesi

> **[NOT:]** 6 iş paketinin özetini tablo halinde gösterin ve Gantt benzeri bir zaman çizelgesi ekleyin:

> | İP | Adı | Başlangıç | Bitiş | Süre | Ara Çıktı |
> |----|-----|-----------|-------|------|-----------|
> | 1 | Proje Yönetimi ve İş Paketleri Takibi | 01.09.2025 | 30.11.2026 | 455 gün | Kavramsal Tasarım Dokümanı (31.10.2025) |
> | 2 | Gereksinim Analizi ve Mimari Tasarım | 01.09.2025 | 30.11.2025 | 90 gün | Sistem Mimari Dokümanı + UML (30.11.2025) |
> | 3 | Federe Öğrenme Altyapısının Geliştirilmesi | 01.11.2025 | 30.04.2026 | 180 gün | Entegre Test Protokol Sonuç Raporu (30.04.2026) |
> | 4 | Karbon Ayak İzi Tahmin Modellerinin Geliştirilmesi | 01.12.2025 | 31.05.2026 | 181 gün | Hibrit Model Prototipi (31.05.2026) |
> | 5 | Gerçek Zamanlı Veri İşleme ve Dashboard | 01.02.2026 | 31.07.2026 | 180 gün | Kullanıcı Arayüz Geliştirmeleri (31.07.2026) |
> | 6 | Test, Entegrasyon ve Validasyon | 01.07.2026 | 30.11.2026 | 152 gün | Collabreen Final Ürün (30.11.2026) |

> Gantt çubuk grafiğini de ekleyin (proje önerisindeki zaman çizelgesini temel alarak).

### 5.3. İş Paketleri Arası Bağımlılıklar

> **[NOT:]** Ara çıktıların hangi iş paketinden hangi iş paketine aktığını gösteren basit bir bağımlılık diyagramı çizin:
> - İP1 (Kavramsal Tasarım) --> İP2'ye girdi
> - İP2 (Sistem Mimari Dokümanı) --> İP3'e girdi
> - İP3 (Protokol Sonuç Raporu) --> İP4'e girdi
> - İP4 (Hibrit Model Prototipi) --> İP5'e girdi
> - İP5 (Kullanıcı Arayüz) --> İP6'ya girdi
>
> Kritik yol: İP2 -> İP3 -> İP4 -> İP5 -> İP6 (paralel çalışan paketlerin çakışma alanlarını gösterin)

### 5.4. Ekip Yapısı ve Sorumluluk Matrisi (RACI)

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

### 5.5. Akademik Danışmanlık Planı

> **[NOT:]** Prof. Dr. Turgay Tugay Bilgin ile işbirliği yapısını tanımlayın:
> - Danışmanlık kapsamı: yapay zeka konularında akademik yönlendirme, TÜBİTAK dokümanlarının akademik değerlendirmesi, personellerin yönlendirilmesi
> - Toplantı sıklığı ve formatı (aylık/sprint bazlı, yüz yüze/online)
> - Kritik karar noktalarında akademik danışman onayı gerektiren konular (model seçimi, yöntem değişikliği vb.)
> - Bursa Teknik Üniversitesi Bilgisayar Mühendisliği Bölümü ile işbirliği kapsamı

### 5.6. Dokümantasyon ve Versiyon Yönetimi

> **[NOT:]** ISO 9001 ve ISO/IEC 27001 uyumlu dokümantasyon süreçlerini tanımlayın:
> - Doküman şablonları: her iş paketi çıktısı için standart şablonlar
> - Versiyonlama kuralları: major.minor.patch, değişiklik takibi
> - Onay akışı: kim hazırlıyor, kim review ediyor, kim onaylıyor
> - Confluence sayfa yapısı ve bilgi tabanı organizasyonu
> - Kod versiyonlama: Git branching stratejisi (GitFlow veya trunk-based)

### 5.7. İletişim Planı

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

## 7. Risk Analizi ve Yönetimi

> **[NOT - Firma için:]** İP1'in "risk yönetimi için risk matrix metodolojisi" taahhüdünün somut çıktısı bu bölüm olmalı.

### 7.1. Risk Matrisi

> **[NOT:]** Proje önerisindeki teknik belirsizlikleri risk formatına dönüştürün. Tablo halinde gösterin:

> | No | Risk | Olasılık | Etki | Risk Skoru | Azaltma Stratejisi | Sorumlu | İlgili İP |
> |----|------|----------|------|------------|-------------------|---------|-----------|
> | R1 | FL'nin endüstriyel ölçeğe uyarlanmasında performans sorunları | Orta | Yüksek | Yüksek | Küçük ölçekli PoC ile erken doğrulama, asenkron FL alternatifi | Sermet Ö. | İP3 |
> | R2 | Gizlilik katmanlarının model doğruluğuna olumsuz etkisi | Orta | Orta | Orta | DP epsilon parametresinin kademeli optimizasyonu | Şevval B. | İP3 |
> | R3 | Pilot tesisten yeterli kalitede veri alınamaması | Orta | Yüksek | Yüksek | Erken veri erişim anlaşması, sentetik veri alternatifi | Feyza C. | İP2 |
> | R4 | Heterojen veri kaynaklarının entegrasyon zorlukları | Yüksek | Orta | Yüksek | SAP MES entegrasyon deneyiminin kullanılması, OPC-UA/MQTT standartları | Sermet Ö. | İP2 |
> | R5 | Karbon tahmin modelinin hedef doğruluğa (R²>0.85) ulaşamaması | Orta | Yüksek | Yüksek | Hibrit modelleme, ensemble yaklaşım, akademik danışman desteği | İdris C. E. | İP4 |
> | R6 | Gerçek zamanlı işleme hedefinin (<5 sn) tutturulamaması | Düşük | Orta | Düşük | Kademeli optimizasyon, batch fallback stratejisi | Beyza D. | İP5 |
> | R7 | Personel kaybı | Düşük | Yüksek | Orta | Bilgi paylaşımı kültürü, çapraz eğitim, dokümantasyon | M.M. Bozkır | İP1 |
> | R8 | Pilot tesis işbirliğinde aksaklık | Düşük | Yüksek | Orta | Alternatif pilot tesis belirlenmesi, güçlü iletişim planı | M.M. Bozkır | İP2 |

### 7.2. Risk İzleme ve Güncelleme Planı

> **[NOT:]** Risklerin nasıl takip edileceğini belirtin:
> - Her sprint review'da risk tablosunun gözden geçirilmesi
> - Yeni risk tanımlama prosedürü
> - Risk skorunun değişmesi durumunda eskalasyon süreci
> - TÜBİTAK dönem raporlarında risk durumunun raporlanması

---

## 8. Standartlara Uyum Özeti

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

## 9. TÜBİTAK Raporlama ve Mali Yönetim Planı

> **[NOT - Firma için:]** Firmanın ilk TÜBİTAK projesi olduğu için bu bölüm hem iç referans hem de hakeme "raporlama sürecini biliyoruz" mesajı verir.

### 9.1. Dönemsel Raporlama Takvimi

> **[NOT:]** TÜBİTAK'ın beklediği raporlama dönemlerini ve her dönemde hangi iş paketlerinin tamamlanmış/devam ediyor olacağını belirtin:
> - I. Dönem (2025/2. yarı): İP1 devam, İP2 tamamlanmış, İP3 başlamış
> - II. Dönem (2026/1. yarı): İP3 tamamlanmış, İP4 tamamlanmış, İP5 devam
> - III. Dönem (2026/2. yarı): İP5 tamamlanmış, İP6 tamamlanmış, proje kapanışı

### 9.2. Dönem Raporu Hazırlık Süreci

> **[NOT:]** Her dönem raporu için:
> - Teknik ilerleme raporunun nasıl hazırlanacağı (kim hazırlıyor, kim onaylıyor)
> - Mali belgelerin toplanma ve düzenlenme süreci
> - YMM raporu süreci
> - Akademik danışman değerlendirmesinin rapora dahil edilmesi
> - Hakem ziyaretine hazırlık: hangi dokümanlar, demolar ve kanıtlar sunulacak

### 9.3. Harcama Takip Mekanizması

> **[NOT:]** Personel giderleri, Ar-Ge test kuruluşlarına yapılan ödemeler ve diğer maliyet kalemlerinin nasıl takip edileceğini, fatura/belge arşivleme yöntemini kısaca belirtin. Dönemsel gider tablosundaki bütçe dağılımına atıfta bulunun.

---

## 10. Sonraki Adımlar: İP2'ye Geçiş

> **[NOT - Firma için:]** Bu doküman İP2'ye girdi oluşturuyor. Bu bölümde köprüyü net kurun.

### 10.1. İP2'de Yapılacak Çalışmaların Özeti

> **[NOT:]** İP2 faaliyetlerini ve bu kavramsal tasarımdan nasıl besleneceklerini kısaca belirtin:
> - Kullanıcı Gereksinimlerinin Toplanması: Pilot tesis stratejisi (Bölüm 6) temel alınacak
> - FL İçin Sistem Mimarisi Tasarımı: Kavramsal mimari (Bölüm 4) detaylandırılacak, UML diyagramları üretilecek
> - Veri Pipeline Tasarımı: Yöntem ana hatları (Bölüm 3) temelinde ETL detay tasarımı yapılacak
> - Testler: Başarı ölçütleri (Bölüm 3.3) doğrultusunda test senaryoları oluşturulacak

### 10.2. Açık Kalan Konular

> **[NOT:]** Kavramsal aşamada henüz kesinleşmemiş konuları dürüstçe listeleyin. Bu hakemde güven oluşturur:
> - FL framework kesin seçimi (İP2'de PoC ile netleşecek)
> - Haberleşme protokolü kesin seçimi (İP2'de değerlendirilecek)
> - SARIMA parametre kalibrasyonu (pilot verisi ile İP4'te belirlenecek)
> - Pilot tesisteki veri erişim kapsamı (İP2'de tesis ziyaretiyle netleşecek)
> - Asenkron vs. senkron FL performans karşılaştırması (İP3'te yapılacak)

---

## Ekler

### Ek A: Kısaltmalar ve Terimler Sözlüğü

> **[NOT:]** FL, SARIMA, Bi-LSTM, DP, ETL, MES, IoT, OEE, gRPC, MQTT, KVKK, GDPR, GHG, SKDM, QFD, CPM, RACI vb.

### Ek B: Proje Zaman Çizelgesi (Gantt Grafiği)

> **[NOT:]** Proje önerisindeki iş zaman çubuk grafiğinin güncel ve detaylı versiyonu.

### Ek C: Kavramsal Mimari Diyagramı (Büyük Boy)

> **[NOT:]** Bölüm 4.1'deki üst düzey mimari diyagramın büyük boyutlu versiyonu.
