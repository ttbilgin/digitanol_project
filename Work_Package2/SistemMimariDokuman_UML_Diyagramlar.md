# COLLABREEN: Federe Öğrenme ile Karbon Ayak İzi Tahminleme ve Yeşil Üretim Çözümleri

## Sistem Mimari Dokümanı + UML Diyagramları

**Proje No:** 181553  
**İş Paketi:** İP2 - Gereksinim Analizi ve Mimari Tasarım  
**Hazırlayan:** Digitanol Yazılım A.Ş.  
**Akademik Danışman:** Prof. Dr. Turgay Tugay Bilgin, Bursa Teknik Üniversitesi  
**Versiyon:** 1.0  
**Tarih:** [GG.11.2025]  

---

## Doküman Revizyonu

| Versiyon | Tarih | Değişiklik Açıklaması | Hazırlayan |
|----------|-------|----------------------|------------|
| 1.0 | [Tarih] | İlk sürüm | [İsim] |

---

## İçindekiler

1. Giriş
2. Gereksinim Analizi Raporu
3. Sistem Mimarisi Tasarımı
4. Veri Toplama ve Ön İşleme Pipeline Tasarımı
5. Test Sonuçları Raporu
6. İP3'e Geçiş ve Açık Konular
7. Ekler

---

## 1. Giriş


> **[NOT:]** Bu dokümanın İP2 (Gereksinim Analizi ve Mimari Tasarım) resmi ara çıktısı olduğunu belirtin. Girdi olarak İP1 Kavramsal Tasarım Dokümanı'nı (31.10.2025) kullandığını, çıktı olarak İP3 (Federe Öğrenme Altyapısının Geliştirilmesi) için temel oluşturduğunu açıklayın.
>

---

## 2. Gereksinim Analizi Raporu

> **[NOT - Firma için:]** Bu bölüm Feyza Caniklioğlu'nun faaliyetinin çıktısıdır (Eyl-Kas 2025, %25 adam/ay). Hakem burada "sahaya inilmiş, gerçek ihtiyaçlar toplanmış" kanıtı arar. Saha fotoğrafları ve somut gereksinim listeleri eklenebilir.

### 2.1. Pilot Tesis Gereksinimleri Toplama Süreci

#### 2.1.1. HP Pelzer Pimsa Saha Ziyareti ve Görüşmeler

> **[NOT:]** Saha çalışmasının detaylarını raporlayın:
> - Görüşülen kişilerin rolleri: üretim müdürü, enerji yöneticisi, IT/OT sorumlusu, kalite müdürü
> - Saha fotoğrafları (üretim hattı, mevcut ölçüm cihazları, kontrol odası)
>



### 2.2. DigiMES Entegrasyon Gereksinimleri

> **[NOT:]** Collabreen'in DigiMES Solutions üzerine inşa edileceği kararının (İP1'de belirlenmiş) gereksinim düzeyinde somutlaştırılması:

#### 2.2.1. DigiMES Veritabanı Veri Erişim Gereksinimleri

> **[NOT:]** Pilot tesiste DigiMES kurulumunun mevcut durumunu ve Collabreen'in ihtiyaç duyduğu veri noktalarını tablo halinde gösterin:
>
> | Veri Kategorisi | DigiMES Tablosu/Alanı | Veri Tipi | Frekans | Collabreen Kullanım Amacı |
> |-----------------|----------------------|-----------|---------|--------------------------|
> | İş emri bilgileri | [tablo adı] | [tip] | [frekans] | Üretim bazlı emisyon eşleştirme |
> | Operasyon süreleri | [tablo adı] | [tip] | [frekans] | Enerji tüketim hesaplaması |
> | OEE değerleri | [tablo adı] | [tip] | [frekans] | Verimlilik-emisyon korelasyonu |
> | Duruş kodları ve süreleri | [tablo adı] | [tip] | [frekans] | Boşta enerji tüketimi analizi |
> | Bileşen tüketimleri | [tablo adı] | [tip] | [frekans] | Malzeme bazlı Scope 3 tahmini |
> | Üretim miktarları | [tablo adı] | [tip] | [frekans] | Birim başına emisyon hesaplaması |
>


#### 2.2.2. DigiCON Sensör Veri Gereksinimleri

> **[NOT:]** DigiCON üzerinden toplanması gereken ek sensör verilerini belirleyin:
> - Enerji ölçer verileri (anlık güç tüketimi, kümülatif enerji)
> - Makine proses parametreleri (sıcaklık, basınç, hız, devir vb.)
> - Doğalgaz/buhar tüketim sayaçları (varsa)
> - Mevcut DigiCON kurulumunda hangi veriler zaten toplanıyor, hangilerinin eklenmesi gerekiyor
> - Kullanılan protokoller (OPC-UA, Modbus, MQTT vb.) ve uyumluluk durumu

#### 2.2.3. RabbitMQ Altyapısı Yeterlilik Değerlendirmesi

