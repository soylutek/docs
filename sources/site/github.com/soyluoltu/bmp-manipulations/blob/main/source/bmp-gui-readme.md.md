# Source: https://github.com/soyluoltu/bmp-manipulations/blob/main/source/bmp-gui-readme.md

[soyluoltu](https://github.com/soyluoltu) / **[bmp-manipulations](https://github.com/soyluoltu/bmp-manipulations)** Public

- [Notifications](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations) You must be signed in to change notification settings
- [Fork 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
- [Star 0](https://github.com/login?return_to=%2Fsoyluoltu%2Fbmp-manipulations)
 

 

## FilesExpand file tree

 main

/

# bmp-gui-readme.md

Copy path

Blame

More file actions

Blame

More file actions

## Latest commit

## History

[History](https://github.com/soyluoltu/bmp-manipulations/commits/main/source/bmp-gui-readme.md)

History

123 lines (81 loc) · 4.88 KB

 main

/

# bmp-gui-readme.md

Copy path

Top

## File metadata and controls

- Preview
 
- Code
 
- Blame
 

123 lines (81 loc) · 4.88 KB

[Raw](https://github.com/soyluoltu/bmp-manipulations/raw/refs/heads/main/source/bmp-gui-readme.md)

Copy raw file

Download raw file

Outline

Edit and raw actions

## Giriş

BMP Manipülatörü GUI, BMP (Bitmap) dosyalarını analiz etmek, düzenlemek ve steganografi işlemleri gerçekleştirmek için geliştirilmiş kullanıcı dostu bir grafik arayüz uygulamasıdır. Bu uygulama, komut satırı araçlarının tüm işlevlerini sezgisel bir arayüzle kullanmanızı sağlar.

## Özellikler

- **Görsel BMP Analizi**
 
 - BMP başlık bilgilerini ve dosya yapısını detaylı inceleme
 - Dosya boyutu, çözünürlük ve bit derinliği gibi bilgileri görüntüleme
 - Görüntü önizlemesi
- **Gelişmiş Renk Analizi**
 
 - Görüntüdeki benzersiz renk sayısını tespit etme
 - Renk dağılımını grafikler ile görselleştirme
 - En çok kullanılan renkler listesi
- **Metadata Yönetimi**
 
 - BMP dosyalarına metadata ekleme ve silme
 - Mevcut metadata içeriğini görüntüleme
 - Şifreli metadata desteği
- **Steganografi Araçları**
 
 - Metin veya dosya gizleme işlemleri için kullanıcı dostu arayüz
 - Güvenli şifreleme entegrasyonu
 - Bit derinliği ve renk kanalı seçenekleri
 - Gizli veri çıkarma araçları
- **Raporlama**
 
 - BMP dosyası için kapsamlı analiz raporları oluşturma
 - Renk dağılımı grafiklerini kaydetme
 - Metadata ve dosya özelliklerini raporlara dahil etme

[![Ekran görüntüsü](https://github.com/soyluoltu/bmp-manipulations/raw/main/docs/Bmp_Dosya_bilgileri.png)](https://github.com/soyluoltu/bmp-manipulations/blob/main/docs/Bmp_Dosya_bilgileri.png)

### Temel Kullanım

1. Uygulamayı başlatın: `python source/Bmp_analiz_gui.py`
2. "Dosya Seç" düğmesini kullanarak bir BMP dosyası açın
3. "Analiz Et" düğmesine tıklayarak dosya analizini başlatın
4. Sonuçları incelemek için sekmeleri kullanın:
 - "Görüntü ve Bilgiler" - BMP dosyasının önizlemesi ve temel bilgileri
 - "Renk Analizi" - Renk dağılımı grafiği ve en çok kullanılan renkler listesi

### Renk Analizi

Renk Analizi sekmesi, seçilen BMP dosyasındaki renk dağılımını gösterir:

- Pasta grafiği, en çok kullanılan 10 rengi ve bunların yüzde dağılımını gösterir
- Tablo, en çok kullanılan 100 rengin RGB değerlerini, piksel sayısını ve yüzde oranını listeler
- Renk çeşitliliği oranı, benzersiz renklerin toplam piksel sayısına oranını gösterir

### Rapor Oluşturma

Analiz sonuçlarından detaylı bir rapor oluşturmak için:

1. "Analiz" menüsünden "Rapor Oluştur" seçeneğini tıklayın
2. Rapor dosyasını kaydetmek için bir konum seçin
3. Rapor, BMP dosyasının tüm özelliklerini ve renk analizini içerecektir

### Metadata İşlemleri

Metadata işlemleri için \[Metadata Yönetimi\] menüsünü kullanabilirsiniz (Geliştirme aşamasında)

### Steganografi İşlemleri

Steganografi işlemleri için \[Steganografi\] menüsünü kullanabilirsiniz (Geliştirme aşamasında)

## Ekran Görüntüleri

### Ana Ekran

[![Ana Ekran](https://github.com/soyluoltu/bmp-manipulations/raw/main/docs/screenshots/main_screen.png)](https://github.com/soyluoltu/bmp-manipulations/blob/main/docs/screenshots/main_screen.png)

### Renk Analizi

[![Renk Analizi](https://github.com/soyluoltu/bmp-manipulations/raw/main/docs/screenshots/color_analysis.png)](https://github.com/soyluoltu/bmp-manipulations/blob/main/docs/screenshots/color_analysis.png)

### Rapor Örneği

[![Rapor Örneği](https://github.com/soyluoltu/bmp-manipulations/raw/main/docs/screenshots/report_example.png)](https://github.com/soyluoltu/bmp-manipulations/blob/main/docs/screenshots/report_example.png)

## Planlanan Özellikler

- [ ] Metadata yönetimi için tam GUI entegrasyonu
- [ ] Steganografi işlemleri için görsel arayüz
- [ ] Toplu dosya işleme desteği
- [ ] Görüntü filtreleri ve dönüşümleri
- [ ] Çoklu dil desteği
- [ ] Koyu tema desteği

## Komut Satırı Arayüzü ile Entegrasyon

BMP Manipülatörü GUI, aynı proje içindeki komut satırı araçlarıyla tam entegrasyon sunar. Komut satırı arayüzünün tüm özellikleri hakkında bilgi edinmek için [CLI Kullanım Kılavuzu](https://github.com/soyluoltu/bmp-manipulations/blob/main/source/cli-example.md) belgesine bakabilirsiniz.

## Katkıda Bulunma

BMP Manipülatörü GUI projesine katkıda bulunmak isterseniz:

1. Bu depoyu fork edin
2. Özellik dalınızı oluşturun (`git checkout -b özellik/yeni-özellik`)
3. Değişikliklerinizi commit edin (`git commit -am 'Yeni özellik: X ekle'`)
4. Dalınıza push yapın (`git push origin özellik/yeni-özellik`)
5. Bir Pull Request oluşturun

## Teknik Detaylar

### BMP Dosya Yapısı

BMP Manipülatörü GUI, BMP dosya formatının yapısını analiz eder ve görselleştirir. BMP formatı hakkında daha fazla bilgi için [BMP Yapısı Dokümantasyonu](https://github.com/soyluoltu/bmp-manipulations/blob/main/bmp-structure.md) belgesine bakabilirsiniz.

### Renk Analizi Algoritması

Renk analizi modülü, BMP piksel verilerini tarayarak benzersiz renkleri tespit eder ve istatistiksel analiz yapar. Algoritma, farklı bit derinliklerindeki BMP dosyalarını destekler ve verimli bellek kullanımı için optimize edilmiştir.

### Güvenlik Özellikleri

Steganografi ve metadata şifreleme modülleri, AES-256-GCM ve ChaCha20-Poly1305 gibi güçlü şifreleme algoritmalarını kullanır. Şifreleme için PBKDF2 anahtar türetme fonksiyonu ve güvenli rastgele sayı üreteci kullanılmaktadır.

## Lisans

Bu proje MIT Lisansı altında lisanslanmıştır. Detaylar için [LICENSE](https://github.com/soyluoltu/bmp-manipulations/blob/main/source/LICENSE) dosyasına bakın.

## İletişim

Sorular, öneriler veya geri bildirimler için:

- GitHub Issues: [github.com/soyluoltu/bmp-manipulations/issues](https://github.com/soyluoltu/bmp-manipulations/issues)