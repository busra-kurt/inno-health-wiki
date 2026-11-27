---
type: analysis
title: Yeni Sensör Kiti (Dingheng) — Eksik Analizi ve Eylem Planı
description: 23 Kasım 2026 e-postası ve ekli belgelere göre sensör değişiklikleri, mevcut kodla farkları, Yöntem 1/2 karşılaştırması ve Çarşamba sunumuna kadar adımlar.
tags: [analysis, sensors, dingheng, bmh05108, plan, demo]
source: raw/saglik-kiosk/documents (height/SPO2/weight docx, HeTaiDa UART pdf, BMH05108 paketi, RBP-7000 paketi), e-posta 2026-11-23
generated: 2026-11-23
---

# Yeni Sensör Kiti — Eksik Analizi ve Eylem Planı

⬅ [Genel Mimari (Hub)](hub.md)

Hedef: Ankara ofisinde **Çarşamba 25 Kasım 2026 sabahı** İnnova kiosk sunumu. Aynı Android PC (RK3576), ama çevre birimleri önceki kitten farklı.

## Değişen / değişmeyen

| Modül | Eski (kodda) | Yeni kit | Protokol (belge) | Koddaki durum |
|-------|--------------|----------|------------------|----------------|
| Tansiyon | Raycome RBP-7000, FTDI 0403:6001, 9600, ikili `CC/AA 80 03` | **Aynı** | Aynı V1.3 | Hazır. `android-log` FT232R→ttyUSB0 doğruluyor |
| Boy | DYP-H01 (CH340, ikili, 9600) | Dingheng CQ-UL-DMUT, USB-seri | ASCII 4800: `MSxxx$` başlat, `Mxxx.x$` değer, `MT$` durdur, anormal `M0000$` | **Çözücü var** (`HeightParser`, `VitalCommands.heightStart`); adaptör/port yok |
| SpO2 | BerryMed YS2000A (Cypress, BCI 5 bayt akış) | Dingheng modülü | ASCII 4800: `#XL0$`→`#XL1$`, `#XM0$`→`#XMC095$` (C=var/E=hata, 0 anlık/1 kilitli) | **Çözücü var** (`Spo2Parser`); adaptör yok. **Nabız ve pleth dalgası yok** |
| Kilo | Yok | Dingheng, SpO2 ile **aynı port** | ASCII 4800: `#SL0$`, `#SWA1$` (kg modu), `#SM1$`, `#SMW1xxxxx$` kilitli (her sn tekrar, `#SOK$` ile durur), `#SP$`/`#SUP$` dara, `#SRT$` sıcaklık, kalibrasyon `#SVW…$`/`#SVS$` | **Çözücü + komutlar var**; adaptör, port, tur adımı yok |
| Ateş | HeTaiDa HTD8808C **USB HID** | Aynı çekirdek, **seri 9600 8N1**, kiosk içinde sabit; tetik **DTR darbesi** (LOW-HIGH-LOW ~100 ms) | `^ [bellek 2B] [durum] [veri] 0D`; durum `0x30` vücut, `0x31` yüzey, `0x32/33` kayıtlı | Paket çözücü hazır (`HetaidaPacketParser`), ama **seri adaptör ve DTR yok**; çözücü durum baytını süzmüyor |
| BMI/Vücut analizi | Yok | BestHealth **BMH05108** (3. parti), 38400, USB CDC/UART | İkili `55/AA`, LRC; ağırlık+empedans+`0xD0` yerleşik algoritma | Hiçbir şey yok (yalnız BMI = kilo/boy hesabı hazır) |

## Kodla karşılaştırma: bulunan boşluklar