> **[NOT:]** DigiMES'in mevcut RabbitMQ tabanlı mesajlaşma altyapısının Collabreen veri akışı için yeterliliğini değerlendirin:
> - Mevcut mesaj hacmi ve kapasite durumu
> - Collabreen'in getireceği ek yük tahmini
> - Ek stream processing katmanı (Kafka/Spark) gereksinimi değerlendirmesi
> - Karar ve gerekçesi

### 2.3. Fonksiyonel Gereksinimler

> **[NOT:]** Toplanan ihtiyaçlardan çıkarılan fonksiyonel gereksinimleri yapılandırılmış bir listede sunun. Her gereksinime benzersiz bir ID verin (ör: FR-001, FR-002...):
>
> **FR-001:** Sistem, DigiMES veritabanından iş emri bazında üretim verilerini otomatik olarak çekebilmelidir.
> **FR-002:** Sistem, her üretim tesisi için yerel model eğitimi yapabilmelidir (FL istemci).
> **FR-003:** ...
>
> Kategoriler: Veri toplama, FL eğitim, model tahmin, güvenlik/gizlilik, raporlama/dashboard, karar destek. Her gereksinim için öncelik seviyesi (Zorunlu/Önemli/İsteğe Bağlı) belirtin.

### 2.4. Fonksiyonel Olmayan Gereksinimler

> **[NOT:]** Proje önerisindeki parametrelerle uyumlu performans, güvenlik ve uyumluluk gereksinimlerini listeleyin:
>
> | ID | Kategori | Gereksinim | Hedef Değer | Kaynak |
> |----|----------|------------|-------------|--------|
> | NFR-001 | Performans | Sistem gecikme süresi | < 5 saniye | Proje önerisi |
> | NFR-002 | Performans | Veri işleme hızı | > 10.000 kayıt/dk | Proje önerisi |
> | NFR-003 | Ölçeklenebilirlik | Eş zamanlı istemci sayısı | > 5 başlangıç | Proje önerisi |
> | NFR-004 | Güvenilirlik | Sistem uptime oranı | > %80 | Proje önerisi |
> | NFR-005 | Dayanıklılık | Ağ kesintilerinde hata toleransı | [oran] | Proje önerisi |
> | NFR-006 | Güvenlik | KVKK/GDPR uyumluluğu | Tam uyum | Yasal zorunluluk |
> | NFR-007 | Veri bütünlüğü | Veri kaybı oranı | < %2 | Proje önerisi |
> | NFR-008 | Uyumluluk | ISO 14064, GHG Protocol | Tam uyum | Proje hedefi |

### 2.5. QFD (Quality Function Deployment) Matrisi

> **[NOT:]** Proje önerisinde taahhüt edilen QFD matrisini oluşturun. Kullanıcı beklentilerini (satırlar) teknik gereksinimlere (sütunlar) eşleştirin:
>
> Satırlar (Kullanıcı beklentileri): "Karbon ayak izimi görmek istiyorum", "Verilerim güvende olsun", "Gerçek zamanlı izleme", "Hangi parametre emisyonu artırıyor bilmek istiyorum", vb.
>
> Sütunlar (Teknik gereksinimler): FL altyapısı, veri pipeline, tahmin modeli, güvenlik katmanı, dashboard, karar destek.
>
> Korelasyon güçleri: Güçlü (9), Orta (3), Zayıf (1).
>
> Tam matris Ek B'de büyük boyutlu olarak verilecektir.

### 2.6. Kullanıcı Senaryoları (Use Case)

> **[NOT:]** Proje önerisinde taahhüt edilen kullanıcı senaryolarını tanımlayın. Her senaryo için: aktör, ön koşul, ana akış, alternatif akış, son koşul. Temel senaryolar:
>
> - **UC-001:** Üretim müdürü, günlük karbon ayak izi raporunu dashboard'dan görüntüler.
> - **UC-002:** Enerji yöneticisi, belirli bir üretim hattının emisyon trendini inceler.
> - **UC-003:** IT yöneticisi, yeni bir tesis (FL istemci) ekler ve sisteme dahil eder.
> - **UC-004:** Üretim planlama uzmanı, what-if analizi ile alternatif üretim parametrelerinin emisyon etkisini simüle eder.
> - **UC-005:** Sistem yöneticisi, FL eğitim döngüsünün durumunu izler.
> - **UC-006:** [Diğer senaryolar pilot tesis görüşmelerinden çıkarılacak]
>
> Bu senaryolar UML use case diyagramı olarak Bölüm 3'te görselleştirilecektir.

### 2.7. Gereksinim Doğrulama ve İzlenebilirlik

