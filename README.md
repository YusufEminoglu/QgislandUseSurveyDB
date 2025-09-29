# QgislandUseSurveyDB

Proje, coğrafi bilgi sistemleri (CBS) ortamında arazi kullanım ve örtüsü tiplerinin hızlı ve tutarlı bir şekilde envanterinin çıkarılması için gerekli olan tablo şemalarını, ilişki matrislerini ve veri sözlüğünü sunar. QGIS masaüstü ve QField/Input gibi mobil uygulamalarla saha verisi toplama sürecini optimize etmeyi amaçlar.

## Depo Hakkında

Bu depo Bergama ilçesine ait arazi kullanım verilerinin yönetimi ve saha anketi süreçlerinin dijitalleştirilmesi için hazırlanmıştır. Temel veri kaynağı **AraziKullanimDB.gpkg** adlı GeoPackage dosyasıdır. GeoPackage; binalar, yollar, çevre düzeni planı, alan grupları, saha çalışma sınırı ve çalışma ölçeği 1/5000 olan tematik katmanları içerir. Ada poligonlarının çizimi 1/1000 detayında yapılmış, ancak analiz ve plan kararları 1/5000 ölçeği referans alacak şekilde yapılandırılmıştır. Öğrencilerin sahada topladığı güncel ada bilgileri hem çizgisel hem de alansal detaylarıyla kayıt altına alınabilir. Mobil form kurulumları ve QField/Input uyarlamaları yapılmıştır; dosya yerel ortama taşındığında saha veya ofis düzenlemeleri için hazırdır.

## Neden Bu Depo?

* **QGIS uyumu:** GeoPackage dosyası doğrudan QGIS'e eklenerek katmanlara erişim sağlanır. Tüm katmanlar EPSG:5253 koordinat referans sistemini kullanır.
* **QField / Input entegrasyonu:** Arazi anket formları ve alan doğrulama süreçleri için mobil cihazlarda kullanılabilecek optimize edilmiş bir şema sunar.
* **Çok katmanlı veri yapısı:** Bina, yol, çevre düzeni ve ada sınırı gibi katmanlar tek dosyada yönetilir.
* **Standartlaştırılmış öznitelik şeması:** Arazi kullanım tipleri, plan kararları ve saha notları için ortak alan adları kullanılır.
* **Ölçek uyumu:** Planlama ve analizler 1/5000 ölçeğine göre organize edilir; ada çizimleri sahada 1/1000 detayında gözden geçirilir.

## Katman Odakları

* **CIZGI (Çizgi Fonksiyonları):** Yol fonksiyonları, genişlik değerleri ve kaplama türleri için değer haritaları içerir. Form görünümünde `is_valid($geometry)` ifadesi ile geçersiz geometriler kırmızı vurgulanır.
* **ALAN (Arazi Kullanımı):** Alternatif fonksiyon kod listeleri, hâkim yapı türü seçenekleri ve yapı yılı, kat sayısı gibi plan notları sahada girilir. Bu katmanda da `is_valid($geometry)` kontrolü form koşullu biçimlendirmesine bağlıdır.

## Kurulum ve Hazırlık Adımları

Aşağıdaki sıralama, GeoPackage'ı indirip QGIS projesini açtıktan sonra veri üretimine vakit kaybetmeden başlayabilmeniz için tasarlandı:

1. **Dosyayı çöz ve arşivle:** Depoda sağlanan `AraziKullanimDB.zip` (veya kurumunuzdan aldığınız arşiv) dosyasını indirip açın. Ortaya çıkan `AraziKullanimDB.gpkg` dosyasını güvenilir ve kısa yol içermeyen bir klasöre taşıyın; önerilen dizin yapısı `D:\PLN3113\Veri` veya `C:\PLN3113\Veri` şeklindedir.
2. **QGIS'te kaynağı bağla:** QGIS 3.22+ sürümünü açın. **Tarayıcı Paneli**'nden **GeoPackage** başlığına sağ tıklayıp **Bağlantı Ekle** seçeneğini kullanarak `AraziKullanimDB.gpkg` dosyasını gösterin. Bağlantı eklendikten sonra GeoPackage altındaki yeşil simgeli proje dosyasını (QGZ) çift tıklayın; böylece çizgi ve alan formları hazır şekilde QGIS ana görünümünde açılır.
3. **Çalışma adalarını içe aktar:** Bergama'daki çalışma adalarınızı mevcut projeye referans olarak ekleyin. Geçici katman (ör. "Scratch Layer" veya harici shapefile) olarak eklediğiniz ada geometrilerini seçip **ALAN** katmanına kopyala-yapıştır yapın. Bu işlemden sonra ALAN katmanında yeni kayıtlar oluşturulur.
4. **Anket verilerini doldur:** ALAN katmanındaki kayıtlar için yapı yılı, hâkim yapı türü, alternatif fonksiyon gibi form alanlarını doldurun. Tüm değer listeleri QField formu ile eşlenmiştir; veri sözlüğündeki kodlara sadık kalın.
5. **Çizgisel veriyi tamamla:** Aynı süreçle `CIZGI` katmanına geçiş yapın. Yol fonksiyonu, genişlik değerleri, kaldırım/refüj ölçüleri, yol durumu ve yol malzemesi alanlarını formdaki değer haritalarını kullanarak doldurun.
6. **Geometri geçerliliğini doğrula:** Hem ALAN hem CIZGI katmanında form görünümünde `fid` alanı koşullu biçimlendirme ile renklendirilir: yeşil = geçerli, kırmızı = geçersiz. Veri girişini sonlandırmadan önce `Vector > Geometry Tools > Check Validity` aracını çalıştırarak olası hataları giderin.

> **İpucu:** Proje dosyasını açtığınızda katman grupları, etiket ayarları ve form düzenleri otomatik olarak yüklenir. Böylece QGIS arayüzünde ek bir kurulum gerektirmez.

## Tavsiye Edilen Proje Yapısı

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
6. Saha dönüşünde, yukarıdaki geometri ve form kontrollerini tekrar çalıştırarak veri bütünlüğünü doğrulayın.

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
