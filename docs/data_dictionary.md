# Arazi Kullanım GeoPackage Veri Sözlüğü

Bu doküman, `AraziKullanimDB.gpkg` GeoPackage dosyasında bulunan ana katmanları, öznitelik alanlarını ve alanların amaçlanan kullanımını özetler. Dosya, Bergama çalışma alanında yapılan arazi kullanım envanterinin dijital kaydıdır ve hem ofis hem de saha ekiplerinin ortak bir veri modeli üzerinden çalışmasını sağlar.

## GeoPackage Dosyası

| Özellik | Değer |
| --- | --- |
| Dosya adı | `AraziKullanimDB.gpkg` |
| Koordinat Referans Sistemi | EPSG:5253 (ITRF96 / TM30 3 Derece) |
| Çalışma Ölçeği | 1/5000 (ada çizimleri 1/1000 detayında yapılmıştır) |
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

### 2. `ALAN`

`ALAN` katmanı; fonksiyon, hakim yapı özellikleri ve hizmet alanı stokunu gösteren poligonları içerir. Çalışma ölçeği 1/5000 olup ada sınırları 1/1000 detayında çizilmiştir. Form görünümünde `is_valid($geometry)` koşullu biçimlendirmesi ile geçersiz geometriler kırmızı vurgulanır.

| Alan | Tip | Uzunluk / Hassasiyet | Açıklama |
| --- | --- | --- | --- |
| `fid` | Integer64 | — | Otomatik kimlik; geçersiz geometrileri ayırt etmek için `is_valid($geometry)` ifadesiyle renklenir. |
| `nipfonksiyon` | String | 100 | 1/5000 ölçeğindeki NİP fonksiyon kodu. Değerler ilişkili referans tablosundan seçilir. |
| `TarihIlk` | DateTime | — | Poligonun oluşturulduğu tarih-saat. |
| `TarihSon` | DateTime | — | Son güncelleme tarih-saat bilgisi. |
| `Kullanici` | String | 50 | Kaydı düzenleyen kullanıcı. |
| `AlanHa` | Real | p=10, s=4 | Nesnenin hektar cinsinden alanı. |
| `HakimKatSayisi` | Integer64 | — | Ada bazlı ortalama kat sayısı (üst sınır 15). |
| `HakimYapiTuru` | String | 50 | Hakim yapı türü (aşağıdaki değer listesine bakın). |
| `YapiYili` | String | 50 | Hakim yapıların yapım dönemi (aşağıdaki değer listesine bakın). |
| `AnaKonutVarMi?` | String | 50 | Alanda ana konut fonksiyonunun bulunup bulunmadığı (Evet/Hayır). |
| `OtoparkVarMi?` | String | 50 | Kapalı/açık otopark varlığı (Evet/Hayır). |
| `OtoparkKamuMu?` | String | 50 | Otopark kamuya mı ait bilgisini tutar. |
| `OtoparkKapasite?` | String | 50 | Otopark kapasitesi veya açıklaması. |
| `YatiliLiseVarMi?` | String | 50 | Yatılı lise varlığı için Evet/Hayır alanı. |
| `EndMeslekLiseVarMi?` | String | 50 | Endüstri/meslek lisesi varlığı. |
| `OzEgitimOkuluVarMi?` | String | 50 | Özel eğitim okulu varlığı. |
| `YurtVarMi?` | String | 50 | Öğrenci yurdu varlığı. |
| `SaglikOcagiVarMi?` | String | 50 | Sağlık ocağı veya aile sağlığı merkezi varlığı. |
| `HemsireVarMi?` | String | 50 | Sağlık personeli varlığı (Evet/Hayır). |
| `HakimYapiKalitesi` | String | 50 | Yapı kalitesi sınıfı (aşağıdaki değer listesine bakın). |
| `KullanimDurumu` | String | 50 | Mevcut kullanım durumu (aşağıdaki değer listesine bakın). |
| `AlternatifFonksiyon` | String | 50 | Alternatif fonksiyon sınıfı (aşağıdaki değer listesine bakın). |

#### `ALAN.HakimYapiTuru` değerleri

| Kod | Etiket |
| --- | --- |
| `ÇELİK KARKAS` | Çelik Karkas |
| `BETONARME KARKAS` | Betonarme Karkas |
| `YIĞMA KAGİR` | Yığma Kagir |
| `AHŞAP` | Ahşap |
| `TAŞ` | Taş |
| `KERPİÇ` | Kerpiç |
| `GECEKONDU` | Gecekondu |

#### `ALAN.YapiYili` değerleri

| Kod | Etiket |
| --- | --- |
| `1985-1994` | 1985-1994 |
| `1995-2004` | 1995-2004 |
| `2005-2014` | 2005-2014 |
| `2015-2024` | 2015-2024 |

#### `ALAN.HakimYapiKalitesi` değerleri

| Kod | Etiket |
| --- | --- |
| `ÇOK İYİ` | Çok İyi |
| `İYİ` | İyi |
| `ORTA` | Orta |
| `KÖTÜ` | Kötü |
| `ÇOK KÖTÜ` | Çok Kötü |

#### `ALAN.KullanimDurumu` değerleri

| Kod | Etiket |
| --- | --- |
| `Metruk` | Metruk |
| `Insaat` | İnşaat Halinde |
| `Bos Alan` | Boş Alan |
| `Harabe` | Harabe |
| `Kullanimda` | Kullanımda |

#### `ALAN.AlternatifFonksiyon` değerleri