> **[NOT:]** Proje önerisindeki "gereksinim doğrulama oranı" ve "teknik gereksinim-kullanıcı gereksinimi eşleşme oranı" parametrelerini ölçün:
> - Toplam toplanan gereksinim sayısı
> - Doğrulanan gereksinim sayısı ve oranı (%)
> - Teknik-kullanıcı gereksinimi eşleşme oranı (QFD'den)
> - Kullanıcı memnuniyeti skoru (anket sonucu)
> - Gereksinim izlenebilirlik matrisi: her kullanıcı gereksiniminin hangi teknik gereksinime, hangi mimari bileşene ve hangi test senaryosuna bağlandığını gösteren izlenebilirlik tablosu

---

## 3. Sistem Mimarisi Tasarımı

> **[NOT - Firma için:]** Bu bölüm dokümanın kalbidir ve Sermet Öztoprak'ın faaliyetinin çıktısıdır (Eyl-Kas 2025, %25 adam/ay). Mimari, framework'ten bağımsız ve modüler olarak tasarlanmalıdır.

### 3.1. Mimari Tasarım İlkeleri

> **[NOT:]** Mimari kararları yönlendiren temel ilkeleri belirtin:
> - **Framework bağımsızlığı:** Mimari, FL framework seçiminden (TFF, Flower vb.) bağımsız olarak tasarlanmıştır. Modüler yapı sayesinde İP3'te yapılacak framework seçimine kolayca uyarlanabilir.
> - **DigiMES uyumluluğu:** Mimari, mevcut DigiMES Solutions altyapısı üzerine inşa edilir; mevcut bileşenlere müdahale minimize edilir.
> - **Ölçeklenebilirlik:** Pilot tesisten çoklu tesis yapısına geçişi destekleyen mimari.
> - **Güvenlik önceliği:** Gizlilik katmanlarının (DP, Secure Aggregation) sonradan eklenmesine uygun tasarım.

### 3.2. Genel Sistem Mimarisi

#### 3.2.1. DigiMES Solutions Entegrasyon Mimarisi

> **[NOT:]** Collabreen'in DigiMES Solutions ürün ailesi üzerine konumlandığını gösteren üst düzey mimari diyagramı çizin. İP1'deki kavramsal diyagramın detaylandırılmış hali:
>
> ```
> L4: SAP ERP  <--DigiDOC (IDOC)--> DigiMES Veritabanı
>                                          |
> L3: DigiMES  -------- COLLABREEN KATMANI -------
>       |               |          |              |
>       |          FL Sunucu   Tahmin Modeli   Dashboard
>       |               |
> L2: DigiCON ---- FL İstemci (Tesis 1) ---- FL İstemci (Tesis N)
>       |
> L1: PLC / Sensör / Enerji Ölçer
> ```
>
> Bu diyagramı draw.io veya Visio ile profesyonel olarak çizin. Mevcut DigiMES bileşenleri ve Collabreen'in eklediği yeni bileşenler renk ayrımı ile gösterilmelidir. Büyük boyutlu versiyonu Ek C'de.

#### 3.2.2. Sistem Bileşenleri Tanımı

> **[NOT:]** Her bileşeni detaylı olarak tanımlayın:
>
> **Mevcut Bileşenler (Değişiklik gerektirmeyen):**
> - DigiCON: Makine veri toplama konnektörü. PLC entegrasyonu (Siemens S7, Delta, Omron, Panasonic), protokol desteği (OPC-UA, OPC-DA, Modbus, MQTT, Euromap 63, Socket, TCP-UDP). Update Rate bazlı veri okuma, değer değişikliğinde tetikleme.
> - DigiMES Veritabanı: Üretim veri ambarı. İş emri, operasyon, OEE, duruş, bileşen tüketimi, kalite kayıtları.
> - DigiDOC: SAP ERP entegrasyon katmanı. IDOC tabanlı çift yönlü haberleşme.
> - RabbitMQ: Asenkron mesajlaşma altyapısı. Load-balancing, queue-enqueue, acknowledge yapıları.
>
> **Yeni Bileşenler (Collabreen kapsamında geliştirilecek):**
> - Collabreen Veri Pipeline: DigiMES veritabanından model girdilerinin çekilmesi, ön işleme, emisyon faktörü eşleştirme.
> - Collabreen FL Sunucu: Model aggregation, kümeleme yönetimi, eğitim döngüsü koordinasyonu.
> - Collabreen FL İstemci: Tesis bazında yerel model eğitimi, parametre gönderimi.
> - Collabreen Tahmin Motoru: Eğitilmiş modelle karbon emisyon tahmini.
> - Collabreen Güvenlik Modülü: DP, Secure Aggregation (İP3'te geliştirilecek, mimari yeri burada tanımlanır).
> - Collabreen Dashboard Modülü: DigiMES raporlama üzerine karbon ayak izi katmanı (İP5'te geliştirilecek, mimari yeri burada tanımlanır).

### 3.3. UML Diyagramları

> **[NOT - Firma için:]** Proje önerisinde UML diyagramları taahhüt edilmiştir. Aşağıdaki diyagramların her birini profesyonel olarak çizin. Büyük boyutlu versiyonları Ek C'de yer alacaktır.

#### 3.3.1. Bileşen Diyagramı (Component Diagram)

> **[NOT:]** Sistemin tüm bileşenlerini, aralarındaki bağımlılıkları ve arayüzleri gösteren UML bileşen diyagramı:
> - FL Sunucu bileşeni: model parametrelerinin toplanması, global modelin güncellenmesi ve istemcilere dağıtılması. 
> - FL İstemci bileşeni: tesis bazında yerel veri üzerinde model eğitimi ve güncellenmiş parametrelerin sunucuya iletilmesi.
> - Veri Pipeline bileşeni: DigiMES veritabanından veri çekme, ön işleme, emisyon faktörü eşleştirme.
> - Güvenlik bileşeni: gizlilik ve güvenlik katmanları (İP3'te detaylandırılacak, burada mimari yeri gösterilir).
> - Dashboard bileşeni: raporlama ve karar destek arayüzü (İP5'te detaylandırılacak, burada mimari yeri gösterilir).
> - DigiMES arayüzleri: veritabanı bağlantısı, RabbitMQ entegrasyonu, DigiCON veri akışı.
>
> Alt bileşen detayları (aggregation algoritması, eğitim koordinasyonu, parametre iletim mekanizması vb.) İP3'te framework seçimiyle birlikte kesinleşecektir. İP2'de bileşenler arası temel veri akışları ve bağımlılıklar gösterilmelidir.

#### 3.3.2. Dağıtım Diyagramı (Deployment Diagram)

> **[NOT:]** Bileşenlerin fiziksel altyapıya dağılımını gösteren UML dağıtım diyagramı:
> - **Merkezi Sunucu (Digitanol / Bulut):** FL Sunucu, Aggregation Engine, Global Model Repository, Dashboard Backend
> - **Pilot Tesis Sunucusu (HP Pelzer Pimsa OT Network):** FL İstemci, Local Trainer, Veri Pipeline, DigiCON servisi
> - **DigiMES Sunucusu (Pilot tesis veya bulut):** DigiMES Veritabanı, DigiDOC servisi, RabbitMQ
> - **Kullanıcı Cihazları:** Dashboard Frontend (web/mobil)
>
> Sunucular arası iletişim protokolleri (seçilen haberleşme protokolü) ve ağ topolojisi gösterilmelidir.

#### 3.3.3. Sıralama Diyagramları (Sequence Diagrams)

> **[NOT:]** En az 3 kritik akış için sıralama diyagramı çizin:
>
> **SD-001: FL Eğitim Döngüsü Akışı**
> Aktörler: FL Sunucu, FL İstemci(ler), DigiMES DB, Güvenlik Modülü
> Akış: Sunucu global model dağıtır --> İstemci lokal veriyi yükler --> İstemci lokal eğitim yapar --> İstemci parametre günceller (DP uygulanır) --> Sunucu parametreleri toplar (Secure Aggregation) --> Sunucu global modeli günceller --> Döngü tekrar
>
> **SD-002: Veri Toplama ve Ön İşleme Akışı**
> Aktörler: PLC/Sensör, DigiCON, RabbitMQ, DigiMES DB, Veri Pipeline, Feature Store
> Akış: Sensör veri üretir --> DigiCON okur --> RabbitMQ'ya gönderir --> DigiMES DB'ye yazılır --> Veri Pipeline çeker --> Ön işleme yapar --> Emisyon faktörü eşleştirir --> Feature Store'a yazar
>
> **SD-003: Karbon Tahmin İsteği Akışı**
> Aktörler: Kullanıcı (Dashboard), Tahmin Motoru, Feature Store, Global Model
> Akış: Kullanıcı tahmin ister --> Dashboard backend Feature Store'dan güncel verileri çeker --> Tahmin Motoru modeli çalıştırır --> Sonuç Dashboard'a döner --> Kullanıcıya gösterilir

#### 3.3.4. Kullanım Durumu Diyagramı (Use Case Diagram)

> **[NOT:]** Bölüm 2.6'daki kullanıcı senaryolarının UML use case diyagramı olarak görselleştirilmesi:
> - Aktörler: Üretim Müdürü, Enerji Yöneticisi, IT Yöneticisi, Üretim Planlama Uzmanı, Sistem Yöneticisi
> - Use case'ler: UC-001 ile UC-00N arası (Bölüm 2.6'dan)
> - Include/extend ilişkileri

### 3.4. Veri Akış Diyagramları (DFD)

> **[NOT:]** Proje önerisinde taahhüt edilen DFD'leri çizin:

#### 3.4.1. DFD Level 0 (Bağlam Diyagramı)

> **[NOT:]** Sistemin dış dünya ile etkileşimini gösteren en üst düzey diyagram:
> - Dış varlıklar: DigiMES/DigiCON, SAP ERP/DigiDOC, Pilot Tesis Kullanıcıları, Diğer Tesisler (FL istemcileri)
> - Tek proses: Collabreen Sistemi
> - Veri akışları: üretim verileri, sensör verileri, ERP verileri, tahmin sonuçları, raporlar, model parametreleri

#### 3.4.2. DFD Level 1 (Detay Diyagramı)

> **[NOT:]** Level 0'ın açılımı. Ana prosesler:
> - P1: Veri Toplama ve Ön İşleme
> - P2: FL Eğitim Yönetimi
> - P3: Karbon Emisyon Tahmini
> - P4: Güvenlik ve Gizlilik
> - P5: Raporlama ve Karar Destek
>
> Veri depoları: DigiMES DB, Feature Store, Model Repository, Emissions DB
>
> Her proses arası veri akışlarını oklar ile gösterin. Büyük boyutlu versiyon Ek D'de.

### 3.5. FL Sunucu-İstemci Topolojisi

> **[NOT:]** FL mimarisinin topolojik kararını ve gerekçesini detaylı açıklayın:

> **[NOT:]** İstemcilerin sisteme dahil olma/ayrılma senaryoları:
> - Yeni tesis ekleme prosedürü
> - İstemci çevrimdışı olduğunda davranış (asenkron güncelleme desteği)
> - Minimum istemci sayısı gereksinimi

### 3.6. Haberleşme Protokolü Seçimi

> **[NOT:]** Proje önerisinde gRPC, REST ve MQTT protokollerinin incelenmesi taahhüt edilmiştir. Karşılaştırma tablosu ve gerekçeli seçim yapın:

> | Kriter | gRPC | REST (HTTP/2) | MQTT |
> |--------|------|---------------|------|
> | Gecikme | Düşük (binary protobuf) | Orta (JSON) | Düşük (lightweight) |
> | Bant genişliği | Verimli (sıkıştırılmış) | Yüksek (text-based) | Çok verimli |
> | Çift yönlü iletişim | Streaming desteği | Sınırlı | Publish-subscribe |
> | Güvenlik | TLS, token-based | TLS, OAuth | TLS, username/password |
> | Endüstriyel uyumluluk | Orta | Yüksek (yaygın) | Yüksek (IoT standardı) |
> | FL model parametre transferi | Çok uygun | Uygun | Orta |
> | DigiMES uyumluluğu | [değerlendirin] | DigiMES web API mevcut | DigiCON zaten destekliyor |
>
> **Seçim:** [Seçilen protokol ve detaylı gerekçe]
>
> **Not:** Proje önerisindeki İP3'te "gRPC tabanlı düşük gecikmeli protokoller" ifadesi geçmektedir. İP2'deki bu karşılaştırma, İP3'teki geliştirmeye temel oluşturacaktır.

### 3.7. Senkron vs Asenkron FL Mimari Değerlendirmesi

> **[NOT:]** Proje önerisinde "senkron/asenkron FL algoritmaları için farklı mimari alternatifler değerlendirilecektir" taahhüdü bulunmaktadır. Bu bölümde değerlendirme yapılır, kesin seçim İP3'te deneylerle belirlenir:

> | Kriter | Senkron (FedAvg) | Asenkron (FedAsync/FedBuff) |
> |--------|-----------------|---------------------------|
> | Yakınsama garantisi | Teorik olarak güçlü | Daha zayıf, ama pratik çözümler var |
> | Straggler problemi | Var (en yavaş istemci tıkar) | Çözülmüş |
> | Endüstriyel ortam uyumu | Zayıf (ağ kesintileri) | Güçlü (kesintilere dayanıklı) |
> | Uygulama karmaşıklığı | Düşük | Orta-Yüksek |
> | Referans çalışma uyumu | Cui et al. (FedAvg), Yi et al. (Fed-I-AMP) | Proje önerisindeki İP3 taahhüdü |
>
> **Mimari karar:** Mimari, hem senkron hem asenkron çalışmayı destekleyecek şekilde tasarlanmıştır. Aggregation katmanı soyutlanarak (abstraction layer), İP3'te yapılacak karşılaştırmalı denemelere kolayca uyarlanabilir bir yapı öngörülmüştür.
>
> **Aggregation Abstraction Layer tasarımı:** Sunucu tarafında FedAvg, FedAsync, Fed-I-AMP ve potansiyel diğer algoritmaları çalıştırabilecek bir arayüz (interface) tasarımı. Bu katman sayesinde İP3'teki deneysel karşılaştırma sürecinde algoritmalar arası geçiş kolay olacaktır.

### 3.8. DigiMES Entegrasyon Mimari Kararları

#### 3.8.1. Mevcut RabbitMQ vs Ek Kafka Katmanı Kararı

> **[NOT:]** Bölüm 2.2.3'teki gereksinim değerlendirmesi sonucuna dayanarak mimari kararı raporlayın:
> - Değerlendirilen alternatifler: (a) mevcut RabbitMQ üzerinden Collabreen veri akışı, (b) RabbitMQ + Kafka köprüsü, (c) Kafka ile paralel pipeline
> - Seçim ve gerekçesi
> - Mimari diyagramdaki konumu

#### 3.8.2. DigiMES Veritabanı Erişim Stratejisi

> **[NOT:]** Collabreen'in DigiMES veritabanına nasıl erişeceğini belirleyin:
> - Doğrudan DB erişimi mi yoksa DigiMES API üzerinden mi
> - Veri çekme frekansı ve yöntemi (polling, event-driven, batch)
> - Veri izolasyonu: Collabreen'in DigiMES'in performansını etkilememesi için alınan önlemler

---

## 4. Veri Toplama ve Ön İşleme Pipeline Tasarımı

> **[NOT - Firma için:]** Bu bölüm Şevval Bengül'ün faaliyetinin çıktısıdır (Eki-Kas 2025, %22 adam/ay). Proje önerisinde ETL yaklaşımı, normalizasyon, outlier detection, missing data imputation, PCA ve emisyon faktörü eşleştirmeleri taahhüt edilmiştir.

### 4.1. Veri Kaynakları Envanteri

> **[NOT:]** Bölüm 2.2'deki gereksinim analizi sonuçlarına dayanarak tüm veri kaynaklarının detaylı envanterini oluşturun:
>
> | Kaynak | Veri Türü | Format | Frekans | Hacim Tahmini | Protokol | Durum |
> |--------|-----------|--------|---------|---------------|----------|-------|
> | DigiMES DB - İş Emri | Yapılandırılmış | SQL | Olaya bağlı | [adet/gün] | DB Connector | Mevcut |
> | DigiMES DB - OEE | Yapılandırılmış | SQL | Vardiya bazlı | [kayıt/gün] | DB Connector | Mevcut |
> | DigiMES DB - Duruş | Yapılandırılmış | SQL | Olaya bağlı | [kayıt/gün] | DB Connector | Mevcut |
> | DigiCON - Enerji Ölçer | Sayısal (kWh) | JSON/PLC | [x] saniye | [kayıt/gün] | OPC-UA/Modbus | [Mevcut/Eklenmeli] |
> | DigiCON - Proses Param. | Sayısal | JSON/PLC | [x] saniye | [kayıt/gün] | OPC-UA/Modbus | [Mevcut/Eklenmeli] |
> | SAP ERP (DigiDOC) | Yapılandırılmış | XML (IDOC) | Olaya bağlı | [mesaj/gün] | IDOC | Mevcut |
> | Emisyon Faktörleri | Referans | CSV/DB | Statik | Sabit tablo | Manuel/API | Oluşturulacak |
>
> Pilot tesis ziyareti sonrasında gerçek değerlerle güncellenecektir.

### 4.2. ETL Pipeline Mimari Tasarımı

> **[NOT:]** Extract-Transform-Load sürecinin detaylı tasarımını yapın:

#### 4.2.1. Extract (Veri Çekme) Katmanı

> **[NOT:]** Her veri kaynağından veri çekme yöntemini tanımlayın:
> - DigiMES veritabanından: hangi yöntemle (SQL sorgu, API, change data capture), hangi frekansla
> - DigiCON'dan: mevcut akış mı kullanılacak yoksa ayrı bir kanal mı
> - SAP ERP'den: DigiDOC üzerinden hangi IDOC mesajları ilgili
> - Veri çekme zamanlaması: batch (periyodik) mi, event-driven mı, hibrit mi

#### 4.2.2. Transform (Dönüştürme) Katmanı

> **[NOT:]** Veri temizleme ve dönüştürme adımlarını sırasıyla tanımlayın:
>
> **a) Veri Temizleme:**
> - **Outlier detection:** Seçilen yöntem ve gerekçesi (Z-score, IQR, Isolation Forest vb.). DigiMES üretim verisinin karakteristiğine uygunluk değerlendirmesi.
> - **Missing data imputation:** Seçilen yöntem ve gerekçesi (ortalama/medyan, KNN imputation, zaman serisi interpolasyon vb.). Eksik verinin kaynağı analizi (sensör arızası, iletişim kesintisi, planlı duruş).
> - **Noise filtering:** Sensör gürültüsü temizleme stratejisi.
> - **Duplikasyon kontrolü:** RabbitMQ'nun acknowledge yapısı sayesinde azaltılan ama kontrol edilmesi gereken tekrarlı kayıtlar.
>
> **b) Normalizasyon:**
> - Seçilen yöntem ve gerekçesi (Min-Max, Z-score, Robust Scaler vb.)
> - Farklı DigiMES kurulumlarından (farklı tesisler) gelen verilerin ölçek farklılıklarının yönetimi
>
> **c) Özellik Mühendisliği:**
> - Zaman tabanlı özellikler: kayan pencere ortalamaları, trend, mevsimsellik
> - İstatistiksel özellikler: standart sapma, minimum, maksimum, çeyreklikler
> - Domain-spesifik özellikler: birim başına enerji tüketimi, OEE-emisyon oranı, duruş süresi-enerji kaybı
> - PCA ile boyut indirgeme değerlendirmesi (proje önerisinde taahhüt edilmiş)
>
> **d) Karşılaştırmalı Deneme İçin Veri Hazırlama:**
> - **Yaklaşım A (Cui et al.) için:** Zaman serisi formatında veri seti hazırlama. Enerji tüketim ve emisyon zaman serileri.
> - **Yaklaşım B (Yi et al.) için:** Proses parametre bazlı veri seti hazırlama. İş emri bazında proses parametreleri (sıcaklık, basınç, hız, süre), makine performans metrikleri, enerji tüketimi.
> - Her iki veri seti arasındaki eşleştirme ve tutarlılık kontrolü.

