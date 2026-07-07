# Source: https://github.com/soyluoltu/bmp-manipulations/blob/3172945bfea2e02990681390b03b9c1cc8931257/readme.md

[soyluoltu](https://github.com/soyluoltu) / **[bmp-manipulations](https://github.com/soyluoltu/bmp-manipulations)** Public

- [Notifications](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
- [Star 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
 

 

## FilesExpand file tree

 3172945

/

# readme.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/soyluoltu/bmp-manipulations/commits/3172945bfea2e02990681390b03b9c1cc8931257/readme.md)

History

132 lines (92 loc) · 4.67 KB

## FilesExpand file tree

 3172945

/

# readme.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

132 lines (92 loc) · 4.67 KB

[Raw](https://github.com/soyluoltu/bmp-manipulations/raw/3172945bfea2e02990681390b03b9c1cc8931257/readme.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

# BMP Manipülatörü

Metadata işlemleri ve steganografi odaklı kapsamlı bir BMP dosya manipülasyon aracı.

[![BMP Manipülatörü Logo](https://github.com/soyluoltu/bmp-manipulations/raw/3172945bfea2e02990681390b03b9c1cc8931257/docs/logo.png)](https://github.com/soyluoltu/bmp-manipulations/blob/3172945bfea2e02990681390b03b9c1cc8931257/docs/logo.png)

[![Lisans: MIT](https://camo.githubusercontent.com/08cef40a9105b6526ca22088bc514fbfdbc9aac1ddbf8d4e6c750e3a88a44dca/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f4c6963656e73652d4d49542d626c75652e737667)](https://github.com/soyluoltu/bmp-manipulations/blob/3172945bfea2e02990681390b03b9c1cc8931257/LICENSE) [![Yapı Durumu](https://camo.githubusercontent.com/62c1fad0f6693ee786750ed1d10badb5b51cf87e5afe47eddcdf6a04466319f0/68747470733a2f2f696d672e736869656c64732e696f2f6769746875622f776f726b666c6f772f7374617475732f736f796c756f6c74752f626d702d6d616e6970756c61746f722f4349)](https://github.com/soyluoltu/bmp-manipulator/actions) [![Versiyon](https://camo.githubusercontent.com/9b8a63eb5a126d3f3cbb0562344d18b70c87ad94abe2d66bbf324e05c489cfb2/68747470733a2f2f696d672e736869656c64732e696f2f62616467652f76657273696f6e2d312e302e302d627269676874677265656e2e737667)](https://github.com/soyluoltu/bmp-manipulator/releases)

## Giriş

BMP Manipülatörü, BMP (Bitmap) dosyalarıyla temel görüntü düzenlemenin ötesinde çalışmak için özelleştirilmiş bir kütüphanedir. BMP formatının genellikle gözden kaçan yönlerine odaklanır: başlıklar, metadata ve steganografi dahil çeşitli amaçlar için kullanılabilecek kullanılmayan alanlar.

## Temel Özellikler

- **Tam BMP Başlık Yönetimi**
 
 - BMP başlıklarının tüm yönlerini inceleme, değiştirme ve genişletme
 - Tüm BMP başlık versiyonları ve yapıları için destek
 - Başlık bütünlüğü için gelişmiş doğrulama
- **Genişletilmiş Metadata İşlemleri**
 
 - BMP dosyalarına özel metadata alanları ekleme
 - Mevcut metadataları çıkarma ve yorumlama
 - Standartlara uygun ve özel metadata formatları için destek
- **Steganografi Araç Seti**
 
 - Çoklu steganografi algoritmaları (LSB, Palet manipülasyonu, vb.)
 - Özelleştirilebilir veri gizleme stratejileri
 - Gizli veriler için şifreleme desteği
 - Steganaliz ve tespit için araçlar
- **Geliştirici Dostu Tasarım**
 
 - Programatik kullanım için temiz API
 - Kapsamlı CLI komutları
 - Detaylı belgelendirme ve örnekler

## BMP Yapısına Genel Bakış

BMP dosya formatı birkaç temel bileşenden oluşur:

1. **Dosya Başlığı (14 bayt)**
 
 - Dosya türü, boyutu ve piksel verisine offset
2. **DIB Başlığı (değişken boyut)**
 
 - Görüntü boyutları, renk derinliği ve sıkıştırma bilgisi
 - Farklı sürümleri: BITMAPINFOHEADER (40 bayt), BITMAPV4HEADER (108 bayt), BITMAPV5HEADER (124 bayt)
3. **Renk Paleti (isteğe bağlı)**
 
 - 8-bit veya daha düşük renk derinliğindeki görüntüler için
4. **Bit Maskesi (isteğe bağlı)**
 
 - Bazı renk formatları için
5. **Piksel Verisi**
 
 - Satır başına 4-baytlık hizalama gerektirir
 - Alt-üst yapılandırması (aşağıdan yukarıya doğru dizilir)
6. **ICC Renk Profili (isteğe bağlı, BITMAPV5HEADER ile)**
 
 - Renk yönetimi için
7. **Özel Metadata Alanları (bu proje ile)**
 
 - Standart yapıda tanımlanmayan ek veriler

## Metadata İşlemleri

BMP Manipülatörü, BMP dosyalarında bulunan standart metadata alanları dışında özel metadata eklemek için birkaç yöntem sunar:

### Metadata Ekleme Yöntemleri

1. **Başlık Genişletmesi**
 
 - DIB başlığının kullanılmayan alanlarını kullanma
 - BITMAPV5HEADER'ın rezerve edilmiş alanlarını kullanma
2. **Application Extension Blocks**
 
 - Piksel verisinden sonra özel bloklar ekleme
 - Uygulamaya özel tanımlayıcılar kullanma
3. **Metadata Bölümleri**
 
 - Dosya sonuna metadata eklemek için özel format
 - Başlıkta referans olmadan gizli veri saklama

### Metadata Formatları

- **Anahtar-Değer Çiftleri**
 
 - Basit metin tabanlı metadata
 - JSON formatında yapılandırılmış veriler
- **XMP Metadata**
 
 - Adobe XMP formatı ile uyumlu
 - Standartlaştırılmış metadata şemaları
- **Özel İkili Formatlar**
 
 - Özel uygulamalar için optimize edilmiş
 - Sıkıştırılmış veya şifrelenmiş metadata

## Steganografi Özellikleri

BMP Manipülatörü, çeşitli steganografi tekniklerini destekler:

### Desteklenen Teknikler

1. **En Az Önemli Bit (LSB) Steganografisi**
 
 - Piksel verilerinin en az önemli bitlerinde bilgi gizleme
 - Ayarlanabilir bit derinliği (1-4 bit)
 - Kanal seçimi (R, G, B veya tümü)
2. **Palet Tabanlı Steganografi**
 
 - Paletli BMP'lerde renkler arasındaki sıralamayı değiştirme
 - Görsel değişiklikler olmadan veri gizleme
3. **Başlık Alanı Steganografisi**
 
 - Kullanılmayan veya az kullanılan başlık alanlarında veri gizleme
 - Görüntü verisi etkilenmeden veri saklama
4. **Boş Alan Kullanımı**
 
 - Piksel verisi ve dosya sonu arasındaki boşluğu kullanma
 - Yüksek kapasite, düşük tespit edilebilirlik

### Güvenlik Özellikleri

- **Şifreleme Entegrasyonu**
 
 - AES-256, ChaCha20 gibi modern şifreleme algoritmaları
 - Steganografi öncesi veri şifreleme
- **Dağıtılmış Gizleme**
 
 - Verinin dosya genelinde dağıtılması
 - Tespit zorluğunu arttırma
- **Sahte Positif Enjeksiyonu**
 
 - Steganaliz araçlarını yanıltmak için sahte veri
 - Gerçek gizli veriyi maskeleme