| Kod | Etiket |
| --- | --- |
| `ARITMA_TESISI` | Atıksu Arıtma Tesisi |
| `ATIKSU_POMPA_ISTASYONU` | Atıksu Pompa İstasyonu |
| `BANKA` | Banka |
| `BELEDIYE_HIZMET_ALANI` | Belediye Hizmet Alanı |
| `BHA` | BHA |
| `BOS` | Boş Alan |
| `BOTANIK_CICEKCILIK` | Botanik - Çiçekçilik |
| `BOTANIK_CICEKCILIK_SERA` | Botanik - Çiçekçilik + Sera |
| `BOTANIK_FIDANCILIK` | Botanik - Fidancılık |
| `BOTANIK_FIDANCILIK_CICEKCILIK` | Botanik - Fidancılık + Çiçekçilik |
| `BOTANIK_FIDANCILIK_SERA` | Botanik - Fidancılık + Sera |
| `BOTANIK_SERA` | Botanik - Sera |
| `CAMASIRHANE` | Çamaşırhane |
| `COK_FONKSIYONLU_TICARET_HIZMET` | Çok Fonksiyonlu Ticaret-Hizmet |
| `DEPOLAMA` | Depolama |
| `DEPO_KONUT` | Depo + Konut |
| `DEPO_LOJISTIK` | Depolama + Lojistik |
| `DEPO_TARIM` | Depo + Tarım |
| `DINI_TESIS` | Dînî Tesis |
| `EGITIM_KAMPUS` | Eğitim Kampüsü |
| `EKILI_TARIM` | Ekili Tarım |
| `EMNIYET_TESISI` | Emniyet Tesisi |
| `ENERJI_ISTASYONU_AKARYAKIT` | Akaryakıt İstasyonu |
| `ENERJI_URETIM_TESISI` | Enerji Üretim Tesisi |
| `HABERLESME_ALTYAPI` | Haberleşme Altyapısı |
| `HARABE` | Harabe |
| `HAYVANCILIK` | Hayvancılık |
| `HAYVAN_BARINAGI` | Hayvan Barınağı |
| `ICMESUYU_TESISI` | İçmesuyu Tesisi |
| `ILKOKUL` | İlkokul |
| `INSAAT` | İnşaat |
| `ITFAIYE_TESISI` | İtfaiye Tesisi |
| `IZSU_TESISI` | İZSU Tesisi |
| `KAMU_HIZMET_ALANI` | Kamu Hizmet Alanı |
| `KARAYOLU_ALANI` | Karayolu Alanı |
| `KATI_ATIK_BERTARAF` | Katı Atık Bertaraf Tesisi |
| `KATI_ATIK_DONUSUM` | Katı Atık Dönüşüm |
| `KENTSEL_SERVIS_ALANI` | Kentsel Servis Alanı |
| `KONUT` | Konut |
| `KONUT_TARIM` | Konut + Tarım |
| `KONUT_TICARET` | Konut + Ticaret |
| `KONUT_TICARET_SOSYO_K` | Konut + Ticaret + Sosyo-Kültürel |
| `KONUT_TICARET_TURIZM` | Konut + Ticaret + Turizm |
| `KONUT_TURIZM` | Konut + Turizm |
| `KSS` | Küçük Sanayi Sitesi (KSS) |
| `LISE` | Lise |
| `LOJISTIK` | Lojistik |
| `MESIRE_ALANI` | Mesire Alanı |
| `MEYVE_TARIMI` | Meyve Tarımı |
| `MEZARLIK` | Mezarlık |
| `MUSTEMILAT` | Müstemilat |
| `OFIS_BURO` | Ofis/Büro |
| `ORTAOKUL` | Ortaokul |
| `OSB` | Organize Sanayi Bölgesi (OSB) |
| `OTOGAR` | Otogar |
| `OTOPARK` | Otopark |
| `OTOPARK_TICARET` | Otopark + Ticaret |
| `PARK` | Park |
| `PAZAR_YERI` | Pazar Yeri |
| `PTT` | PTT |
| `RAYLI_SISTEM` | Raylı Sistem Hattı |
| `RAYLI_SISTEM_ISTASYONU` | Raylı Sistem İstasyonu |
| `REKREASYON` | Rekreasyon Alanı |
| `RESMI_TESIS` | Resmî Tesis |
| `SAGLIK` | Sağlık |
| `SAGLIK_KAMPUS` | Sağlık Kampüsü |
| `SAGLIK_KONUT` | Sağlık + Konut |
| `SANAYI` | Sanayi |
| `SANAYI_DEPOLAMA` | Sanayi + Depolama |
| `SOSYO_KULTUREL_TESIS` | Sosyo-Kültürel Tesis |
| `SPOR_TESISI` | Spor Tesisi |
| `TARIM_TICARET` | Tarım + Ticaret |
| `TEKNIK_ALTYAPI` | Teknik Altyapı Alanı |
| `TERMINAL` | Terminal |
| `TICARET` | Ticaret |
| `TICARET_DEPO` | Ticaret + Depo |
| `TICARET_SOSYO_K` | Ticaret + Sosyo-Kültürel |
| `TRAFO` | Trafo |
| `TURIZM` | Turizm |
| `TURIZM_TESISI` | Turizm Tesisi |
| `TURIZM_TICARET` | Turizm + Ticaret |
| `ZEYTINLIK` | Zeytinlik |
| `ASKERI_ALAN` | Askeri Alan |

### 3. Diğer katmanlar

GeoPackage içerisinde `CIZGI` ve `ALAN` katmanlarına ek olarak binalar, alan grupları, çalışma sınırı ve ada poligonları gibi tamamlayıcı katmanlar bulunur. Bu katmanların öznitelik şemaları saha çalışması sırasında toplanan verilere göre yapılandırılmıştır ve yeni sürümlerde ayrıntılı tablolar eklenecektir.

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