#### 4.2.3. Load (Yükleme) Katmanı

> **[NOT:]** İşlenmiş verilerin nereye ve nasıl yazılacağını tanımlayın:
> - Feature Store yapısı (tablo/schema tasarımı)
> - Versiyon yönetimi (eğitim verisi sürümlendirme)
> - Arşivleme ve saklama politikası

### 4.3. ISO 14064 / GHG Protocol Emisyon Faktörü Entegrasyon Tasarımı

> **[NOT:]** Proje önerisinde "emisyon faktörlerinin doğrudan veri pipeline'a gömülmesi" taahhüt edilmiştir:
> - Emisyon faktörü veritabanı tasarımı: Scope 1 (doğrudan), Scope 2 (enerji kaynaklı), Scope 3 (dolaylı) sınıflandırması
> - Türkiye'ye özgü emisyon katsayıları kaynağı (EPDK, TEİAŞ, uluslararası veritabanları)
> - Enerji tüketim verisi (kWh, m³ doğalgaz, lt mazot) ile emisyon katsayısı çarpım mantığı
> - Emisyon faktörü güncelleme mekanizması (yıllık/periyodik)
> - Doğrulama: hesaplanan emisyon değerlerinin referans değerlerle karşılaştırılması

### 4.4. Veri Kalitesi İzleme Tasarımı

