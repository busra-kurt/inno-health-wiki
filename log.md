# Sağlık Kiosk Wiki Günlüğü

[Sağlık Kiosk Hub](hub.md) · [Sağlık Kiosk İndeks](index.md)

Sadece-ekleme (append-only) kronolojik kayıt; yalnız `wiki/saglik-kiosk/` ile ilgili girdiler.

## [2026-10-12] ingest | Sağlık Kiosk wikisi oluşturuldu

Kaynak: `raw/saglik-kiosk` (Android uygulaması, tedarikçi belgeleri, `etc/` cihaz klasörleri, Lztek SDK). Karpathy LLM-wiki yaklaşımı ve vault `CLAUDE.md` şeması (Hub-and-Spoke) izlendi; `wiki/saglik-kiosk/` altında 1 hub + 17 spoke sayfası yazıldı.

**Spoke'lar:** `saglik-kiosk-app`, `hexagonal-mimari`, `olcum-oturumu`, `kiosk-ui-akisi`, `donanim-kesfi-ve-usb`, `dyp-h01-boy`, `hetaida-htd8808c-ates`, `berrymed-ys2000a-spo2`, `raycome-rbp7000-tansiyon`, `lztek-anakart`, `oem-ascii-protokolu`, `fizibilite-raporu`, `faz2-yazici-vucut-analizi-yukleme`, `gelistirme-sureci`, `test-stratejisi`, `veri-yukleme-arayuzleri`, `inbca-urun-katalogu`.

