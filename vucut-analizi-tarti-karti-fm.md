---
type: device
title: Vücut Analizi — Tartı Kartı #FM Yolu ve BMH05108 Durumu
description: Vücut analizinin bugün nasıl hesaplandığı (tahmini), BMH05108'in 0xD2'ye yanıt vermemesi ve diğer kioskun kullandığı Dingheng tartı kartı #FM komutunun tam biçimi; yarınki yapılacaklar.
tags: [device, body-composition, bmh05108, dingheng, ascii, todo]
source: APK/土耳其.apk (Dingheng Cordova uygulaması), log16.txt, docs/vendor BMH05108 Rev V1.7
generated: 2026-11-26
---

# Vücut Analizi — Tartı Kartı `#FM` Yolu ve BMH05108 Durumu

⬅ [Genel Mimari (Hub)](hub.md) · İlgili: [OEM ASCII Protokolü](oem-ascii-protokolu.md), [Yeni Sensör Kiti](yeni-sensor-kiti-analizi.md)

## Bugünkü durum (26.11.2026)

- Tutamaklar **BMH05108**'in LeftArm/RightArm girişlerine bağlı. Mod `55 06 B0 03 05 ED` (iki el, 50 kHz), ham empedans `55 05 B1 51 A4` ile **geliyor** (ör. 991–1065 Ω).
- Kartın algoritma komutu `0xD2` (`55 0C D2 …`) belgeyle birebir gönderiliyor, **yanıt yok** (3 deneme + mod kapatıp yeniden deneme).
- Kart sürümü: **Master V5.1, Bia V2.1** (`55 05 E0 00 C6` → `AA 07 E0 00 05 01 69`, `55 05 E0 01 C5` → `AA 07 E0 01 02 01 6B`).
- Bu yüzden `EstimatedBodyComposition` devrede: **yağ oranı = Deurenberg (VKİ + yaş + cinsiyet)**, ölçülen empedans yalnız **en fazla ±4 puan** düzeltiyor. Su (0,73×yağsız kütle), kemik (0,042×yağsız), BMR (370+21,6×yağsız), kas aralıkları **hep bu yağ oranından türetiliyor**. İç yağ seviyesi ve metabolik yaş da tahmini. Raporda "tahmini" notu var.

## Diğer kiosk nasıl yapıyor (APK incelemesi)

`APK/土耳其.apk` = Dingheng'in Cordova uygulaması (`com.cndingheng.dh`).

- "İki el" (4 elektrot) modu (`/fat/index`; ayarda `fatSerial` tanımlı değilse) BMH'yi **kullanmıyor**. `FatController.startBodyFat` → **tartı seri portuna** (SerialKeyEnum.WEIGHT, kilo için zaten konuştuğumuz Dingheng kart) ASCII komut:

```
#FM + boy×10 (%04d) + kilo×10 (%04d) + cinsiyet (1 erkek / 0 kadın) + yaş (%02d) + $
örnek: 170,0 cm · 70,0 kg · erkek · 31  →  #FM17000700131$
```

- Talimat metni: "yağ ölçer tutamaklarını iki elinizle sıkıca tutun, kollar düz" → tutamaklar orada **tartı kartının yağ ölçer girişine** bağlı.
- 8 elektrot modu (`fatSerial` tanımlıysa) BMH'ye `551ED0…`; BMH iki el yolu `550CD2…` de kodda var ama iki el için kullanılan yol tartı kartı.
- Bluetooth terazi için Aicare algoritma kütüphanesi (`libalgorithm-lib.so`) da var; bizi ilgilendirmiyor.

### Yanıt biçimi (`FatService.manager` / `getFatLumpModel`)

Kart `#FM…$` ile döner (`WeightManager` `#FM` ile başlayanı yağ servisine yollar).

| Konum (dize indeksi) | Anlam | Bölen |
|---|---|---|
| [3] | `C` = başarı, `E` = hata | — |
| [4] | hatada hata kodu; başarıda tip (`1` = tam set) | — |
| [5:8] | yağ oranı % | /10 |
| [8:12] | yağ kütlesi kg | /10 |
| [12:16] | bazal metabolizma kcal | 1 |
| [16:19] | su % | /10 |
| [19:21] | su puanı | 1 |
| [21:24] | iskelet kası | /10 |
| [24:26] | iskelet kası puanı | 1 |
| [26:29] | iç yağ (viseral) | /10 |
| [29:31] | iç yağ puanı | 1 |
| [31:34] | kemik mineral | /10 |
| [34:36] | kemik puanı | 1 |
| [36:39] | hücre dışı sıvı | /10 |
| [39:42] | hücre içi sıvı | /10 |
| [42:45] | toplam su | /10 |
| [45:48] | protein | /10 |
| [48:51] | inorganik tuz (mineral) | /10 |
| [51:53] | vücut yaşı | 1 |
| [53:56] | genel puan | /10 |
| [56:60] | kas kütlesi (varsa, [56] `$` değilse) | /10 |

Yalnız tip `1` değilse ilk üç alan (yağ %, yağ kg, BMR) gelir.


## Kablo karşılaştırması — DÜZELTME (26.11 akşam, ikinci fotoğraf seti)

İlk yorum yanlıştı. Çin'den gelen kioskun net fotoğrafında BMH05108 **USB-C ile Android'e bağlı**, tıpkı bizdeki gibi; UART soketi boş. Ek olarak orada **ayak elektrotları** (üstteki TwoLegs soketleri) da takılı. Android kartı farklı (bizde Lztek RK3576 + TTL/USB ara kartları); tartı için her iki kioskta ayrı yeşil Dingheng kontrol kartı var (COIN/PRINT/BLOOD_KEY).