> **[NOT:]** Pipeline'ın çalışma sırasında veri kalitesini izleme mekanizması:
> - Veri bütünlüğü kontrolü: beklenen kayıt sayısı vs gelen kayıt sayısı
> - Veri kaybı oranı izleme (hedef: <%2)
> - Anomali tespiti: normal dışı veri akışı uyarıları
> - Dashboard üzerinden veri kalitesi metrikleri

---

## 5. Test Sonuçları Raporu

> **[NOT - Firma için:]** Bu bölüm İdris Can Ellik'in faaliyetinin çıktısıdır (Kas 2025, %30 adam/ay). Proje önerisinde 4 test grubu taahhüt edilmiştir. Her test için yapılandırılmış bir rapor formatı kullanın.

### 5.1. Test Yaklaşımı ve Ortamı

> **[NOT:]** Genel test stratejisini açıklayın:
> - Test ortamı tanımı: donanım, yazılım, ağ konfigürasyonu
> - Test verisi stratejisi: gerçek pilot veri mi, sentetik veri mi, karma mı
> - Test araçları ve yöntemleri
> - Başarı/başarısızlık kriterleri

### 5.2. Test 1: Gereksinim Doğrulama Analizi

> **[NOT:]** Proje önerisinden: "Anket, görüşme çıktılarının teknik gereksinimlerle eşleştirilmesi. Sanayi pilot tesislerinin beklentilerinin FL altyapısına doğru yansıtılması." Yapılacağı yer: Yurtiçi kuruluşlarda.
>
> | Alan | Detay |
> |------|-------|
> | Test Amacı | Pilot tesis beklentilerinin teknik gereksinimlere doğru yansıtıldığını doğrulamak |
> | Test Yöntemi | QFD matrisinin pilot tesis yetkilileriyle birlikte gözden geçirilmesi, gereksinim izlenebilirlik analizi |
> | Hedef Parametreler | Gereksinim doğrulama oranı (%), teknik-kullanıcı eşleşme oranı |
> | Test Ortamı | [Belirtiniz] |
> | Sonuçlar | [Test sonrasında doldurulacak] |
> | Hedefle Karşılaştırma | [Değerlendirme] |
> | İP3'e Aktarılan Bulgular | [Varsa] |

