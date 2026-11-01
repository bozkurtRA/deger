değeR v.3 — Tek Dosyalık Web Uygulaması
=======================================

1. ÖZET
-------
Bu teslim, özgün mobil uygulamanın ekranlarını, hesaplama mantığını ve
görsel kimliğini tek bir HTML dosyasında yeniden oluşturur.

    deger.html

Dosyanın içinde şunlar gömülüdür:
  - Tüm HTML, CSS ve JavaScript kodu
  - Logo, TEDAŞ logosu, Sil ve Undo görselleri
  - Sekme simgeleri
  - Uygulama simgeleri ve web uygulaması bildirimi (manifest)

Harici CSS, JavaScript, resim, yazı tipi, CDN veya ağ isteği yoktur.
Bu nedenle dosya cihazda yerel olarak açıldığında internet bağlantısı
gerekmez ve hesaplamalar tamamen cihaz içinde yapılır.

Bu README.txt uygulamanın parçası değildir; yalnızca kullanım ve teslim
notlarını içerir. Uygulamayı çalıştırmak için tek başına deger.html yeterlidir.


2. HIZLI BAŞLANGIÇ
------------------
Android:
  1. deger.html dosyasını telefona/tablete kopyalayın (İndirilenler vb.).
  2. Dosyalar uygulamasında deger.html dosyasına dokunun.
  3. "Chrome ile aç" seçeneğini kullanın.
  4. Adres çubuğunda file:///... biçiminde bir adres görünebilir.

  Alternatif (önerilen): Bilgisayarda veya telefonda basit bir yerel sunucu
  çalıştırıp aynı ağdan açın. Örnek:
      python -m http.server 8000
  Telefonun tarayıcısında:
      http://BILGISAYARIN_IP_ADRESI:8000/deger.html

iOS / iPadOS:
  Dosyalar uygulamasından bir HTML dosyasına dokunmak her sürümde
  JavaScript çalıştırmayabilir. En güvenilir yol, dosyayı HTTPS üzerinden
  bir statik sunucuda yayınlayıp Safari ile açmaktır. Yerel ağda
  http://BILGISAYARIN_IP_ADRESI:8000/deger.html adresi de kullanılabilir.

Masaüstü:
  deger.html dosyasını Chrome, Edge, Firefox veya Safari ile açın.
  Yerel dosya olarak tüm hesaplamalar ve veri saklama çalışır.

Barındırma:
  Dosyayı herhangi bir statik hostinge yükleyebilirsiniz (GitHub Pages,
  Netlify, kurumsal web sunucusu vb.). İsterseniz dosya adını index.html
  yapabilirsiniz. Sunucu tarafı kod, veritabanı veya özel yapılandırma
  gerekmez. HTTPS kullanılması önerilir.

Ana ekrana ekleme:
  - Android Chrome: Menü > "Ana ekrana ekle" / "Uygulamayı yükle".
  - iOS Safari: Paylaş > "Ana Ekrana Ekle".
  HTTP(S) üzerinden açıldığında uygulama, çalışma anında kendi manifestini
  tamamlar ve uygun tarayıcılarda yüklenebilir hale gelir. Yerel file://
  adreslerinde tarayıcıların kurulum/PWA kısıtları geçerlidir.


3. EKRANLAR
-----------
Alt sekmede iki ekran vardır:

  değeR      : Tersine değer, arsa birim değeri, irtifak ve mülkiyet
               hakkı hesaplamaları.
  Hakkında   : Uygulama tanıtımı, sürüm bilgisi, e-posta bağlantısı ve
               TEDAŞ logosu.

3.1. Hesaplama ekranındaki alanlar
----------------------------------
Giriş alanları:
  - Yüz Ölçüm [ m² ]
  - Toplam Gelir [ TL ]
  - Toplam İnşaat Alanı [ m² ]
  - İnşaat m² Birim Fiyat [ TL ]
  - Müteahhit Kar Oranı [ % ]
  - ~ Arsa [ TL/m² ]
  - ~ m²
  - Taşınmaz Birim Değeri [ TL/m² ]
  - İrtifak Hakkı Miktarı [ m² ]
  - DDO [ % ]
  - Mülkiyet Hakkı Miktarı [ m² ]

