# 📡 İron Sulu Kumlama — Canlı Analitik Paneli Kurulum Rehberi

Bu rehber, yönetici panelindeki **"Ziyaretçi İstatistikleri"** sekmesinde
**gerçek ziyaretçi sayısını ve ziyaretçilerin geldiği illeri (şehir bazlı bölge raporu)**
canlı olarak görebilmeniz için hazırlanmıştır.

---

## Neden bu kurulum gerekiyor?

Siteniz statik bir HTML dosyası (sunucusuz) olduğu için ziyaretçi verisini
kendi başına "sayamaz". Veri, bir analitik servisinde (Google Analytics 4,
Umami vb.) birikir; yönetici panelindeki panel de bu servisin raporunu
**gömülü (embed)** olarak ekranınıza getirir. Böylece her sayı **gerçektir**.

> ⚠️ Önemli: localStorage'a "ziyaretçi sayısı" yazan çözümler yalnızca kendi
> tarayıcınızı sayar — gerçek değildir. Bu panelde gördüğünüz her rakam,
> GA4 / Umami sunucusundan gelen gerçek veridir.

---

## 0) GA4 kodu zaten sitenizde (düzeltildi)

`index.html` içindeki GA4 kodu temizlendi ve çalışır durumda:

```html
<script async src="https://www.googletagmanager.com/gtag/js?id=G-3LW561XBPQ"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'G-3LW561XBPQ', { 'send_page_view': true, 'anonymize_ip': true });
</script>
```

- `anonymize_ip: true` → IP adresleri anonimleştirilir (KVKK dostu).
- Sitede yapılan her sayfa açılışı otomatik kaydedilir.
- Telefon/WhatsApp tıklamaları da `trackEvent` ile GA4'e gönderilir.

**Test:** analytics.google.com → Gerçek zamanlı (Realtime) → sayfanızı bir
telefonda açın; ziyaretiniz 1-2 saniye içinde görünmelidir.

---

## Yöntem 1 — Looker Studio (ÖNERİLEN, ücretsiz, mevcut GA4 verinizle)

Looker Studio (eski adıyla Google Data Studio), GA4 verinizle ücretsiz görsel
panel oluşturmanızı sağlar. Kurulum ~10 dakikadır, tek seferliktir.

1. **lookerstudio.google.com** adresine girin (Google hesabınızla).
2. **"+ Boş rapor"** → **"Veri kaynağı ekle"** → **Google Analytics** → 
   mülkünüzü (G-3LW561XBPQ) seçin → bağlayın.
3. Rapora aşağıdaki bileşenleri ekleyin:
   - Skor kartı → Metrik: **Toplam kullanıcılar**
   - Skor kartı → Metrik: **Sayfa görüntüleme**
   - Tablo → Boyut: **Şehir (City)**, Metrik: **Kullanıcılar** *(il bazlı liste)*
   - Grafik → **Google Haritası**, Boyut: **Şehir**, Metrik: **Kullanıcılar** *(il haritası)*
   - İsterseniz: zaman serisi (son 28 gün), cihaz türü, trafik kaynağı grafikleri
4. Sağ üstte **"Paylaş"** butonu → erişimi
   **"Bağlantıdaki herkes görüntüleyebilir"** yapın.
5. Raporun adres çubuğundaki URL'yi kopyalayın. Şöyle görünür:
   ```
   https://lookerstudio.google.com/reporting/XXXXXXXXXXXX/page/YYYYYYYYYYYY
   ```
6. URL'deki **`reporting`** kelimesini **`embed/reporting`** yapın:
   ```
   https://lookerstudio.google.com/embed/reporting/XXXXXXXXXXXX/page/YYYYYYYYYYYY
   ```
   *(Not: Bazı hesaplarda "Dosya → Raporu göm" menüsü hazır embed kodu verir —
   o kodun içindeki `src="..."` adresini de direkt kullanabilirsiniz.)*
7. Sitenizde: Yönetici Paneli → **Ziyaretçi İstatistikleri** →
   **"🔗 Panel Bağlantısını Ayarla"** → bu embed URL'yi yapıştırın →
   **"💾 Kaydet ve Göster"**.

✅ Artık yönetici panelinizde canlı ziyaretçi sayısı ve il haritası görünecek.

---

## Yöntem 2 — Umami (alternatif: il haritası hazır gelir)

Umami, gizlilik odaklı açık kaynak analitik servisidir. Ülke, bölge, **il/şehir**
dağılımını kutudan çıktığı gibi gösterir.

1. **umami.is** → ücretsiz hesap açın (Cloud) **veya** kendi sunucunuza Docker ile kurun.
2. **Add Website** → domain: `ironkumlama.com` → size bir takip kodu verilir:
   ```html
   <script defer src="https://cloud.umami.is/script.js" data-website-id="XXXXXX"></script>
   ```
3. Bu kodu `index.html` içinde head bölümündeki
   `<!-- 📡 ÖZEL ANALİTİK: ... -->` yorum satırının **hemen altına** ekleyin.
4. Birkaç gün veri toplandıktan sonra Umami panelinde siteye girip
   **Share → Enable share link** deyin. Şöyle bir bağlantı alırsınız:
   ```
   https://cloud.umami.is/share/xxxxxxxx/siteName
   ```
5. Yönetici Paneli → Ziyaretçi İstatistikleri → **"🔗 Panel Bağlantısını Ayarla"**
   → bu linki yapıştırın → **Kaydet ve Göster**.

---

## Yönetici panelindeki butonlar

| Buton | İşlevi |
|---|---|
| 🔗 Panel Bağlantısını Ayarla | Embed URL'sini kaydeder/değiştirir |
| 💾 Kaydet ve Göster | URL'yi kaydedip paneli yükler |
| 🔄 Yenile | Paneli yeniden yükler (verileri tazeler) |
| 📊 Google Analytics 4 | GA4'e yeni sekmede gider (illik rapor: Raporlar → Kullanıcı → Konum) |

Bağlantı **yalnızca sizin tarayıcınızda** saklanır (localStorage) — site
ziyaretçileri panel bağlantısını göremez.

---

## Sorun Giderme

| Sorun | Çözüm |
|---|---|
| Panelde "boş/erişim yok" hatası | Looker Studio'da Paylaş ayarını "Bağlantıdaki herkes" yaptığınızdan emin olun. Umami'de share linkin **açık** olduğunu kontrol edin. |
| GA4'te hiç veri yok | analytics.google.com → Gerçek zamanlı → sayfayı telefonda açıp test edin. Siteyi yükledikten sonra tarayıcı önbelleğini temizleyin (Ctrl+Shift+R). |
| İller "Diğer" olarak görünüyor | GA4, az ziyaretçili verilerde gizlilik eşiği uygular. Veri biriktikçe iller tek tek görünür. Umami'de bu eşik yoktur. |
| Panel yüklenmiyor | Bağlantının https ile başladığından ve tam URL olduğundan emin olun (kırpılmamış). |
| Veriler 1 gün öncekini gösteriyor | GA4 raporları ~24-48 saat gecikmeli işlenir; "Gerçek zamanlı" sekmesi anlıktır. Looker Studio'da tarih aralığını "Son 28 gün" yapın. |

---

## Önemli Notlar

- **GA4'te il bazlı veri** ziyaretçinin IP adresinden çıkarılır; `anonymize_ip`
  açık olduğu için bu KVKK'ya uygun şekilde yapılır.
- Raporların anlamlı olması için site birkaç gün trafik aldıktan sonra
  kontrol edin.
- Umami kullanırsanız GA4'ü kapatmanıza gerek yoktur; ikisi paralel çalışabilir.