1. **Rol haritası VID/PID tabanlı** (`UsbSensorRoleMap`: CH340=boy, Cypress=SpO2, FTDI=tansiyon). Yeni kitte boy, SpO2/kilo ve muhtemelen ateş USB-seri çeviriciler; **aynı çip birden çok cihazda olabilir** → çakışır. Çözüm: el sıkışmayla keşif (ASCII sondalar `VitalModuleProbes` zaten var) + elle port ataması.
2. **Keşif bir port = bir modül varsayar** (`claimedSerialPorts`). SpO2 ve kilo **aynı portta**; paylaşımlı port desteklenmeli. `HardwareDiscoveryService.probeSerial` yalnız DYP/BerryMed/Raycome türlerini biliyor.
3. **`BinarySerialPort` DTR/RTS kontrolü sunmuyor**; `UsbSerialBinaryPortFactory` açılışta DTR/RTS'i 0→1 yapıyor (Raycome için gerekli). Ateşte bu açılış **istemeden tetik** üretir. Port başına `setDtr` ve "açılışta dokunma" seçeneği gerekir. Pin yalnız USB-seri çeviricide var; kart UART'ında olmayabilir (bağlantı şekli belirsiz).
4. **`MeasurementSession.measure(WEIGHT)` hata atıyor**, `TourModuleOrder` kiloyu içermiyor, `SensorAdapterSuite` kilo portu taşımıyor, admin modül anahtarları ve zaman aşımı tablosu kilo için hazır değil (`timeoutMsFor(WEIGHT)` boy süresini kullanıyor).
5. **SpO2 arayüzü nabız/pleth bekliyor** (`LiveSpo2(spo2, pulse, pleth)`); yeni modül yalnız yüzde veriyor. Canlı grafik ve nabız boş kalır (nabız tansiyondan gelebilir). Dingheng'den daha zengin protokol sorulmalı.
6. **BMH05108**: ayrı sürücü, giriş ekranı (yaş, cinsiyet), el elektrotu yönergesi, ~10 sn geri sayım, sonuç ekranı (çok sayıda metrik), HID/seri port keşfi (CDC, VID/PID belgede yok). Ağırlık birimi **jin ×10** (kg = değer / 20).
7. **Boy hesabı**: belge `Boy = 223,5 − ölçülen − kalibrasyon` diyor; modül doğrudan boy mu yoksa mesafe mi döndürüyor sahada netleşmeli (kodda sabit `REFERENCE_HEIGHT_CM = 223.5` yalnız DYP için).
8. **Ortam sıcaklığı**: boy başlatma komutu sıcaklık ister; şu an sabit 25 °C. Kilo modülünden `#SRT$` ile gerçek sıcaklık okunabilir.
9. **Kalibrasyon** (boy, kilo) için yönetim paneli ekranı yok.
10. Yeni kit USB günlüğü (VID/PID) elimizde yalnız tansiyon için var; boy/SpO2-kilo/ateş için **yok**.

## BMH05108 not defteri

- Çerçeve: `55 len cmd data… LRC` / yanıt `AA …`; LRC = `~(toplam)+1`. Belgedeki örnek çerçevelerin (`55 05 A0 01 05`, `55 05 A1 00 05`, `55 06 B0 04 00 F1`, `55 05 F0 00 B6`, `55 0C D1 …`) LRC'si doğrulandı; birim testte kullanılabilir.
- Akış: `A0 01` normal tartı → `A1` ile 100 ms'de bir durum → kilo sabit → ellere tutunma + 10 sn → `B0 04` (8 elektrot çift frekans) → `B1` ile durum/empedans → `D0` (cinsiyet, ürün 0, boy, yaş, kilo×10, 10 empedans) → 5 paketlik yanıt.
- Girdi gereksinimleri: **cinsiyet, yaş, boy**. Uygulamada yaş/cinsiyet girişi yok.
- Şifreli empedans yalnız BestHealth sunucu/APP algoritması içindir; yerleşik `0xD0` için gerekmez.
- Lisans: belge paketi yalnız değerlendirme amaçlı; tersine mühendislik/yeniden dağıtım yasak. Ticari kullanım öncesi test ve belgelendirme müşteri sorumluluğunda.

## Yöntem 1 ve Yöntem 2

| | Yöntem 1: Dingheng uygulaması | Yöntem 2: Kendi uygulamamız |
|---|---|---|
| Ateş | İçeridekini kullanamaz → harici HeTaiDa gerekir | Dahili ateş DTR ile (yazılacak) |
| Tansiyon | Raycome ile uyum belirsiz | Hazır |
| UI | Dingheng/İnnova arayüzü | Bizim tasarım, admin, ekran koruyucu |
| Risk | Yapılandırma bilinmiyor; HOME başlatıcı çakışması | Geliştirme ve saha testi süresi |

Öneri: Yöntem 2 ana yol, Yöntem 1 yedek (Dingheng APK yedeği ve yeni sürüm elde). Çarşamba için en az risk: dört basit sensör + BMI; BMH05108 ayrı hedef.

## Adımlar (öncelik sırası)