### 5.3. Test 2: Yük Testi, Gecikme Ölçümleri ve Hata Tolerans Testi

> **[NOT:]** Proje önerisinden: "FL mimarisinin ölçeklenebilirlik, güvenilirlik ve düşük gecikme performansını doğrulamak." Yapılacağı yer: Firmada.
>
> | Alan | Detay |
> |------|-------|
> | Test Amacı | FL mimarisinin ölçeklenebilirlik, güvenilirlik ve düşük gecikme performansını doğrulamak |
> | Test Senaryoları | (a) Çoklu istemci senaryoları: 5, 10, 20 istemci simülasyonu. (b) Gecikme ölçümü: parametre gönderim süresi. (c) Ağ kesintisi simülasyonu: istemci bağlantı kaybı ve yeniden bağlanma |
> | Hedef Parametreler | Sistem gecikme süresi < 5 sn, istemci sayısı desteği > 5, uptime > %80, hata toleransı oranı |
> | Test Ortamı | Firma içi sunucularda simülasyon ortamı |
> | Sonuçlar | [Test sonrasında doldurulacak] |
> | Hedefle Karşılaştırma | [Parametre bazında tablo] |
> | İP3'e Aktarılan Bulgular | [Darboğazlar, optimizasyon önerileri] |

### 5.4. Test 3: Veri Bütünlüğü, Veri Kaybı/Eksiklik ve Outlier Testleri

