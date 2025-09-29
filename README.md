 # QgislandUseSurveyDB

Proje, coğrafi bilgi sistemleri (CBS) ortamında arazi kullanım ve örtüsü tiplerinin hızlı ve tutarlı bir şekilde envanterinin çıkarılması için gerekli olan tablo şemalarını, ilişki matrislerini ve veri sözlüğünü sunar. QGIS masaüstü ve QField/Input gibi mobil uygulamalarla saha verisi toplama sürecini optimize etmeyi amaçlar.

## Depo Hakkında

Bu depo Bergama ilçesine ait arazi kullanım verilerinin yönetimi ve saha anketi süreçlerinin dijitalleştirilmesi için hazırlanmıştır. Temel veri kaynağı **AraziKullanimDB.gpkg** adlı GeoPackage dosyasıdır. GeoPackage; binalar, yollar, çevre düzeni planı, alan grupları, saha çalışma sınırı ve çalışma ölçeği 1/5000 olan tematik katmanları içerir. Ada poligonlarının çizimi 1/1000 detayında yapılmış, ancak analiz ve plan kararları 1/5000 ölçeği referans alacak şekilde yapılandırılmıştır. Öğrencilerin sahada topladığı güncel ada bilgileri hem çizgisel hem de alansal detaylarıyla kayıt altına alınabilir. Mobil form kurulumları tamamlanmıştır; proje dosyası indirildiğinde QField/Input üzerinde saha gezisi sonrası güncelleme yapılmaya hazırdır.

## Özellikler

* **QGIS uyumu:** GeoPackage dosyası doğrudan QGIS'e eklenerek katmanlara erişim sağlanır. Tüm katmanlar EPSG:5253 koordinat referans sistemini kullanır.
* **QField / Input entegrasyonu:** Arazi anket formları ve alan doğrulama süreçleri için mobil cihazlarda kullanılabilecek optimize edilmiş bir şema sunar.
* **Çok katmanlı veri yapısı:** Bina, yol, çevre düzeni ve ada sınırı gibi katmanlar tek dosyada yönetilir.
* **Standartlaştırılmış öznitelik şeması:** Arazi kullanım tipleri, plan kararları ve saha notları için ortak alan adları kullanılır.
* **Ölçek uyumu:** Planlama ve analizler 1/5000 ölçeğine göre organize edilir; ada çizimleri sahada 1/1000 detayında gözden geçirilir.

## Katman Odakları

* **CIZGI (Çizgi Fonksiyonları):** Yol fonksiyonları, genişlik değerleri ve kaplama türleri için değer haritaları içerir. Form görünümünde `is_valid($geometry)` ifadesi ile geçersiz geometriler kırmızı vurgulanır.
* **ALAN (Arazi Kullanımı):** Alternatif fonksiyon kod listeleri, hakim yapı türü seçenekleri ve yapı yılı, kat sayısı gibi plan notları sahada girilir. Bu katmanda da `is_valid($geometry)` kontrolü form koşullu biçimlendirmesine bağlıdır.

## Başlangıç

1. `AraziKullanimDB.gpkg` dosyasını depodaki `data/` klasörüne kopyalayın veya [sistem tarafından sağlanan konuma](docs/data_dictionary.md#geopackage-dosyasi) yerleştirin.
2. QGIS (3.22 veya üzeri) uygulamasını açın ve **Veri Kaynağı Yöneticisi** üzerinden GeoPackage dosyasını projeye ekleyin.
3. Katmanları inceleyin ve `docs/data_dictionary.md` dosyasındaki tabloyu referans alarak öznitelik alanlarını gözden geçirin. Örneğin `CIZGI` katmanı yol ve hat fonksiyonlarını, genişliklerini ve malzeme durumunu barındırır.
4. QField Sync eklentisi ile sahaya çıkmadan önce projeyi mobil cihazlara aktarın.

## Tavsiye Edilen Proje Yapısım

```
QgislandUseSurveyDB/
├── data/
│   └── AraziKullanimDB.gpkg
├── docs/
│   └── data_dictionary.md
├── qgis/
│   └── Bergama_Arastirma.qgz (opsiyonel proje dosyası)
└── README.md
```

> **Not:** GeoPackage dosyası büyük boyutlu olduğundan sürüm kontrolüne dahil edilmez. Dosyayı depoya eklemek yerine `data/` klasöründe yerel olarak tutmanız önerilir.

## QField / Input ile Saha Çalışması

1. QGIS projesini hazırladıktan sonra **QFieldSync** eklentisini yükleyin.
2. Projeyi hedef klasöre senkronize edin ve `Input`/`QField` uygulamasında açın.
3. `AlanCalismaSiniri` katmanını "yalnızca oku" moduna alın, diğer katmanlara düzenleme yetkisi verin.
4. Saha sırasında anket formunu doldurmak için **AlanGruplari** katmanındaki form bileşenlerini kullanın.
5. Çalışma tamamlandıktan sonra cihazı QGIS'e bağlayın ve değişiklikleri birleştirin.

## Veri Kalite Kontrolleri

* **Geometri doğrulama:** QGIS'te `Vector > Geometry Tools > Check Validity` aracıyla geçersiz geometrileri kontrol edin. `CIZGI` katmanında `fid` alanı form görünümünde `is_valid($geometry)` ifadesiyle renklendirilir; geçersiz geometriler kırmızı, geçerli geometriler yeşil olarak vurgulanır.
* **Alan değerleri:** `AraziTuru`, `PlanKarari` gibi alanlar için kod listelerini kullanın. Kod listeleri `docs/data_dictionary.md` dosyasında yer alır.
* **Alan çatışmaları:** Ada poligonları ile bina sınırlarının çakışmasını kontrol etmek için `Select by Location` fonksiyonundan yararlanın.

## Katkıda Bulunma

1. Bu depoyu forklayın ve yerel ortamınıza klonlayın.
2. Yeni bir dal oluşturun: `git checkout -b ozellik/ogrenci-formu`.
3. Yaptığınız değişiklikleri test edin ve `docs/data_dictionary.md` dosyasındaki yönergeleri güncel tutun.
4. Pull request açarak değişikliklerin gözden geçirilmesini talep edin.

## Lisans

Bu proje [MIT Lisansı](LICENSE) kapsamında lisanslanmıştır.
=======
 