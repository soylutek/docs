# Source: https://github.com/soyluoltu/bmp-manipulations/blob/e3f51ce103cf49ffa0fb7fcbaf098f80b2e072a5/Description.md

[soyluoltu](https://github.com/soyluoltu) / **[bmp-manipulations](https://github.com/soyluoltu/bmp-manipulations)** Public

- [Notifications](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
- [Star 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
 

 

## FilesExpand file tree

 e3f51ce

/

# Description.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/soyluoltu/bmp-manipulations/commits/e3f51ce103cf49ffa0fb7fcbaf098f80b2e072a5/Description.md)

History

62 lines (42 loc) · 2.54 KB

## FilesExpand file tree

 e3f51ce

/

# Description.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

62 lines (42 loc) · 2.54 KB

[Raw](https://github.com/soyluoltu/bmp-manipulations/raw/e3f51ce103cf49ffa0fb7fcbaf098f80b2e072a5/Description.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# BMP Manipülatörü: Kapsamlı BMP Dosya Analiz ve Steganografi Aracı

## Proje Özeti

BMP Manipülatörü, BMP (Bitmap) dosya formatına odaklanmış kapsamlı bir analiz, metadata işleme ve steganografi aracıdır. Proje, BMP formatının görüntü işleme ötesindeki potansiyelini keşfetmeyi ve bu formatın genellikle gözden kaçan özelliklerini kullanmayı amaçlamaktadır.

## Motivasyon

BMP formatı, yapısının sadeliği ve sıkıştırmasız doğası nedeniyle steganografi ve metadata işlemleri için ideal bir ortam sunar. Endüstri standardı görüntü formatları arasında, BMP formatının teknik özellikleri ve yapısal detayları genellikle yeterince incelenmemiştir. Bu proje, BMP'nin başlıklarını, metadata alanlarını ve piksel verilerini detaylı bir şekilde inceleyerek, veri gizleme, metadata entegrasyonu ve dosya bütünlüğü doğrulama gibi gelişmiş kullanım senaryoları sunmayı hedeflemektedir.

## Teknik Özellikler

### Desteklenen BMP Formatları

- **Başlık Sürümleri**:
 
 - BITMAPINFOHEADER (40 bayt)
 - BITMAPV4HEADER (108 bayt)
 - BITMAPV5HEADER (124 bayt)
- **Renk Derinlikleri**:
 
 - 8-bit İndeksli (Paletli)
 - 24-bit RGB
 - 32-bit RGBA
- **Sıkıştırma Metodları**:
 
 - BI\_RGB (Sıkıştırmasız)
 - Diğer sıkıştırma metodları için sınırlı destek

### Metadata İşlemleri

BMP Manipülatörü, BMP dosyalarına metadata entegre etmek için birden fazla yöntem sunar:

1. **Başlık Alanları Kullanımı**:
 
 - Rezerve edilmiş alanların metadata için yapılandırılması
 - Kullanılmayan başlık bölümlerini veri depolama için değerlendirme
2. **EOF (Dosya Sonu) Metadata**:
 
 - Dosya sonuna özel biçimlendirilmiş metadata blokları ekleme
 - Sağlama toplamları ile veri bütünlüğü doğrulama
 - Metadata imzası (BMPM) ile tanımlama
3. **Metadata Formatları**:
 
 - Basit anahtar-değer çiftleri
 - JSON formatında yapılandırılmış veriler
 - XMP metadata formatı desteği (geliştirme aşamasında)

### Steganografi Özellikleri

Proje, çeşitli steganografi tekniklerini uygulayarak BMP dosyalarında veri gizlemeyi mümkün kılar:

1. **LSB (En Az Önemli Bit) Steganografisi**:
 
 - 1-4 bit derinliğinde veri gizleme
 - Seçilebilir renk kanalları (R, G, B veya kombinasyonları)
 - Görünürlük etkisi minimalize edilmiş algoritma
2. **Palet Manipülasyonu**:
 
 - 8-bit paletli BMP'lerde renk paletini değiştirerek veri gizleme
 - Görüntü kalitesini korurken yüksek miktarda veri saklama
3. **Başlık Alan Steganografisi**:
 
 - Ba