> **[NOT:]** Proje önerisinden: "Farklı kaynaklardan gelen heterojen verilerin güvenilir şekilde normalize edilmesi ve model doğruluğunun sağlanması." Yapılacağı yer: Yurtiçi kuruluşlarda.
>
> | Alan | Detay |
> |------|-------|
> | Test Amacı | Pipeline'ın veri bütünlüğünü koruduğunu, kayıp/eksik veriyi doğru yönettiğini, outlier'ları tespit ettiğini doğrulamak |
> | Test Senaryoları | (a) Veri bütünlüğü: kaynak-hedef kayıt sayısı karşılaştırması. (b) Eksik veri: yapay eksiklik oluşturma ve imputation doğruluğu. (c) Outlier: bilinen anomalilerin tespit oranı. (d) Noise: sensör gürültüsü simülasyonu |
> | Hedef Parametreler | Veri bütünlüğü > %80, veri kaybı < %2, emisyon faktörü eşleştirme doğruluk oranı |
> | Test Ortamı | [Belirtiniz] |
> | Sonuçlar | [Test sonrasında doldurulacak] |
> | Hedefle Karşılaştırma | [Parametre bazında tablo] |
> | İP3'e Aktarılan Bulgular | [Veri kalitesi bulguları, pipeline iyileştirme önerileri] |