Salt okunur sonuç alanları:
  - Müteahhit Kar Payı [ TL ]
  - Toplam İnşaat Maliyeti [ TL ]
  - Arsa [ TL/m² ]  (kırmızı, büyük sonuç)
  - ~ Toplam Gelir [ TL ]
  - ~ Bürüt [ TL/m² ]
  - ~ Bürüt Sonuç [ TL ]
  - İrtifak Hakkı Değeri [ TL ]
  - Mülkiyet Hakkı Değeri [ TL ]
  - Toplam Değer [ TL ]

Sil düğmesi:
  Hesaplama alanlarını temizler. Özgün uygulamanın davranışı korunmuştur:
  "Müteahhit Kar Oranı [ % ]" alanı temizlenmez, diğer alanlar temizlenir.

Undo (geri al) simgesi:
  Özgün uygulamada tıklama olayı olmadığı için yalnızca görsel bir öğedir.
  Bu sürümde de işlevsizdir ve ekran okuyuculardan gizlenmiştir.

Ondalık ayırıcı:
  Hem nokta (12.5) hem virgül (12,5) ondalık ayırıcı olarak kabul edilir.
  Sonuçlar her zaman nokta ile ve gereksiz sondaki sıfırlar olmadan
  gösterilir (12.5 gibi). Binlik ayırıcı eklenmez.


4. HESAPLAMA KURALLARI
----------------------
Tüm sonuçlarda iki ondalık basamak kullanılır:

    round2(n) = Math.round(n * 100) / 100

Formüller:

    Müteahhit Kar Payı
      = round2(ToplamGelir * KarOran / 100)

    Toplam İnşaat Maliyeti
      = round2(ToplamInsaatAlani * BirimFiyat)

    Arsa [ TL/m² ]
      = round2(((ToplamGelir - KarPayi) - ToplamInsaatMaliyeti) / Alan)

    İrtifak Hakkı Değeri
      = round2(((TasinmazBirimDeger * DDO) / 100) * IrtifakMiktari)
      (İrtifak miktarı veya DDO boşsa "0")

    Mülkiyet Hakkı Değeri
      = round2(TasinmazBirimDeger * MulkiyetMiktari)
      (Mülkiyet miktarı boşsa "0")

    Toplam Değer
      = round2(IrtifakHakkiDegeri + MulkiyetHakkiDegeri)
      (Ekranda yuvarlanmış alan değerleri kullanılır.)

    ~ Toplam Gelir
      = round2(((TersineArsaBedel * Alan) + ToplamInsaatMaliyeti)
               / (1 - KarOran / 100))

    ~ Bürüt [ TL/m² ]
      = round2(TersineToplamGelir / ToplamInsaatAlani)
      (Ekranda yuvarlanmış ~ Toplam Gelir değeri kullanılır.)

    ~ Bürüt Sonuç
      = round2(m2 * Burutm2Gelir)
      (Ekranda yuvarlanmış ~ Bürüt değeri kullanılır.)

Olay sırası özgün uygulamadakiyle aynıdır:
  - Toplam Gelir değişince: önce Kar Payı, sonra Arsa.
  - Kar Oranı değişince: önce Kar Payı, sonra Arsa.
  - Toplam İnşaat Alanı değişince: önce Maliyet, sonra Arsa.
  - Birim Fiyat değişince: önce Maliyet, sonra Arsa.
  - Yüz Ölçüm değişince: Arsa.
  - Taşınmaz Birim Değeri / İrtifak Miktarı / DDO / Mülkiyet Miktarı
    değişince: Taşınmaz değerleri.
  - ~ Arsa değişince: Tersine toplam gelir ve bürüt m².
  - ~ m² değişince: Bürüt sonuç.