Buna göre Çin kioskunun uygulamasında `fatSerial` (八电极脂肪串口) tanımlı → `isEight=1` → "eightFat" sayfası; "iki el" seçilince `FatController.start2Fat` BMH'ye doğrudan **`550CD2…`** gönderiyor (tam vücut seçilirse `551ED0…`). Yani `#FM` yolu büyük ihtimalle orada **kullanılmıyor**; BMH aynı 0xD2'ye orada yanıt veriyor gibi.

Görülen farklar (koddan):
- Empedans sorgusu: onlar `55 05 B1 01 F4`, biz `55 05 B1 51 A4`.
- Ölçüm modu: el+ayak takılı olduğu için 8 elektrot modları (`55 06 B0 01 06 EE` 100 kHz, `55 06 B0 01 03 F1` 20 kHz) kodda; iki el modu öncesi hangi B0 gönderildiği net değil.
- D2 çerçevesi bizimkiyle aynı yapı (cinsiyet, 00, boy, yaş, kilo LE, empedans LE, sağlama).

Kesin teşhis için: Çin kioskunda `adb logcat` alınıp "iki el" ölçümü yapılmalı (çerçeveler ve kartın sürümü görünür); ya da onların APK'sı bizim kioska kurulup aynı ölçüm denenmeli (donanım mı yazılım mı ayrılır).


## Çin kioskunun logu (cin-kiosk-log.txt, 26.11 20:19) — kesin bilgi

Akış (iki el, erkek, 31 yaş, 169 cm, 72,3 kg):
```
→ 55 06 B0 03 05 ED            ← AA 05 B0 00 A1      (iki el 50 kHz)
→ 55 05 B1 01 F4  (500 ms'de bir) ← AA 0D B1 05 01 01 …  (durum 1: temas bekleniyor)
                               ← AA 0D B1 05 03 01 00 00 74 03 00 00 18  (durum 3: 884 Ω)
→ 55 0C D2 01 00 A9 1F D3 02 74 03 B8   (14 ms sonra)
← AA 56 D2 11 00 B5 00 FB 00 … AD  (86 bayt, 7 ms içinde)  → yağ %25,1, yağ 18,1 kg, BMR 1555
```
- **Bizim istek üreticimiz aynı girdilerle bayt bayt aynı çerçeveyi üretiyor; bizim çözücümüz bu cevabı doğru çözüyor** (test: `Bmh05108TwoArmsFixtureTest`).
- Bizim başarısız iki logda da cinsiyet baytı **00 (kadın)** idi (`55 0C D2 00 …`). Çin kioskundaki başarılı örnek **01 (erkek)**. Tek kalan fark bu (ve kartın kendisi).
- Sıradaki deneme: bizim kioskta **Erkek** seçerek ölçüm. Cevap gelirse kart yazılımı kadın (0) değerinde D2'yi yanıtlamıyor demektir → üreticiye sorulacak; gelmezse fark karttadır (Çin kioskunun kartının sürümü alınmalı / kartlar takas edilerek denenmeli).
- B1 sorgu baytı farkı (onlar `01`, biz `51`) cevabı etkilemiyor; ikisi de durum/empedans döndürüyor.


### Çin kioskunda kadın denemesi (cin-kiosk-log2.txt, 20:24)
`55 0C D2 00 00 A9 1F D4 02 A4 03 88` (kadın, 932 Ω) → `AA 56 D2 11 …` 86 bayt, yağ %37,9 / 27,4 kg / BMR 1346. **Cinsiyet sorun değil.** B1 sorgu baytı farkı (01 = geçerli frekans, 51 = 50 kHz) belgeye göre eşdeğer.

Kalan olasılıklar: (a) bizim kart (yazılım V5.1/V2.1 farklı ya da arızalı algoritma), (b) bizim Android → kart yolunda 12 baytlık çerçevenin bozulması. Not: bizim logda D2'den sonra gönderilen B0 durdurma cevabı ~500 ms gecikmeli geldi (normalde ~15 ms); kart D2'de takılıyor olabilir.

Ayırt etmek için: (1) bizim kartı USB-C ile PC'ye bağlayıp seri terminalden aynı çerçeveleri elle gönder; (2) iki kioskun BMH kartlarını (ya da yalnız USB-C + el kablolarını) takas et.

## Yarın yapılacaklar

1. **Kart mı yazılım mı:** bizim BMH'yi PC'de seri terminalle dene (B0 03 05 → B1 01 durum 3'e kadar → D2) ya da iki kioskun BMH kartlarını takas et.
2. **Yazılım (onay bekliyor, ~45 dk):** Dingheng tartı adaptörüne `#FM` gönder/çözümle; admin'e "Vücut analizi kaynağı: Tartı kartı / BMH" seçimi; tartı kartı seçiliyse `#FM` sonucu, alınamazsa tahmin yedek. Varsayılan bugünkü gibi BMH.
3. Kablo takılmadan da denenebilir: `#FM…$` gönderilince `#FME?$` (hata) dönerse kart komutu destekliyor demektir.
4. BMH'yi yine de istersen üreticiye sor (sürüm V5.1/V2.1, 0xD2 yanıtsız; ISP güncellemesi 0xE1 mümkün).
