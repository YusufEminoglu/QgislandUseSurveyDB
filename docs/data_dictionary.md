# Arazi Kullanım GeoPackage Veri Sözlüğü

Bu doküman, `AraziKullanimDB.gpkg` GeoPackage dosyasında bulunan ana katmanları, öznitelik alanlarını ve alanların amaçlanan kullanımını özetler. Dosya, Bergama çalışma alanında yapılan arazi kullanım envanterinin dijital kaydıdır ve hem ofis hem de saha ekiplerinin ortak bir veri modeli üzerinden çalışmasını sağlar.

## GeoPackage Dosyası

| Özellik | Değer |
| --- | --- |
| Dosya adı | `AraziKullanimDB.gpkg` |
| Koordinat Referans Sistemi | EPSG:5253 (ITRF96 / TM30 3 Derece) |
| Varsayılan Ölçek | 1/1000 |
| Temel Kullanım | QGIS masaüstü ve QField/Input saha uygulamaları |

## Katmanlar

### 1. `CIZGI`

`CIZGI` katmanı, Bergama çalışmasında toplanan tüm çizgisel altyapı ve ulaşım unsurlarını içerir. Katman QField formu ile tam uyumlu olup aşağıdaki öznitelik alanlarını barındırır:

| Alan | Tip | Uzunluk / Hassasiyet | Açıklama |
| --- | --- | --- | --- |
| `fid` | Integer64 | — | Otomatik oluşturulan benzersiz kimlik. Form görünümünde `is_valid($geometry)` ifadesi ile renklendirilir; geçerli geometriler yeşil, hatalı geometriler kırmızı görünür. |
| `nipfonksiyon` | String | 100 | Çizgi fonksiyonu. Değerler ["Çizgi Fonksiyonu" kod listesi](#cizgi-nipfonksiyon-degerleri) üzerinden seçilir. |
| `yolTipi` | String | 50 | Kurumsal yol sınıfı veya taşıt yolu türü. |
| `yolGenislik` | Real | p=10, s=2 | Alan ölçümüne göre girilen birincil yol genişliği (m). |
| `solKaldirimGenislik` | Real | p=10, s=2 | Sol kaldırım genişliği (m). |
| `refujGenislik` | Real | p=10, s=2 | Orta refüj genişliği (m). |
| `SolSeritSayisi` | Integer64 | — | Sol şerit adedi. |
| `SagSeritSayisi` | Integer64 | — | Sağ şerit adedi. |
| `yolGenislik2` | Real | p=10, s=2 | Yol kesitine göre hesaplanan ikinci genişlik (m). |
| `Kullanıcı` | String | 50 | Kaydı oluşturan kullanıcı adı. |
| `TarihIlk` | DateTime | — | Oluşturma tarihi (QField tarih-saat bileşeni). |
| `TarihSon` | String | 50 | Güncelleme tarihini tutan metin alanı (örn. QField otomasyon ifadeleriyle güncellenir). |
| `Uzunluk(m)` | Real | p=10, s=2 | Geometrinin metrik uzunluğu. |
| `sagKaldirimGenislik` | Real | p=10, s=2 | Sağ kaldırım genişliği (m). |
| `YolDurum` | String | 20 | Yolun mevcut fiziksel durumu. [Değer haritası](#cizgi-yoldurum-degerleri) kullanılarak seçilir. |
| `YolMalzeme` | String | 20 | Kaplama malzemesi türü. [Değer haritası](#cizgi-yolmalzeme-degerleri) kullanılarak seçilir. |

#### `CIZGI.nipfonksiyon` değerleri

| Kod | Etiket |
| --- | --- |
| `DEMİRYOLU` | Demiryolu |
| `BORU HATTI` | Boru Hattı |
| `ENERJİ NAKİL HATTI` | Enerji Nakil Hattı |
| `İÇME SUYU ANA HATTI` | İçme Suyu Ana Hattı |
| `SOĞUTMA SUYU ALMA HATTI` | Soğutma Suyu Alma Hattı |
| `MANİA PLANI` | Mania Planı |
| `ERİŞME KONTROLLÜ KARAYOLU (OTOYOL)` | Erişme Kontrollü Karayolu (Otoyol) |
| `BİRİNCİ DERECE YOL` | Birinci Derece Yol |
| `İKİNCİ DERECE YOL` | İkinci Derece Yol |

#### `CIZGI.YolDurum` değerleri

| Kod | Etiket |
| --- | --- |
| `IYI` | İyi |
| `ORTA` | Orta |
| `KOTU` | Kötü |
| *(boş)* | Belirtilmemiş |

#### `CIZGI.YolMalzeme` değerleri

| Kod | Etiket |
| --- | --- |
| `BITUMLU_KARISIMLAR` | Asfalt |
| `BETONARME_KAPLAMA` | Beton |
| `DOGAL_TAS` | Arnavut |
| `KILITLI_PARKE` | Parke Taşı |
| `GRANULER` | Stabilize Toprak |

### 2. Diğer katmanlar

GeoPackage içerisinde çizgisel katmana ek olarak binalar, alan grupları, çalışma sınırı ve 1/1000 ölçekli ada poligonları gibi tamamlayıcı katmanlar bulunur. Bu katmanların öznitelik şemaları saha çalışması sırasında toplanan verilere göre yapılandırılmıştır ve yeni sürümlerde ayrıntılı tablolar eklenecektir.

## Kod Listeleri

Katmanlardaki tüm kod listeleri QField form yapılandırması ile birlikte gelir; değer haritaları QGIS'te form görüntüsünde otomatik sunulur.

## Veri Yönetimi İpuçları

* `fid` alanı otomatik artan kimlik alanıdır; değerini manuel değiştirmeyin.
* Saha sırasında QField otomasyonları `TarihIlk` ve `TarihSon` alanlarını güncel tutar. Gerektiğinde QGIS ifade editöründe `now()` veya `format_date()` tabanlı ifadeleri kullanabilirsiniz.
* Genişlik ve şerit sayısı alanları için metre cinsinden değer girildiğinden emin olun; QGIS'te birim doğrulaması için alan kısıtları eklenebilir.

## Sürüm Kontrolü Tavsiyeleri

* GeoPackage dosyasını Git LFS veya harici bir bulut depolama alanında saklayın.
* `data/` klasöründe `.gitignore` dosyası oluşturarak büyük boyutlu verilerin yanlışlıkla sürüm kontrolüne eklenmesini engelleyin.
* Form yapılandırmalarında yapılan değişiklikleri belgelemeniz için bu doküman üzerinde ilgili bölümü güncelleyin.