**A. Bilgi toplama (bugün, 1–2 saat):** yeni kitin USB günlüğü (VID/PID, ttyUSB sırası), hangi modülün hangi USB-seri çeviricide olduğu, ateşin bağlantı şekli (USB-seri mi kart UART'ı mı), BMH05108 USB mi UART mi, tek platform mu iki platform mu (Dingheng kilo + BMH05108 kilo).

**B. Çarşamba için çekirdek (kod):**
1. `BinarySerialPort` + `setDtr/setRts`; fabrikada "açılışta DTR/RTS'e dokunma" seçeneği.
2. `SerialHetaidaTemperatureAdapter`: DTR darbesi, `^…\r` paketi, yalnız durum `0x30`.
3. ASCII adaptörler: `AsciiHeightAdapter` (`MS…$`, kararlılık, `MT$`), `AsciiSpo2Adapter` (`#XM0$`, kilit 1), `AsciiWeightAdapter` (`#SL0$`→`#SWA1$`→`#SM1$`→kilitli→`#SOK$`).
4. Keşif: ASCII el sıkışma sondaları, paylaşımlı port, **elle port ataması** (admin → Donanım Portları, kalıcı).
5. Tur: kiloyu ekle (boy → kilo → ateş → SpO2 → tansiyon), `SensorAdapterSuite` + modül anahtarları + zaman aşımı (kilo ~20 sn), BMI sonuç ekranı (hazır).
6. SpO2 arayüzünü nabız/pleth yokken uyumlu yap.
7. Mock paketi ve birim testler (belge örnekleri).

**C. Hedef (ikinci tur):** BMH05108 sürücüsü + yaş/cinsiyet girişi + vücut analizi sonuç ekranı; boy/kilo kalibrasyon ekranı.

**D. Yedek:** Yöntem 1 için Dingheng uygulamasını ikinci cihazda/ayrı başlatıcıyla hazır tut; hangisinin varsayılan HOME olacağına karar ver.

## Ek inceleme: SDK, kart şartnamesi, Dingheng yedeği (2026-11-23)

**Lztek SDK (Programming/Testing Guide)**
- `openSerialPort(path, baud)` yalnız yol ve baud alır; veri `InputStream/OutputStream`. **DTR/RTS API'si yok.** GPIO API'si var (`gpioEnable`, `setGpioValue`) ama **root ister** ve pin numaraları karta göre değişir.
- `/dev/ttySn` = COMn. Sessiz kurulum, `suExec`, açılışta uygulama başlatma (BOOT_COMPLETED) SDK'da var; 20211022 sonrası seri port için root gerekmez.

**Kart şartnamesi (KEC-3566 / K-3566)**
- Kutu arkasındaki DB9 **COM0 = ttyS0, COM1 = ttyS8** (RS232). Yalnız **pin 2 RX, 3 TX, 5 GND** bağlı; **DTR (pin 4) yok**. Opsiyonel RS485 ttyS3/ttyS4.
- Sonuç: **DTR ile ateş tetiklemesi kartın DB9'unda yapılamaz.** Tetik USB-seri çeviricide (FTDI/CH340 gibi, DTR pinli) ya da GPIO ile olmalı. Not: şartname RK3566 içindir; sahadaki kutu RK3576 ise pin düzeni ayrıca doğrulanmalı.

**Dingheng yedeği (`dingheng.db`, kendi cihazımızdan alınmış)**
- Uygulamanın seri port ayarları: `heightSerial`, `weightSerial`, **`heightAndWeightSerial`** (birleşik kart), `fatSerial`, `bloodPressureSerial`, `printSerial`, `t232`. **SpO2 için ayrı port alanı yok** → SpO2 büyük olasılıkla boy/kilo kartının portunda.
- Örnek kayıt: `heightSerial=ttySMT6, weightSerial=ttySMT9, fatSerial=ttyACM0 (BMH05108, USB CDC), t232=ttyUSB10 (ateş, USB-seri), bloodPressureSerial=boş`.
- Ölçüm süreleri (sn): boy-kilo 50, ateş 40, SpO2 40, **tansiyon 150**, vücut analizi 30. Bizde tansiyon 120 sn; Dingheng 150 kullanıyor.
- Kullanıcı başına **yaş ve cinsiyet** tutuyor (BMH05108 girdisi); 20/50/100 kHz empedansları kaydediyor.
- Dingheng ayarlarında varsayılan parola 888888.

**Okunamayanlar:** BMH05108 Çince PDF'leri (Çince yazı tipi yok; İngilizce sürümler aynı kapsamda), `.msi` değerlendirme aracı, `.dxf`, RoHS belgesi. `RBP-7000 Series.pdf` kullanım kılavuzudur: seri/USB ile veri aktarımı var, protokol yok.

## Açık sorular (belgeler ve kutu fotoğraflarıyla güncellendi)

- **Cevaplandı:** Boy modülü doğrudan boy döndürür (belge örneği `M175.3$` = 175,3 cm); `223,5 − ölçülen − kalibrasyon` modülün iç hesabıdır. Sahada kalibrasyon değeri yine gerekir.
- **Cevaplandı:** SpO2 protokolünde nabız yoktur (yalnız yüzde). Daha zengin protokol Dingheng'e sorulabilir.
- **Cevaplandı:** DTR kartın DB9'unda yok, Lztek SDK'da da yok → ateş **USB-seri çeviriciden** (DTR'li) tetiklenecek; Dingheng yedeği de `t232=ttyUSB10` diyor. Hangi çip (VID/PID) olduğu çarşamba günlüğünden öğrenilecek.
- **Kısmen:** Boy ve kilo aynı karttan (kullanıcı; Dingheng `heightAndWeightSerial` ile uyumlu). BMH05108 ayrı (CDC) ve kendi tartısını da taşır; ağırlık kaynağı kararı donanımla verilecek.
- **Açık:** Yeni kitin USB günlüğü (VID/PID).
- **Karar:** Sunumda vücut analizi şart mı?

## İlgili

- [OEM ASCII Protokolü](oem-ascii-protokolu.md) · [Donanım Keşfi](donanim-kesfi-ve-usb.md) · [HeTaiDa Ateş](hetaida-htd8808c-ates.md) · [Faz 2](faz2-yazici-vucut-analizi-yukleme.md) · [Ölçüm Oturumu](olcum-oturumu.md)
