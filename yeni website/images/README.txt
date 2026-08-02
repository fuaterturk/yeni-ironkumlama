IRON KUMLAMA - GÖRSEL KLASÖR YAPISI VE KULLANIM REHBERİ
======================================================

Sitenizin süper hızlı açılması ve görsellerin düzenli olması için klasör yapısı aşağıdaki gibidir:

1. LOGO GÖRSELİ:
   - Yol: images/logo.png (veya images/logo.jpg / logo.svg)
   - Sitedeki sol üst köşede ve footer'da otomatik görünür.

2. ARKA PLAN / KAPAK GÖRSELİ (HERO COVER):
   - Yol: images/hero-bg.jpg
   - Ana sayfa girişindeki büyük kapak alanında arkaplan olarak yüklenir.
   - Hızlı açılma için sayfa yüklendikten hemen sonra arkada yumuşakça belirir (0 saniye gecikme).

3. ÖNCESİ & SONRASI GÖRSELLERİ (BEFORE / AFTER):
   - Öncesi (Paslı/Korozyonlu hal): images/before-after/before.jpg
   - Sonrası (Kumlanmış & Boyalı hal): images/before-after/after.jpg

4. PROJE GALERİSİ GÖRSELLERİ:
   - Yol: images/galeri/1.jpg, images/galeri/2.jpg ... images/galeri/12.jpg
   - Sitedeki akıllı galeri bu klasördeki resimleri otomatik olarak çeker ve listeler.

PERFORMANS İPUCU (Hızlı Yükleme):
- Web sitenizde tüm görsellere "lazy loading" (tembel yükleme) ve "async decoding" özellikleri eklenmiştir.
- Bu sayede 50 tane resim ekleseniz bile web siteniz 0.1 saniyede anında açılır, resimler ekrana geldikçe arka planda sessizce yüklenir!