Boş alan kuralı:
  Bir hesap için gereken alanlardan biri boşsa ilgili sonuç temizlenir.
  Taşınmaz hesabında Taşınmaz Birim Değeri boşsa üç sonuç da temizlenir;
  doluysa eksik alt alanlar "0" kabul edilir.

Sıfıra bölme:
  Özgün uygulamadaki JavaScript davranışı korunur. Geçersiz/sıfır
  bölenlerde sonuç "Infinity", "NaN" veya "-Infinity" olarak görünebilir.
  Gerçek kullanımda ilgili alanların dolu ve sıfırdan farklı olması
  önerilir.


5. VERİ SAKLAMA
---------------
Girilen değerler otomatik olarak tarayıcının yerel deposuna yazılır:

    localStorage anahtarı: deger.v3.fields

Sunucuya veri gönderilmez. Veriler yalnızca aynı tarayıcı ve aynı cihaz
profilinde saklanır. Gizli/özel sekmede veya depolama engelliyse kalıcılık
çalışmayabilir; hesaplamalar yine de çalışır. Tarayıcı site verileri
temizlenirse kayıtlı değerler silinir.

Not: file:// ile açılan sayfalarda tarayıcıların yerel depolama kuralları
değişebilir. Kalıcı veri ve ana ekrana ekleme için HTTP(S) üzerinden
sunmak daha güvenilirdir.


6. TEKNİK VE UYUMLULUK
----------------------
  - Mobil öncelikli, akışkan (responsive) yerleşim.
  - 320 px'ten geniş ekranlara kadar yatay taşma olmadan çalışır.
  - Dikey ve yatay yönlendirme desteklenir.
  - Çentik ve ana ekran çubuğu için safe-area boşlukları kullanılır.
  - Giriş alanları 16 px yazı boyutundadır; iOS'ta otomatik yakınlaştırma
    oluşmaz.
  - Dokunma hedefleri en az 44–46 px yüksekliğindedir.
  - Alt sekme çubuğu sabittir; içerik doğal sayfa kaydırmasıyla ilerler.
  - Etiketler, ARIA rolleri, seçili sekme durumu ve klavye odağı desteği
    vardır. Sekmelerde sağ/sol ok, Home ve End tuşları çalışır.
  - Form alanlarında Enter, bir sonraki düzenlenebilir alana geçer.
  - Tek dosya olduğu için ayrı bir service worker dosyası yoktur.
    Yerel dosya tamamen çevrimdışıdır. HTTP(S) üzerinden kurulan sürümde
    çevrimdışı ilk açılış tarayıcı önbelleğine bağlıdır.
  - Görseller data: URI olarak gömülüdür; ağ isteği oluşturmaz.
  - Manifest, HTTP(S) ortamında sayfa adresine göre çalışma anında
    tamamlanır. Böylece dosya farklı bir alan adında da yüklenebilir.


7. YAPILAN TESTLER
------------------
Aşağıdaki testler bu teslimde fiilen çalıştırılmıştır:

Yerleşim (Chromium):
  320x568, 360x640, 375x667, 375x812, 390x844, 414x896, 430x932,
  667x375, 812x375, 844x390 ve 768x1024.
  Her birinde:
    - scrollWidth = görünüm genişliği (yatay taşma yok)
    - 20 form alanı mevcut
    - giriş yazı boyutu 16 px
    - alt sekme çubuğu görünüm genişliğine sığıyor
    - hesaplama ekranının son alanı sekme çubuğunun üstüne kaydırılabiliyor

Yerleşim (WebKit / Safari motoru):
  iPhone 13 (390x664), iPhone SE (320x568), iPhone 13 Pro Max (428x746),
  iPad Mini (768x1024), 844x390 yatay.
  Yatay taşma yok; hesaplamalar, yerel depolama ve dvh desteği çalışıyor;
  konsol hatası yok.