**Oturum içinde eklenen kaynaklar** (`INBCA EN Catalogue…pdf`, `Pre-job_Examination…V1.2_EN.docx`, `interface protocol/` — Smart Health Cabin v1.15 PDF'i ve `sample data.txt`) `veri-yukleme-arayuzleri` ve `inbca-urun-katalogu` sayfalarına işlendi.

**Önemli bulgular:** (1) Belgeler/rapor Lztek UART + ASCII tasarımı anlatıyor, kod dört USB cihaza göre yazılmış; (2) kilo, vücut analizi, yazıcı, HTTP ve SQLite yok; (3) `runVitalTour` arayüzde kullanılmıyor; (4) üç farklı yükleme sözleşmesi var, hedefi belirsiz. Okunamayanlar: `.env` (bilerek), `K-3566 Motherboard Spec` PDF'i, `sdkapi` içindeki root zip'leri ve katalogdaki görsel içerik.

## [2026-10-12] lint | Sağlık Kiosk wikisi doğrulandı

`wiki/saglik-kiosk/` (18 sayfa) için otomatik denetim: frontmatter alanları tam, spoke'larda breadcrumb var, her Mermaid bloğu `%%{init…}%%` ile başlıyor, kırık link ve yetim sayfa yok, tüm spoke'lar hub'dan bağlı. Kaynak dosyalara dokunulmadı. Bilinen açıklar: `K-3566 Motherboard Spec` PDF'i okunamadı; INBCA kataloğu yalnız metin katmanından özetlendi.

## [2026-10-12] note | Günlük ve indeks proje bazında ayrıldı

Sağlık Kiosk girdileri ortak `wiki/log.md`'den bu dosyaya taşındı, `saglik-kiosk/index.md` oluşturuldu; `wiki/index.md` ve `wiki/log.md` artık yalnız projelere işaret ediyor.

## [2026-10-12] ingest | Derleme ve kurulum adımları eklendi

`gelistirme-sureci.md` içine "Derleme ve Kurulum Adımları" bölümü eklendi (ön koşullar, adımlar, tuzaklar). Kaynak: Gradle dosyaları, `settings.gradle.kts`, `ci.yml`, `CLAUDE.md`, `etc/*/python/requirements.txt`. Derleme çalıştırılmadı.

## [2026-10-12] ingest | Figma ekran tasarımı eklendi

`raw/saglik-kiosk/Figma` (canvas.fig, thumbnail, 18 PNG) metin katmanları ve küçük resim üzerinden okundu; `figma-ekran-tasarimi.md` yazıldı, hub ve indekse bağlandı. Ekranlar piksel düzeyinde incelenmedi; PNG'ler tek tek görülmedi.

## [2026-10-13] ingest | Geliştirici rehberi (C# / Qt geçmişi için) eklendi

`gelistirici-rehberi-cpp-qt.md` yazıldı: C#/Qt–Kotlin sözlüğü, bileşen ve akış diyagramları, 75 dosyalık harita, bugünkü "yeni cihaz ekleme" kontrol listesi (≈10 dosya) ve kayıt/telemetri/kurum entegrasyonu için önerilen mimari (DeviceDescriptor/registry, MeasurementRecord + outbox, UploadGateway, TelemetryPort). Bölüm 6–7 öneridir, kaynakta yoktur. Kaynaklar `MainActivity`, `SensorPorts`, `SensorAdapterFactory`, `KnownUsbDevices`, `MeasurementSession`, `DypH01HeightAdapter` ve dosya listesinden okundu; testler çalıştırılmadı.

## [2026-11-18] ingest | Yönetim paneli ve ekran koruyucu (Faz 1)

Kaynak: `Downloads/sağlık-kiosk-admin-panel` (15 fotoğraf, referans kioskun admin menüsü) ve uygulamaya yazılan Faz 1 kodu. `admin-paneli-ve-ekran-koruyucu.md` yazıldı, hub ve indekse bağlandı. Kararlar: ekran koruyucu yalnız ana ekranda, tek sessiz video, varsayılan PIN 888888, Log out şifresi yok. Kod cihazda henüz derlenmedi; Android dosya seçicisinin RK3576'da varlığı doğrulanmadı.

## [2026-11-23] ingest | Yönetim paneli Faz 2 (adım 1–2)

Faz 1 cihazda doğrulandı (COP31 videosu ekran koruyucu). "Portlar" düğmesi ana ekrandan admin'e taşındı; "Ölçüm Modülleri" sekmesi eklendi (`AppSettings.enabledModules`, tur ve hazırlık listesi buna göre). `admin-paneli-ve-ekran-koruyucu.md` güncellendi. Ayrıca `AdminPinDialog.kt`'de `fillMaxWidth`'ı gölgeleyen kalıntı fonksiyon silindi. Yeni kod cihazda henüz derlenmedi.

## [2026-11-23] note | Ziyaretçi ekranlarından teknik dil kaldırıldı; uygulama simgesi; 6 haneli PIN

Hazırlık metinleri ziyaretçi yönergesine çevrildi (CH340/FTDI/Cypress/USB ifadeleri yok); "algılandı/algılanmadı" şeridi, "Portları yenile" düğmesi ve ana ekrandaki port şeridi kaldırıldı. Hata ekranı teknik terim içeren mesajları sade metne çeviriyor, ham mesaj logcat'e (`SaglikKiosk`) düşüyor. PIN tam 6 hane, 6. rakamda otomatik giriş; "C" tuşu kaldırıldı. Uygulama simgesi: kırmızı zemin, beyaz kalp + EKG çizgisi (artı işareti kullanıcı isteğiyle kaldırıldı).

## [2026-11-23] ingest | Faz 2 tamam: Süreler ve Sistem Bilgisi; RK3576 düzeltmesi

Oturum zaman aşımı (hazırlık/uyarı/sonuç 60 sn, cihaz hazırlık 120 sn, admin'den ayarlanır) ve Sistem Bilgisi sekmesi yazıldı. `lztek-anakart.md`'ye sahadaki kutunun RK3576 olduğu notu eklendi (kaynak: kutu fotoğrafları, `rk3576_u` model adı, CNX Software RK3576 özellikleri). Dingheng referans uygulaması (`com.cndingheng.dh`, Cordova, AiCare kütüphanesi, HOME launcher) yedeklendi; ayrıntılı inceleme yapılmadı. Yeni kod cihazda derlenmedi.

## [2026-11-23] note | Geri sayım ve ölçüm üst sınırı

Tüm akış ekranlarında sol altta geri sayım. Ölçüm ekranında modülün üst sınırı gösteriliyor; tansiyon üst sınırı 200 → 120 sn. `admin-paneli-ve-ekran-koruyucu.md` ve `olcum-oturumu.md` güncellendi. Cihazda derlenmedi; tansiyonun 120 sn'ye sığdığı sahada doğrulanmalı.

## [2026-11-23] ingest | Yeni sensör kiti (Dingheng) belgeleri ve e-posta analizi

`documents/` klasöründeki boy, SpO2, kilo docx; HeTaiDa UART PDF; BMH05108 paketi (protokol Rev0.5, datasheet V1.7, kalibrasyon SSS, README); RBP-7000 paketi okundu. Yeni sayfa `yeni-sensor-kiti-analizi.md`: değişiklik tablosu, kod boşlukları, Yöntem 1/2, Çarşamba adımları. Kod değişmedi; BMH LRC örnekleri Python ile doğrulandı.

## [2026-11-23] ingest | SDK, kart şartnamesi ve Dingheng yedeği incelendi

Lztek SDK'da DTR/RTS yok; kart DB9 COM0/COM1 yalnız RX/TX/GND. Dingheng `dingheng.db` ayarlarından seri port alanları ve ölçüm süreleri çıkarıldı. `yeni-sensor-kiti-analizi.md` güncellendi.

## [2026-11-25] kod | Çekirdek: modül kayıt defteri + paylaşımlı seri hat
- Sunum Pzt 30 Kasım'a kaydı; öncelik: boy (ayakta), kilo (oturarak), ateş (sabit, [Ölç] + DTR), SpO2, tansiyon, vücut analizi (BMH05108, şart), rapor, yazıcı. NFC/8x8/cilt sonra.
- Kararlar: yaş/cinsiyet elle giriş; saklama yok (arayüz hazır); rapor yorumları standart eşikler; açılışta bizim uygulama; sensör hatası → yaz ve atla.
- Yeni: `application/module/` (MeasurementModule, MeasurementContext, ModuleRegistry, PortBackedModules, Timeouts), `domain/SubjectProfile`, `domain/AsciiFrameSplitter`, `application/port/AsciiFrameLink`, `adapter/serial/SharedAsciiSerialLink`.
- Değişen: MeasurementSession registry ile çalışır, tur hata kaydedip devam eder; TourModuleOrder'a WEIGHT (boydan sonra); tansiyon zaman aşımı 150 sn; VM `failedModules`, uyarı 8 sn sonra kendiliğinden atlar.
- Derlenmedi; kullanıcı derleyecek.

## [2026-11-25] kod | 2026 kiti: rol keşfi, Dingheng boy/kilo/SpO2, DTR'li ateş ölçer
- Port haritası (rk3576, Android 14): BMH05108 04d9:8008 → ttyACM0; PL2303 067b:23a3 → ttyUSB0; iki CH340 1a86:7523 (tty yok, yalnız UsbManager); Custom TL60 yazıcı 0dd4:01ad; OMNIKEY 5422 kart okuyucu 076b:5422; Newland QR (HID klavye); 2× Sonix 5MP kamera; ILITEK dokunmatik. FTDI yok → Raycome da çeviricilerden birinde.
- Çeviriciler kimlikle ayırt edilemediği için `UsbSerialRoleDiscovery` protokolle rol atar: `#SL0$` → Dingheng, Raycome connect → tansiyon, kalan/DTR'ye `^` dönen → ateş ölçer. Yollar `serial-role:*` jetonu (`SerialDeviceRole`, `MutableSerialRoleDirectory`).
- Yeni modüller: `DinghengHeightModule` (#SRT$ ortam sıcaklığı, 3 yakın okuma → medyan, MT$), `DinghengWeightModule` (#SL0/#SWA1/#SM1, kilit → #SOK$, #SM0$), `DinghengSpo2Module` (#XM0$ yoklama, pleth yok), `HetaidaSerialTemperatureModule` (DTR darbe, ters yön yedeği). `BinarySerialPort.setDtr/setRts`.
- `HardwareKit.INNOVA_2026` MainActivity'de seçili; VM factory artık `ModuleRegistry` alıyor. Log etiketleri: SerialRoles, AsciiLink.

## [2026-11-25] kod | Saha düzeltmeleri, tansiyon ttyS5, BMH05108 ve yaş/cinsiyet ekranı
- Saha logu: boy sensörü ayrı CH340'ta ve komutsuz sürekli `Mxxx.x$` akıtıyor; değer **mesafe**: `Boy = 223,5 − ölçülen − kalibrasyon` (belgedeki "M175.3$ = 175,3 cm" örneği yanıltıcı). Kilo+SpO2 diğer CH340'ta (mail de doğruluyor). Ateş PL2303'te, DTR çalışıyor.
- Roller ayrıldı: HEIGHT_SENSOR / WEIGHT_BOARD / SPO2_BOARD; `SerialLinkPool` aynı cihaza düşen rolleri tek hatta toplar. Kilo kartı komutlara cevap vermezse eleme ile atanır.
- Tansiyon USB değil, kart UART'ı **/dev/ttyS5** (kullanıcı bildirdi); Lztek varsa native, yoksa tty (+root'suz stty).
- Keşif sorunu: VM fabrikası onCreate'te kurulmadan keşif çağrılıyordu → düzeltildi; ölçüm öncesi eksik rol için keşif yeniden çalışır.
- BMH05108: `Bmh05108Protocol` (çerçeve, LRC, B0/B1/D0, 5 paket ayrıştırma; belge örnekleriyle test) ve `Bmh05108BodyCompositionModule` (8 elektrot çift frekans → 20k/100k ham empedans → Body270). Kalp pili/hamilelik uyarısı hazırlık ekranında.
- `VitalModule.BODY_COMPOSITION` (kilodan sonra), `SubjectInfoFrame` (cinsiyet + yaş), `MeasurementContext.subject`.

## [2026-11-25] kod | Kalibrasyon ekranı, boş referans, rapor ekranı; kilo kartı donanım sorunu
- Terminal testi: boy sensörü komut alıyor (MT$ durdurur, MS025$ başlatır). Kilo/SpO2 kartı #SHM$/#SNM$ gönderiyor ama hiçbir komuta (#SL0$, #SM1$, #SRT$, #XL0$) cevap vermiyor → kart RX hattı/çevirici/kablo. Donanım ekibine mesaj gönderildi (çevirici değiştirme, TX→RX, Dingheng'e soru, yük hücresi BMH'ye bağlı mı).
- Boy sensörünün altı boşken ~71,7 cm mesafe okuyor (sabit parça). Ayar: `heightEmptyDistanceCm`, ±3 cm içindeki okumalar kişi sayılmaz.
- Yönetim → Kalibrasyon: boş referans, bilinen boyla kalibrasyon (yeni = eski + ölçülen − gerçek), ±0,5 ince ayar. `heightCalibrationCm` kalıcı.
- CH340'larda açılışta DTR/RTS kapalı bırakılıyor (deneme).
- Rapor: `HealthAssessment` (VKİ WHO, tansiyon ESC/ESH, SpO2, alın ateşi, yağ oranı cihaz standardına göre, iç yağ); kartlarda renkli durum etiketi, VKİ göstergesi sürekli ölçek, vücut analizi ayrıntıları, "Ölçülemeyenler" listesi, bilgilendirme notu.

## [2026-11-25] kod | Rapor 2 (Tanita tarzı), boy hızlandırma, keşif hızlandırma, termal yazıcı
- Yedek: `Saglik-kiosk/yedekler/saglik-kiosk-20261125-1609.zip` (git commit yok).
- Rapor: özet başlık, vital kartlar, aralık çubukları (BMH standart aralıkları; yoksa WHO), puan halkası, bölgesel vücut haritası (paket 2+5), ölçülemeyen/atlanan listesi; rapor en az 3 dk açık, sayaç ölçüm geçişlerinde sıfırlanır. Mock'ta kilo + vücut analizi örnek verisi.
- Boy: 8 okumalık pencere, uçlar atılır, ortalama (sensör ~5 Hz, ±2 cm); zaman aşımı 30 sn. Ölçümler tekrarlanabilir (kullanıcı onayı).
- Keşif ~16 sn → ~4–5 sn: boy sensörü pasif dinlemeyle, PL2303 önce DTR ile.
- Yazıcı: Custom TL60 (0dd4:01ad) USB bulk; fiş Android Canvas ile 384 nokta genişlikte çizilir, `GS v 0` raster + `GS V 0` kesim. `ReceiptContent` (domain), `ReceiptRenderer`, `UsbReceiptPrinter`, `PreviewReceiptPrinter`; son fiş `Android/data/<pkg>/files/last_receipt.png`.

## [2026-11-25] kod | Yazıcı çalıştı (ESC *)
- Custom TL60 `GS v 0` raster komutunu tanımıyor (baytları metin olarak bastı). `ESC * 33` (24 nokta sütun) ile fiş düzgün basıldı; varsayılan bu. Yönetim → Yazıcı: yöntem seçimi + test fişi. `/dev/usb/lp*` varsa önce oraya yazılır, yoksa USB bulk OUT (tüm arayüzler denenir).
- Tansiyon turda çalıştı (107/74, nabız 73). Kalan: kilo/SpO2 (donanım), kiosk modu, giriş ekranı/NFC.

## [2026-11-25] kod | Kesim + kiosk modu
- Kesim: `GS V 0` çalışmadı, Custom `ESC i` ile kendi kesiyor (5 satır besleme).
- Kiosk: `KioskModeController` — tam ekran (çubuklar gizli, kaydırınca geçici), ekran hep açık, geri tuşu devre dışı. Cihaz sahibiyse (adb `dpm set-device-owner …/.adapter.system.KioskDeviceAdminReceiver`) lock task + kalıcı HOME + kilit ekranı/durum çubuğu kapalı. Yönetim → Sistem: "Kilidi kaldır", "Android ayarları", "Cihaz sahipliğini kaldır".
- HOME: `adb shell cmd package set-home-activity tr.com.innova.saglikkiosk/.MainActivity`.

## [2026-11-25] durum | Gün sonu
- Kiosk modu doğrulandı (ayarlardan dönüş çalışıyor; Lztek gezinme çubuğu bakımda geri getiriliyor).
- Çalışan: boy, ateş, tansiyon, rapor, yazıcı (kesimli), kalibrasyon, kiosk modu. Bekleyen: kilo/SpO2 kartı (RX sorunu, donanım ekibiyle 26 Kasım), vücut analizi (kiloya bağlı).
- Kalan: giriş ekranı + OMNIKEY taslağı, ölçüm ekranı rötuşları, kilo yedek planı (BMH yük hücresi), cilt API (karar bekleniyor), 8x8 (sonra), Pazartesi kurulum + cihaz sahibi kilidi.

## [2026-11-25] kod | Kilo, SpO2 çalıştı; vücut analizi donanım bekliyor
- Kilo kartı USB tak-çıkar sonrası komut almaya başladı (gevşek bağlantı). Kart #SWA1$/#SRA$'ı yok sayıp AD değeri yolluyor → yazılım kalibrasyonu: sıfır `#SUP$` (dara), katsayı bilinen kiloyla (`WeightCalibration`, Yönetim → Kalibrasyon → Tartı).
- SpO2: her #XM0$ ölçümü yeniden başlatıyor; 25 Hz yoklama 30–60 sn gecikme yapıyordu → 1 Hz yoklama + 4 okuma ±1 / 3 sn kararlılık (~10 sn).
- BMH05108: iki el / iki ayak / 8 elektrot taraması; hepsinde durum 1 ve empedans 0 → tutamak kabloları modüle bağlı değil (donanım). Tarama modu varsayılan, temas yoksa ~25 sn'de atlar.

## [2026-11-26] kod | Kart okuyucu ve kartla giriş
- OMNIKEY 5422: iki CCID arayüzü (temaslı + temassız), Android'de PC/SC yok → `CcidCardReader` (GetSlotStatus, IccPowerOn/ATR, XfrBlock `FF CA 00 00 00` → UID). TC kimlik kartı okundu (UID + tip).
- Innova ve ODTÜ Teknokent personel kartları okunmadı; telefon NFC de tepki vermedi → kartlar büyük olasılıkla 125 kHz (5422 okumaz). Seçenek: OMNIKEY 5427 CK (125 kHz + 13,56) ya da kurum API'si.
- Newland (HID KBW) klavye girişi de dinleniyor (`onHardwareKey`).
- Kartla giriş: ana ekranda okuyucu dinlenir; tanımlı kart (`SharedPrefsCardDirectory`, Yönetim → Kart okuyucu → Kart tanımla) → "Hoş geldiniz, …" + yaş/cinsiyet ekranı atlanır; tanımsız kart → uyarı. Ana ekran düğmesi "Misafir olarak başla".

## [2026-11-26] donanım | Vücut analizi teşhisi (BMH05108)
- Üretici yazılımıyla (Best Health, PC, COM6, 38400) test: haberleşme sağlam, "4 elektrot iki el" komutu `55 06 B0 03 05 ED` bizim kodla birebir aynı.
- İlk durum: kart üzerindeki LeftArm/RightArm soketleri boştu → sürekli `CheckElectrode`, empedans 0. Kablolar takılınca durum `Success` oldu ama empedans milyonlarca Ω, algoritma "双臂测量异常" (iki kol ölçümü anormal) → tutamak plakaları/kablo eşleşmesi donanım ekibinde (her tutamakta akım+gerilim plakası, aynı tutamağın iki teli aynı Arm soketine). Doğrulama: Arm soketlerine 500 Ω direnç testi.
- JBL USB-C kablosu sorun değil (veri taşıyor).

## [2026-11-26] kod | Tanita tarzı rapor + silüetler + yeni fiş
- Rapor ekranı yeniden: kişi bilgi kutucukları, iki sütun "Detaylar" (sonuç + sağlıklı aralık + durum noktası), **8 kademeli vücut şekli** (kadın/erkek ayrı silüet, VKİ: <18,5 · 18,5–21,9 · 22–24,9 · 25–27,4 · 27,5–29,9 · 30–34,9 · 35–39,9 · ≥40), metabolizma/iç yağ/su bantları, 3x3 **fizik değerlendirme** (yağ × kas, aralıklardan), vital kartlar, otomatik "Değerlendirme" maddeleri.
- Silüetler kendi çizimimiz (`domain/report/BodySilhouette`, Catmull-Rom → Bézier); aynı geometri Compose'ta ve fişte (Android Path). Stok görsel lisans nedeniyle kullanılmadı.
- Fiş: silüet sırası (seçili dolu, diğerleri kontur + ok + grup çizgileri), ideal kilo, fizik tablosu (seçili hücre ters renk), değerlendirme maddeleri.
- Yeni: `BmiScale`, `Physique`, `ReportNotes`, `ReportSections.kt`; testler `ReportVisualsTest`. Tasarım: Claude Design tuvali "Rapor – Vücut Tipi Tasarımı".
- İleride: yağ dağılımı (bölgesel) — 8 elektrot/bölgesel veri gelince.

## [2026-11-26] kod | Yönetim → Ses & Kamera
- Ahize/mikrofon testi için yeni bölüm (`AudioCameraSection`, `adapter/system/AudioDiagnostics`): giriş aygıtı (ahize = USB kulaklık/ses aygıtı ya da 3,5 mm kulaklık girişi) ve kayıt türü (MIC, görüşme/yankı engelleme, …) seçimi, ses çıkışı seçimi, canlı dinleme (mikrofon → çıkış), 5 sn kayıt + çalma, 1 kHz test sesi, seviye göstergesi, medya/görüşme ses düzeyi.
- Kamera: CameraX 1.3.4 ile kamera listesi (ön/arka/harici USB) ve önizleme.
- Seçimler saklanır (`kiosk_audio`, `kiosk_camera` tercihleri); 8x8 görüntülü görüşme bunları kullanacak. İzinler: RECORD_AUDIO, CAMERA (cihaz sahibiysek diyalogsuz verilir).

## [2026-11-26] test | Rapor, fiş, ses/kamera cihazda doğrulandı
- Yeni Tanita tarzı rapor ve fiş kioskta çalıştı; kartla giriş ismiyle açıldı. Ahize (Ses & Kamera) ve kameralar bulundu.
- Not: kiosk saati yanlış (26.08.2026 gösteriyor) → Android ayarlarından tarih/saat düzeltilmeli.
- Yedek: `yedekler/saglik-kiosk-20261126-rapor-ses-kamera.zip`.

## [2026-11-26] kod | Sesli yönlendirme + oturarak akış metinleri
- Akış: yalnız boy ayakta; kilo → vücut analizi (tutamaklar) → ateş (oturunca alın ateş ölçer hizasında) → SpO2 → tansiyon taburede oturarak. Ölçüm sırası zaten böyleydi; ekran metinleri buna göre güncellendi (platform/ayak elektrodu ifadeleri kaldırıldı).
- Sesli yönlendirme: Android TTS (Türkçe) — `AndroidTtsVoiceGuide` (tek örnek, "kiosk_voice" tercihleri), `VoiceGuidance` durum akışını izler (hazırlık yönergesi, "ölçülüyor", sonuç, uyarı, rapor, fiş). Cümleler `domain/voice/VoicePrompts`. Yönetim → Ses & Kamera: aç/kapa, hız, dene, Türkçe ses paketi kur, TTS ayarları.
- Yönlendirme görselleri: Figma tarzı 3D görseller görsel üretme aracıyla hazırlanacak (komutlar verildi); kiosk fotoğrafları `Saglik-kiosk/cihaz/`.
- Kioskta hiç TTS motoru yok (`pm list packages` boş, `tts_default_synth` null). Yedek: kayıtlı sesler — `tools/generate_voice.py` (edge-tts, tr-TR-EmelNeural) `tools/voice_lines.json`'dan `app/src/main/assets/voice/*.mp3` üretir; TTS yoksa `AndroidTtsVoiceGuide` bunları çalar (sonuç cümleleri sayısız: "Boy ölçümünüz tamamlandı.").

## [2026-11-26] kod | Vücut analizi: empedans ölçüldü, algoritma cevap vermedi
- Log13: iki el modunda temas ve empedans tamam (991 Ω, durum 3). `0xD2` (4 elektrot algoritması) isteği belgeye birebir uygun ama kart hiç cevap vermedi (3 deneme).
- Yapılan: kart yazılım sürümü (0xE0) loglanıyor; D2 cevapsızsa empedans modu kapatılıp yeniden deneniyor; yine cevap yoksa `EstimatedBodyComposition` (Deurenberg VKİ denklemi + el-el empedans düzeltmesi, FFM'den su/kemik/kas, Katch–McArdle BMR) — rapor ve fişte "tahmini" yazar.
- Önceki atlamanın nedeni: turda boy ölçülmemişti ("önce boy ölçülmeli"); artık boy/kilo yoksa adım açılmadan atlanıp nedeni rapora yazılıyor.

## [2026-11-26] kod | Cilt analizi (DermaAI) entegrasyonu
- Kaynak: `documents/Derma/DermaAI_Kiosk_Entegrasyon_Kilavuzu_v1.0` (Teus). Kök `https://arsiviq.com.tr/dermaai-v5/`, `Authorization: Bearer <cihaz anahtarı>`.
- Akış rapor ekranından isteğe bağlı ("Cilt analizi · Başla"): rıza (`onay`, sürüm `innova-kiosk-aydinlatma-v1`, metin taslak — hukuk onayı gerek) → alt panel kamerasıyla 1–6 fotoğraf (uzun kenar 1800, `kotu` kalite otomatik silinir) → `tara` → yaş/cinsiyet (ölçümden), bölge seçimi (tarama ön seçili) → ahizeden dikte (16 kHz WAV, `ses`) + metin onayı → `baslat` → sorular (`cevap`, serbest cevap, `geri`, `asama` ile bekleme yazısı) → sonuç (seviye rengi, ön tanı öne, diğerleri kapalı, `uyari` her zaman) → `kapat` (vazgeç/zaman aşımında da).
- Hata/toparlanma kılavuz 5.4: tara/ses/baslat 3 deneme; cevap hatasında `GET vaka` → gerekirse `geri` + yeniden gönder. Girdiler göndermeden doğrulanır (adres engeli 5.2).
- Kod: `domain/derma` (modeller, bölgeler, doğrulama), `application/port/DermaService`, `adapter/derma` (HttpURLConnection + org.json, `DermaSettings` — anahtar gizli), `ui/skin` (controller, ekranlar, CameraX çekim). Yönetim → Cilt analizi: aç/kapa, anahtar değiştir/sına, örnek arayüz (WebView).
- Yedek: `yedekler/saglik-kiosk-20261126-cilt-analizi.zip`.

## [2026-11-26] update | Cilt analizi: yazıcı, kamera döndürme, otomatik çekim, sesli yönlendirme
- Kamera döndürme artık kamera başına (`CameraPrefs.rotation_<id>`), Admin › Ses & Kamera'dan 0/90/180/270 seçilir.
- `SkinCamera`: ImageAnalysis ile hareket ölçümü (32×24 Y ızgarası); görüntü ~3,5 sn sabit kalınca 3-2-1 bip + `MediaActionSound` deklanşör ile kendiliğinden çeker. Aynı sahne tekrar çekilmez (sahne değişmeli). Analiz bağlanamazsa elle çekim.
- Fotoğraf adımında üstte yazılı ipucu (göster / sabit tut / çekildi / başka açı) + sesli yönlendirme (DERMA_* klipleri; "sabit tutun" 6 sn'de bir).
- Adım bazlı sesli yönlendirme (rıza, foto, tarama, bilgi, soru, sonuç); döküm sonrası "metni kontrol edin", yeniden çekim uyarısı.
- Sonuç fişi: `DermaReceipt` → `KioskViewModel.printDermaResult` (ortak `printContent`), sonuç ekranında «Fişi yazdır». Ad/TCKN fişe yazılmaz.
- `ReceiptRenderer.labelValue`: değer boşsa etiket satırlara kaydırılır.
- `tools/voice_lines.json` 34 klibe çıktı (10 derma). Dinamik soru metni için TTS motoru gerekir (Google Speech Services önerildi); yoksa "Lütfen ekrandaki soruyu cevaplayın." klibi.
- Yedek: `yedekler/saglik-kiosk-20261126-derma-yazici-otocekim.zip`.

## [2026-11-26] decision | Cihaz TTS motoru ertelendi
- Google Speech Services (20261109) kuruldu, Türkçe ses inmedi. Log (log15): "requires the Google Play Store", Phenotype.API yok, cihazda GMS yok.
- SherpaTTS (Piper) denendi, kalite Emel kliplerinin çok altında kaldı, reddedildi.
- Karar: demo hazır kliplerle yapılacak. Derma soruları "Lütfen ekrandaki soruyu cevaplayın." klibiyle karşılanıyor.
- Sonraki adım (demodan sonra): Azure Speech (tr-TR-EmelNeural) ile dinamik metinler, anahtar admin panelinde maskeli tutulacak.

## [2026-11-26] update | Cilt analizinde ses kaldırıldı
- Kullanıcı: derma sesi kötü ve uzun. SkinAnalysisFrame'de tüm konuşma kaldırıldı (girişte `voice.stop()`); yazılı kamera ipucu, 3-2-1 bip ve deklanşör sesi duruyor.
- DERMA_* klipleri `clipCatalog`'dan ve `voice_lines.json`'dan çıkarıldı (24 klip).

## [2026-11-26] update | Ölçüm ekranları + ölçüm sonrası sonuç ekranları (Figma)
- Figma `.fig` → `New_figma/_work/doc.json.gz` kareleri HTML'e çevrilip ekran görüntüsü alındı (scratchpad `fr/render.py`); ilgili kareler: 2736 (vücut ölçüm %), 2734/2747 (vücut sonuç), 2753/2756 (tansiyon), 2757 (SpO2), 2762 (ateş), 2726 (boy-kilo).
- Figma'da 3D yönlendirme görselleri zaten var (`_work/images`); yeni: `fig_body_left.webp` (tutamaklı kiosk), `fig_fat_ref.webp` (yağ oranı silindirleri).
- `MeasurementTiming`: modül başına beklenen süre (boy 6 / kilo 8 / vücut 25 / ateş 6 / SpO2 25 / tansiyon 45 sn); %90'a kadar doğrusal, sonra %95'te durur; sonuç gelince 0,8 sn %100 gösterilir.
- `MeasuringFrame` yeniden: 3D görsel + Ölçüm Adımları + yüzde halkası + kalan süre; tansiyonda canlı manşet basıncı (`liveCuffMmHg`, "Anlık basınç: N mmHg" ayrıştırılır), SpO2 dalga, boy/kilo canlı değer.
- Yeni `KioskScreen.ModuleResult` + `ModuleResultFrame`: boy-kilo (VKİ + 8 silüet), vücut analizi (3 ana değer, `BodyFatScale` ile Figma silindirinde "Siz" çerçevesi, Genel Vücut Analizi 8 kutu), tansiyon (sınıf basamakları), SpO2 (yarım daire), ateş (renkli kadran). «İleri (N)» ya da 10 sn sonra tur sürer; boydan sonra kilo geliyorsa boy ekranı atlanır.
- Test: `MeasurementTimingTest`. Yedek: `yedekler/saglik-kiosk-20261126-olcum-sonuc-ekranlari.zip`.

## [2026-11-26] fix | Sonuç raporu tek ekrana sığdırıldı, boy eklendi
- Detaylar tablosunda Boy artık her zaman var (önceden vücut analizi varsa gizleniyordu).
- Tek ekran için: "Ölçüm özeti" başlığı kaldırıldı, bölüm aralığı 22→14, silüet 193→120, fizik hücresi 92→66, tablo satırı 12→9 dolgu; cilt analizi kartı yerine alt sırada üçüncü düğme («Fişi yazdır · Cilt analizi · Yeni ölçüm»; yazıcı yoksa kart kalır).
- Yine sığmazsa (ör. ölçülemeyenler kartı) altta "▼ Devamı için aşağı kaydırın" solması.

## [2026-11-26] fix | Fizik tablosu tutarlılığı + rapor sıkıştırma (2. tur)
- `Physique`: ±2 puan yağ toleransı kaldırıldı; sınır Detaylar tablosundaki sağlıklı aralıkla aynı (tablo sarı/yüksek derken tablo "Standart" diyordu). Test güncellendi.
- Not: iç yağ (viseral seviye, organ çevresi) ile yağ oranı (toplam yağ/kilo) ayrı ölçütler. Elle ölçümde yağ oranı tahmini (Deurenberg: VKİ+yaş+cinsiyet + küçük empedans düzeltmesi), bu yüzden VKİ'yi izler.
- Sıkıştırma: bölüm başlığı dolgusu 10→6, BandMeter açıklamaları (iç yağ hariç) ve fizik notu kaldırıldı, fizik hücresi 60, vital kutular küçüldü, tablo satırı 7; kaydırma ipucu koyu hap.

## [2026-11-26] finding | Diğer kiosk vücut analizini tartı kartında (#FM) yapıyor
- BMH05108 sürümü: Master V5.1, Bia V2.1 (`55 05 E0 00/01`). 0xD2 (iki el) çerçevesi belgeyle birebir, kart yanıt vermiyor.
- Diğer kioskun APK'sı (`APK/土耳其.apk`, Dingheng Cordova uygulaması) incelendi:
  - 4 elektrot "iki el" modu (`/fat/index`, ayarda fatSerial yoksa) **Dingheng tartı/ana kartına** ASCII komut gönderiyor: `#FM` + boy×10 (%04d) + kilo×10 (%04d) + cinsiyet (1 erkek, 0 kadın) + yaş (%02d) + `$`. Örn. 170,0 cm 70,0 kg erkek 31 → `#FM17000700131$`. Tutamaklar tartı kartının yağ ölçer girişine bağlı ("脂肪计把手").
  - Yanıt `#FM?...$`: 4. karakter `C` başarı / `E` hata (5. karakter hata kodu). Başarıda 5. karakter tip ('1' = tam set). Alanlar (dize indeksleri): [5:8] yağ oranı/10, [8:12] yağ kütlesi/10, [12:16] BMR; tip '1' ise [16:19] su %/10, [19:21] su puanı, [21:24] iskelet kası/10, [24:26] puan, [26:29] viseral/10, [29:31] puan, [31:34] kemik/10, [34:36] puan, [36:39] hücre dışı sıvı/10, [39:42] hücre içi/10, [42:45] toplam su/10, [45:48] protein/10, [48:51] mineral/10, [51:53] vücut yaşı, [53:56] genel puan/10, varsa [56:60] kas/10.
  - 8 elektrot modu (fatSerial tanımlıysa) BMH'ye `551ED0…` (D0), iki el BMH yolu `550CD2…` gönderiyor; kullanılan yol iki el için tartı kartı.
  - Bluetooth terazi için ayrıca Aicare algoritma kütüphanesi var (bizi ilgilendirmiyor).
- Sonuç: bizim kioskta tutamaklar BMH'ye taşınmıştı. Tutamaklar tartı kartının yağ girişine geri bağlanır ve `#FM` gönderilirse kartın kendi hesabı gelir.
- Ayrı sayfa: [vucut-analizi-tarti-karti-fm.md](vucut-analizi-tarti-karti-fm.md)
- [2026-11-26] Kablo fotoğrafları: diğer kioskta BMH UART→Dingheng kartı, USB-C boş, ayak elektrotları takılı; ayrıntı vucut-analizi-tarti-karti-fm.md
- [2026-11-26] DÜZELTME: Çin kioskunda da BMH USB-C ile bağlı; büyük ihtimalle D2'yi doğrudan BMH'ye gönderiyor. Sonraki adım Çin kioskundan logcat.
- [2026-11-26] Çin kioskunun logu: D2 isteği bizimkiyle bayt bayt aynı, cevap 86 bayt geliyor ve çözücümüz doğru çözüyor. Fark: bizim denemeler kadın (00). Yarın Erkek ile dene.
- [2026-11-26] Çin kioskunda kadın da çalıştı → cinsiyet değil. Sırada: bizim kartı PC'de elle dene / kart takası.

## [2026-11-27] update | Oturmalı kiosk görselleri (New_figma/sitting-kiosk-png)
- Yeni 3D görseller (Çin'den gelen oturmalı kioska uygun) beyaz kenar kırpılıp 1400 px yüksekliğe indirildi, webp: `fig_height_left` (boy, ayakta), `fig_weight_left` (kilo, oturarak — yeni), `fig_body_left` (tutamaklar), `fig_temp_left` (ateş alma), `fig_spo2_left` (parmak — yeni), `fig_bp_left` (tansiyon), `fig_warning` (hata — yeni), `fig_home_left` (Sağlık Kiosku Görsel 1).
- Eşleme: ModulePrep/Measuring yan panelleri modüle göre; kilo sonuç ekranı `fig_weight_left`; uyarı ekranında simge yerine `fig_warning`; `IllustrationPanel` zemini beyaz.
- Kullanılmayanlar: `AtesOlcer.png` (oturan kişi ekrana bakıyor), `Sağlık Kiosku Görsel 2.png` (yatay ürün görseli). Yakın çekim ipuçları (`fig_bp_arm`, `fig_temp_main`, `fig_spo2_finger`) eski hâliyle duruyor.

## [2026-11-27] update | Görseller cinsiyete göre (kadın/erkek)
- Kadın sürümleri `_f` ekiyle: `fig_height_left_f` (boyKadin), `fig_weight_left_f` (kiloKadin), `fig_body_left_f` (analizKadin), `fig_temp_left_f` (atesOlcerKadin), `fig_spo2_left_f` (spo2Kadin), `fig_bp_left_f` (tansiyonKadin); yakın çekimler `fig_temp_main_f` (AtesAlmaKadinGuide), `fig_bp_arm_f` (tansiyonKadinOlcum).
- Erkek yakın çekimler yenilendi: `fig_temp_main` (atesAlmaGuide), `fig_bp_arm` (tansiyonErkekOlcum), `fig_spo2_finger` (SpO2.png, tek sürüm — yalnız el). Yakın çekim oranı 3:2.
- `moduleSideImage(module, female)` / `moduleHero(module, female)`; ModulePrep, Measuring ve ModuleResult (kilo, ateş) `subject.gender`'a göre seçiyor. Kişi bilgisi yoksa erkek sürümü. `fig_warning` (hata.png, kadın) ve ana ekran (kişisiz) tek sürüm.

## [2026-11-27] fix | Cilt kamerası otomatik çekim toleransı
- Kullanıcı: "kaskatı durmayı istiyor". `MotionMeter`: hücre başına 3x3 nokta ortalaması (gürültü), fark için üstel ortalama (0,35), başlatma eşiği 4→9, sayım sırasında gevşek eşik 15 (histerezis), sayımda 2 tik (~400 ms) sarsıntı toleransı, bekleme 3,5→3 sn.
- Hâlâ zorsa ilk ayarlanacak değerler `STABLE_DIFF` ve `HOLD_DIFF` (SkinCamera.kt).
