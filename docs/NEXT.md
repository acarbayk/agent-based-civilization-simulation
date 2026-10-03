# Kaldığımız yer (devam notu)

Dal: `claude/sleepy-mendel-yympgn`; son hâli main'e de gönderildi (hızlı ileri sarma).
Yayındaki sayfa: https://claude.ai/artifact/UFndSmnwGi5eh4gszd11jF (sürüm 22, en güncel).

## Şu an neler var
- Benzetim: seed'li, determinist ajan tabanlı toplum (kişilik, aile, ekonomi, tarım, savaş/yağma, siyaset, hükümdarlar birey).
- Arayüz: ortaçağ temalı masaüstü arayüz, haritada kişi/şehir/sınır, Tarih Kitabı (Legends), takip çubuğu, toast, yıllık özet, ayarlar, tur.
- Hızlı başlangıç (30 yıl önceden oynatma) ve Yönetmen modu (kamera ilginç olayları izler).
- **Danışman modu** (hedef: büyüme): ikilem kartları, hükümdar güveni, rütbeler; savaşı oyuncu kartla yönetir; kart gelince oyun durur.
- **İlahi eller**: inanç puanıyla yağmur, şifa, ilham, yıldırım, deprem, salgın.
- Hızlar ¼×–8× (¼× ve ½× danışman oyunu için).
- Anlatı: olay cümleleri duruma/ada bağlı; yıllık minik hikâyeler (dostluk, husumet, varis, alamet).

## Dürüst durum
- Bağımsız değerlendirmeler: benzetim gerçekçiliği ~6,1, kullanılabilirlik 6,3, **oyuncu testi 5,5 (iki tur)**. Hedef ≥8 sağlanmadı; sağlandığı iddia edilmiyor.
- Gerçek bir insan oyun testi hiç yapılmadı (yalnızca otomatik, kör ajan testleri).

## Açık işler (öncelik sırasıyla)
1. Kararların etkisi hâlâ görünmüyor: büyümeye etkisi dünya gürültüsüne gömülüyor; seçimlerin doğrudan sonucunu (şehir, hazine, güven) 1-2 yıl içinde göster.
2. Kart çeşitliliği: savaş dışı kartlar (din, ekonomi ödülleri, ticaret zinciri); hazinenin sürekli 0 kalması.
3. Toast yığılması, alt olay günlüğünün küçüklüğü, düz açılışta "ne yapmalıyım" yönlendirmesi.
4. Savaş gerekçeleri hâlâ 3-4 şablon; ikinci dereceden gerçekçilik açıkları (hane mülkiyeti, savaş taktiği, ihanet zinciri, isyan parçalanması).
5. Gerçek insan testi; mobil ve parşömen teması yapılmadı; 8× gerçek hızı ~600 kişide 2-3×.

## Notlar
- Test betikleri oturumun geçici klasöründeydi (repoda yok): harness.js (node vm), det.js (determinizm), dyn.js (makro), adv*.js/war1.js (danışman deneyleri), tarayıcı betikleri (Playwright).
- Bekleme döngüsünde `pgrep -f` kendi komutunu eşleştirir; dosya boyutuna bakan döngü kullan.
- Ana dalın son hâlini yayınlarken `index.html` başlık/gövde etiketleri çıkarılıp `color-scheme:dark` eklenerek artifact'a gönderiliyor.
