# Arazi Kullanım GeoPackage Veri Sözlüğü

Bu doküman, `AraziKullanimDB.gpkg` GeoPackage dosyasında bulunan ana katmanları, öznitelik alanlarını ve alanların amaçlanan kullanımını özetler. Dosya, Bergama çalışma alanında yapılan arazi kullanım envanterinin dijital kaydıdır ve hem ofis hem de saha ekiplerinin ortak bir veri modeli üzerinden çalışmasını sağlar.

## GeoPackage Dosyası

| Özellik | Değer |
| --- | --- |
| Dosya adı | `AraziKullanimDB.gpkg` |
| Koordinat Referans Sistemi | EPSG:5254 (ITRF96 / TM30 3 Derece) önerilir |
| Varsayılan Ölçek | 1/1000 |
| Temel Kullanım | QGIS masaüstü ve QField/Input saha uygulamaları |

## Katmanlar

### 1. `Bergama_Binalar`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `bina_id` | Tamsayı | Her bina için benzersiz kimlik. |
| `ada_no` | Metin | 1/1000 ölçekli ada numarası. |
| `kullanim_turu` | Metin | Bina kullanım fonksiyonu (konut, ticaret, karma vb.). |
| `kat_sayisi` | Tamsayı | Bina kat adedi. |
| `yapi_durumu` | Metin | Yapının durumu (mevcut, yıkık, inşaat vb.). |
| `foto_url` | Metin | Saha fotoğrafı bağlantısı veya dosya adı. |
| `notlar` | Metin | Serbest metin notları. |

### 2. `Bergama_Yollari`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `yol_id` | Tamsayı | Yol segmenti kimliği. |
| `ada_no` | Metin | Yolun geçtiği ada numarası. |
| `kaplama` | Metin | Kaplama türü (asfalt, parke, stabilize). |
| `genislik_m` | Ondalık | Tahmini yol genişliği (metre). |
| `ulasim_turu` | Metin | Araç, yaya, bisiklet vb. |
| `notlar` | Metin | Ek açıklamalar. |

### 3. `CevreDuzeniPlani`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `plan_id` | Tamsayı | Plan dilimi kimliği. |
| `plan_karari` | Metin | Çevre düzeni planındaki arazi kullanım kararı. |
| `alt_olcek` | Metin | İlgili alt ölçek (örn. 1/25.000). |
| `onay_tarihi` | Tarih | Plan kararının onay tarihi. |

### 4. `AlanGruplari`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `grup_id` | Tamsayı | Anket formu kayıt kimliği. |
| `alan_turu` | Metin | Arazi kullanım türü (tarım, sanayi, yeşil alan vb.). |
| `grup_kodu` | Metin | Alan grubu için kod listesi değeri. |
| `durum` | Metin | Aktif, planlanan veya öneri gibi durum bilgisi. |
| `guncelleyen` | Metin | Son düzenlemeyi yapan kişi veya ekip. |
| `guncelleme_tarihi` | Tarih | Son düzenleme tarihi. |
| `anket_notu` | Metin | Saha anketinden alınan özet not. |

### 5. `AlanCalismaSiniri`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `sinir_id` | Tamsayı | Çalışma sınırı kimliği. |
| `proje_adi` | Metin | Proje veya saha kampanyasının adı. |
| `aciklama` | Metin | Sınırın tanımı ve kapsamı. |

### 6. `Ada1000`

| Alan | Tip | Açıklama |
| --- | --- | --- |
| `ada_id` | Tamsayı | 1/1000 ölçekli ada kimliği. |
| `ada_no` | Metin | İlgili resmi ada numarası. |
| `ilce` | Metin | İlçe adı (Bergama). |
| `mahalle` | Metin | Mahalle adı. |
| `yuzolcumu` | Ondalık | Ada yüzölçümü (metrekare). |

## Kod Listeleri

### `AlanGruplari.alan_turu`

| Kod | Açıklama |
| --- | --- |
| `TAR` | Tarımsal üretim alanı |
| `SAN` | Sanayi / Depolama |
| `YES` | Yeşil alan / Park |
| `KON` | Konut |
| `TIC` | Ticaret |
| `KAM` | Kamu hizmeti |

### `Bergama_Binalar.kullanim_turu`

| Kod | Açıklama |
| --- | --- |
| `R` | Konut |
| `C` | Ticari |
| `M` | Karma |
| `I` | Sanayi |
| `P` | Kamu |

## Veri Yönetimi İpuçları

* `bina_id`, `yol_id`, `grup_id` gibi tamsayı alanları otomatik artan kimlik alanları olarak kullanın ve kopya kayıt oluşturmaktan kaçının.
* Saha sırasında fotoğraf çekimi yapılıyorsa `foto_url` alanına göreli dosya yolu (örn. `media/2024-bergama/bina_102.jpg`) yazın.
* Değişiklik geçmişini izlemek için QGIS'te "Alan Ekle" aracını kullanarak `guncelleme_tarihi` alanını otomatik güncelleyen ifadeler tanımlayın.

## Sürüm Kontrolü Tavsiyeleri

* GeoPackage dosyasını Git LFS veya harici bir bulut depolama alanında saklayın.
* `data/` klasöründe `.gitignore` dosyası oluşturarak büyük boyutlu verilerin yanlışlıkla sürüm kontrolüne eklenmesini engelleyin.
* Form yapılandırmalarında yapılan değişiklikleri belgelemeniz için bu doküman üzerinde ilgili bölümü güncelleyin.