Hesaplama testi:
  Alan=1000, Toplam Gelir=10.000.000, Kar Oranı=20,
  İnşaat Alanı=5000, Birim Fiyat=10.000,
  Tersine Arsa=10.000, m²=100,
  Taşınmaz Birim Değeri=5000, İrtifak=200, DDO=30, Mülkiyet=400.

  Beklenen ve doğrulanan sonuçlar:
    Kar Payı                 2.000.000
    Toplam İnşaat Maliyeti  50.000.000
    Arsa                    -42.000
    ~ Toplam Gelir          75.000.000
    ~ Bürüt                 15.000
    ~ Bürüt Sonuç            1.500.000
    İrtifak Hakkı Değeri     300.000
    Mülkiyet Hakkı Değeri  2.000.000
    Toplam Değer           2.300.000

Yuvarlama ve sıra testi:
  Alan=3, Gelir=100, Kar Oranı=33.333, İnşaat Alanı=10, Birim Fiyat=2.
  Kar Payı 33.33 olarak yuvarlanmış, Arsa hesabında bu yuvarlanmış değer
  kullanılmış ve sonuç 15.56 bulunmuştur.

Diğer davranış testleri:
  - Virgüllü ondalık girişi (1,5 ve 2,5) doğru hesaplandı.
  - ~ Arsa temizlenince bağlı dört sonuç temizlendi.
  - Sil düğmesi 19 alanı temizledi; Kar Oranı korundu.
  - Sayfa yenilendiğinde 20 alanın tamamı yerel depodan geri yüklendi.
  - Sekme değişimi, geri/ileri gezinme ve kaydırma konumu korundu.
  - file:// ile açılışta hesaplama, görseller ve localStorage çalıştı.
  - HTTP ve file:// açılışlarında konsol hatası oluşmadı.
  - Harici ağ isteği oluşmadı (yalnızca dosya/data: istekleri).
  - Erişilebilirlik, en iyi uygulamalar ve SEO denetimleri 100/100/100
    sonuç verdi (masaüstü Chromium denetimi).

Test edilmeyenler:
  - Fiziksel Android ve iOS cihazlar
  - Gerçek mobil klavye açılma/kapanma animasyonları
  - Ekran okuyucular (TalkBack / VoiceOver) ile uçtan uca kullanım
  - Çok eski tarayıcı sürümleri (ör. iOS 12, Android 7)


8. ÖZGÜN UYGULAMADAN BİLİNÇLİ FARKLAR
-------------------------------------
  - Ekranlar 375 px genişliğinde sabit koordinatlarla değil, akışkan ve
    mobil öncelikli bir yerleşimle oluşturulmuştur. Kullanıcı isteğiyle
    kutu genişlikleri ve dikey ölçüler uyarlanabilir hale getirilmiştir.
  - Hakkında ekranı kısa ve yatay ekranlarda kaydırılabilir.
  - Giriş alanları dokunma kolaylığı için daha yüksek, yazı boyutu iOS
    yakınlaştırmasını önlemek için 16 px'tir.
  - Odak halkası ve daha belirgin pasif sekme rengi erişilebilirlik için
    eklenmiştir.
  - Sil düğmesi, özgün uygulamadaki gibi Kar Oranı alanını korur.
  - Undo simgesi görseldir; tıklama işlevi yoktur.
  - "Bürüt" yazımı özgün etiketlerdeki gibi korunmuştur.
  - Tema rengi ve durum çubuğu görünümü web standardına uyarlanmıştır.


9. DOSYA BİLGİSİ
----------------
  Dosya      : deger.html
  Boyut      : 629.138 bayt (yaklaşık 615 KB)
  SHA-256    : DD8CDAD66CDA67A321B934CA6C52F89383746E5F5BF4A67FAE0CDBC759269A6B
  Sürüm      : değeR v.3 (web)
  Dil        : Türkçe

Not: SHA-256 değeri bu README yazıldığı sıradaki deger.html dosyasına
aittir. Dosyada değişiklik yapılırsa hash de değişir.
