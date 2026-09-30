# Kaldığımız yer (devam notu)

Dal: `claude/sleepy-mendel-yympgn` (PR #2 açık, main'e henüz birleştirilmedi; birleştirme kullanıcı onayıyla).
Yayındaki artifact: https://claude.ai/artifact/UFndSmnwGi5eh4gszd11jF (sürüm 7 = tarım katmanı ÖNCESİ).

## Son durum
- Bağımsız değerlendirme 5: ortalama 6,1 (hedef ≥8 sağlanmadı). Tur 6 düzeltmeleri (hükümdar hatası, kişilik→seçim, hekim etkisi, eksi ambar, Ölçümler paneli, Türkçe ek) bağımsız olarak henüz puanlanmadı.
- Tur 7 (tarım katmanı) kodda: bahar ekim, yaz büyüme, sonbahar hasat, kış çürüme; `CFG.fields`, `fieldShare` 0,5, `foodScale` 0,24; şehir çevresinde tarla yamaları; "kötü/bereketli hasat" hikâyeleri.
- Kalibrasyon (3 seed × 120 yıl): açlık ölümü %4-13, beklenen ömür 34-36, 15'e ulaşma %61-66. foodScale 0,18'de açlık %20-39 çıkmıştı.
- Makro siyaset: foodScale 0,18 + tarım ile 8 seed × 250 yıl kararlı (hata 0, yok oluş 0, ort. krallık 5,9). foodScale 0,24 ile tam makro ölçüm YAPILMADI (iptal edildi).

## Yarın sırayla
1. foodScale 0,24 ile 8 seed × 250 yıl makro ölçüm (dyn.js), tarayıcı + determinizm testi, docs/RESEARCH.md §10.17 + CHANGELOG, artifact'ı yeniden yayınla.
2. Kör değerlendirme 6 (taze ajan, anlık kopya).
3. Açık eksikler: hane mülkiyeti/bireysel servet, gerçek savaş taktiği + seferberlik, ihanet zinciri, isyan parçalanması, nüfus üst sınırı, hikâye çeşitliliği, birincil kaynak doğrulaması (ağ engeli).
4. Puan ≥8 kanıtlanırsa ve kullanıcı onaylarsa PR #2'yi main'e birleştir.

## Notlar
- Test betikleri oturumun scratchpad klasöründeydi (repoda yok): harness.js (node vm), dyn.js (makro), det.js (determinizm), browser*.js (Playwright). Gerekirse yeniden yazılır.
- Arka plan beklerken `pgrep -f` kalıbı kendi komutunu eşleştirir; bekleme döngüsünde kullanma.
