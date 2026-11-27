# Sağlık Kiosk İndeksi

[Hub](hub.md) · [Günlük](log.md)

`wiki/saglik-kiosk/` altındaki tüm sayfaların listesi; giriş noktası hub'dır.

## Sayfalar

- [Genel Mimari (Hub)](hub.md) — giriş noktası
- [BerryMed YS2000A SpO2 Modülü](berrymed-ys2000a-spo2.md) — Cypress USBUART üzerinden 4800 baud BCI akışı veren parmak oksimetresinin 5 baytlık paket yapısı, kararlı SpO2 kilidi ve pleth dalgası.
- [Donanım Keşfi, USB Envanteri ve Port Çözümleme](donanim-kesfi-ve-usb.md) — USB Host, sysfs ve /dev katalogları, VID/PID rol haritası, el sıkışma sondaları, izin yönetimi ve root gerektiren tty/hidraw işlemleri.
- [DYP-H01 Boy (Mesafe) Sensörü](dyp-h01-boy.md) — CH340 USB-UART üzerinden 9600 baud konuşan DYP-H01 mesafe sensörünün çerçeve biçimi, boy hesabı, kararlılık algoritması ve mock davranışı.
- [Faz 2 — Termal Yazıcı, Vücut Analizi, Merkeze Yükleme](faz2-yazici-vucut-analizi-yukleme.md) — Henüz uygulanmamış faz 2 kalemleri; Masung yazıcı SDK'sı, BMH05108 8 elektrotlu vücut analizi ve protokol 1.0.4 HTTP yüklemesi.
- [Teknik Fizibilite Raporu (19 Eylül 2026)](fizibilite-raporu.md) — Üst yönetim için hazırlanmış fizibilite raporunun özeti; yazılabilirlik kararı, modül değerlendirmesi, riskler, fazlar ve donanım zorunluluğu.
- [Geliştirme Süreci (Initium, YZ İş Akışı, CI)](gelistirme-sureci.md) — Projenin türetildiği Initium iskeleti, Claude/Cursor/Continue kural ve komut setleri, ajan yapılandırması, derleme komutları ve CI hattı.
- [HeTaiDa HTD8808C Ateş (HID) Termometresi](hetaida-htd8808c-ates.md) — USB HID olarak çalışan HeTaiDa kızılötesi termometrenin paket biçimi, iki taşıma yolu (UsbManager ve hidraw) ve ateş ölçüm davranışı.
- [Hexagonal Mimari ve Portlar](hexagonal-mimari.md) — Projenin Ports & Adapters katmanları, port arayüzleri, adaptör envanteri, ADR 0001 ve proje dokümanları ile gerçek kod arasındaki farklar.
- [INBCA Ürün Kataloğu (2025)](inbca-urun-katalogu.md) — Eklenen INBCA İngilizce ürün kataloğunun özeti; vücut kompozisyonu aynası, sağlık istasyonları ve tansiyon ürünleri ve projeyle ilişkisi.
- [Kiosk UI Akışı ve Durum Makinesi](kiosk-ui-akisi.md) — Compose ekranları, KioskViewModel durumu, tur adımları, hata/atlama davranışı ve ekranlara özgü kurallar.
- [Lztek Anakart, sdkapi.jar ve KEC-3566](lztek-anakart.md) — Endüstriyel Android anakartı, Lztek sdkapi.jar API'sinin projedeki kullanımı, kiosk modu, kök yetki gereksinimleri ve kullanılmayan özellikler.
- [OEM ASCII Protokolü (#…$ ve M…$)](oem-ascii-protokolu.md) — Tedarikçi dokümanlarındaki ASCII vital protokolü, kodda hâlâ duran komut/çözücü katmanı ve çalışan USB cihaz turuyla ilişkisi.
- [Ölçüm Oturumu (MeasurementSession)](olcum-oturumu.md) — Tek modül ölçümü, sıralı tur, zaman aşımları, hata dönüşümü ve BMI hesabının uygulama katmanındaki davranışı.
- [Raycome RBP-7000 Tansiyon Cihazı](raycome-rbp7000-tansiyon.md) — FTDI USB-UART üzerinden 9600 baud V1.3 ikili protokolle konuşan Raycome RBP-7000'in paket biçimi, ölçüm akışı, hata kodları ve FTDI/DTR sorunları.
- [Sağlık Kiosk Android Uygulaması](saglik-kiosk-app.md) — Endüstriyel Android kioskta boy, ateş, SpO2 ve tansiyonu dokunmatik ekranda sırayla ölçen Kotlin/Compose uygulamasının genel bakışı, derleme bilgisi ve açılış kablolaması.
- [Test Stratejisi ve Kapsamı](test-stratejisi.md) — Donanımsız birim testlerin kapsamı, sahte seri/USB katmanları, boşluklar ve donanım kabul testi gereksinimi.
- [Veri Yükleme Arayüzleri (1.0.4, Pre-job v1.2, Smart Health Cabin v1.15)](veri-yukleme-arayuzleri.md) — Ölçüm verisini merkeze taşıyan üç HTTP sözleşmesinin karşılaştırması; yeniden deneme kuralları, alan şemaları, ek uç noktalar ve örnek veri dosyası.
- [Figma Ekran Tasarımı (Tedarikçi, İngilizce Standart)](figma-ekran-tasarimi.md) — Tedarikçinin Figma dışa aktarımındaki kiosk ekran akışı ve mevcut uygulamayla karşılaştırma.
- [Geliştirici Rehberi (C# / C++ Qt Geçmişi İçin)](gelistirici-rehberi-cpp-qt.md) — Kod tabanının C#/Qt kavramlarıyla anlatımı, dosya haritası, yeni cihaz ekleme kontrol listesi ve entegrasyon mimarisi önerisi.
- [Yönetim Paneli ve Ekran Koruyucu](admin-paneli-ve-ekran-koruyucu.md) — Gizli köşe + PIN ile açılan yönetim paneli, ana ekranda boşta açılan sessiz video; referans kiosk karşılaştırması ve fazlar.
- [Vücut Analizi — Tartı Kartı #FM Yolu](vucut-analizi-tarti-karti-fm.md) — Bugünkü tahmini hesap, BMH05108 0xD2 yanıtsız (Master V5.1/Bia V2.1), diğer kioskun Dingheng tartı kartına gönderdiği `#FM` komutu ve yanıt alanları; yarınki yapılacaklar.
- [Yeni Sensör Kiti — Eksik Analizi ve Eylem Planı](yeni-sensor-kiti-analizi.md) — Dingheng kiti (boy, SpO2, kilo, seri ateş) ve BMH05108'in kodla farkları, Yöntem 1/2, Çarşamba sunumu adımları.