### 5.5. Test 4: End-to-End Doğrulama ve Performans Testi

> **[NOT:]** Proje önerisinden: "Geliştirilen mimarinin gerçek sanayi verisi üzerinde bütünleşik çalışabilirliğini kanıtlamak." Yapılacağı yer: Yurtiçi kuruluşlarda.
>
> | Alan | Detay |
> |------|-------|
> | Test Amacı | Tüm bileşenlerin entegre çalışabilirliğini, veri akışının uçtan uca sağlıklı olduğunu kanıtlamak |
> | Test Senaryoları | (a) DigiMES DB'den veri çekme --> pipeline --> FL istemci'ye teslim döngüsü. (b) Throughput ölçümü: >10.000 kayıt/dk hedefi. (c) Gerçek pilot veri (veya gerçeğe yakın sentetik veri) ile tam döngü testi |
> | Hedef Parametreler | Throughput > 10.000 kayıt/dk, gecikme < 5 sn, uçtan uca veri bütünlüğü |
> | Test Ortamı | [Belirtiniz - mümkünse pilot tesis verisine yakın] |
> | Sonuçlar | [Test sonrasında doldurulacak] |
> | Hedefle Karşılaştırma | [Parametre bazında tablo] |
> | İP3'e Aktarılan Bulgular | [Entegrasyon bulguları, performans darboğazları, mimari iyileştirme önerileri] |

### 5.6. Test Sonuçları Özet Tablosu

> **[NOT:]** Tüm testlerin sonuçlarını tek bir özet tabloda gösterin:
>
> | Test | Hedef Parametre | Hedef Değer | Gerçekleşen | Durum (Başarılı/Kısmi/Başarısız) |
> |------|-----------------|-------------|-------------|--------------------------------|
> | Gereksinim doğrulama | Doğrulama oranı | [%] | [%] | [Durum] |
> | Yük testi | İstemci sayısı desteği | > 5 | [Sonuç] | [Durum] |
> | Yük testi | Gecikme | < 5 sn | [Sonuç] | [Durum] |
> | Yük testi | Uptime | > %80 | [Sonuç] | [Durum] |
> | Veri bütünlüğü | Bütünlük oranı | > %80 | [Sonuç] | [Durum] |
> | Veri bütünlüğü | Veri kaybı | < %2 | [Sonuç] | [Durum] |
> | E2E doğrulama | Throughput | > 10.000 kayıt/dk | [Sonuç] | [Durum] |

---

