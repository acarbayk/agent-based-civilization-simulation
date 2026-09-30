# Uygulama planı

Kaynak: `REALISM.md` (boşluklar, ölçüt), `SOURCES.md` (doğrulama düzeyleri). Sıra bağımlılığa göre seçildi:
demografi her şeyin zeminidir (nüfus eğrisi değişince siyaset, isyan ve savaş eşikleri yeniden ayarlanır), bu
yüzden ilk o yapılır; arayüz en son, çünkü gösterilecek şeyler oturduktan sonra anlamlı olur.

## Kural: her aşama bir "kapı"dan geçer

Bir aşama ancak şu üçü tamamsa kapanır: (1) **ölçüm** hedef aralıkta, (2) **regresyon** yok (aynı seed aynı sonuç,
hata yok, 8 seed × 285 yıl), (3) **belge** güncel (ölçüm tablosu + bilinen sınır). Kapıdan geçmezse bir sonraki
aşamaya geçilmez.

| # | Aşama | Yapılacaklar | Kapı (ölçülür) | Risk |
|---|---|---|---|---|
| 0 | Temizlik | Kaynakları doğrula, hatalı iddiaları düzelt, RNG'yi `sfc32`'ye çevir | SOURCES.md tam, RNG ki-kare ve korelasyon testi, determinizm | düşük |
| 1 | Demografi | cinsiyet, yaşa bağlı ölüm (Gompertz-Makeham), evli kadından doğum, taşıma kapasitesi, salgın, kurucu aileler | e0 28-38, bebek ölümü %13-25, 15'e ulaşma %50-70, nüfus yok olmaz ve çökmez, evli oranı makul | **yüksek**: makro denge (isyan, savaş) yeniden ayarlanır |
| 2 | Makro yeniden ayar | Nüfus eğrisi değişti: isyan eşiği, savaş sıklığı, kuraklık etkisi yeniden | 8 seed × 285 yıl: hata 0, yok olma 0, geç dönem siyaset canlı | yüksek |
| 3 | Mekân | Tohumlu arazi, mevsim, köy, hane, günlük döngü | verim haritası nüfusu sınırlıyor; mevsimsel doğum/ölüm tepkisi ölçülebilir | orta |
| 4 | Savaş | Baskın, yağma, seferberlik; ölüm payı aralıkta | savaş ölümü toplam ölümün makul payı (aralık: SOURCES #9 belirsiz, geniş), nüfus çökmez | orta |
| 5 | Okunabilirlik | Birey takibi (kamera), hayat çizgisi, öne çıkan hikâye akışı, nüfus piramidi, mevsim ve gün görseli | tarayıcı testi, ekran görüntüsü, taze bir gözden geçirme ajanı | orta |
| 6 | Doğrulama | Geniş seed taraması (≥20), rubrik puanı, bağımsız gözden geçirme, yayın | rubrik ortalaması ≥8 iddiası yalnızca kanıtla; kalan sınırlar yazılı | düşük |

## Dürüstlük kuralları

- Ölçüt tutmazsa "tuttu" denmez; tablo gerçek sayıyı gösterir.
- Kaynakla doğrulanamayan sayı hedef olarak kullanılmaz veya "belirsiz" diye işaretlenir.
- Bir aşama hedefi tutturamazsa sebep ve denenenler belgelenir; ölçüt gevşetilmez.
- Tüm sonuçlar bu ortamın ölçüm düzeneğindedir (Node `vm` + Chromium); gerçek cihaz performansı ölçülmedi.

## Sıra değişikliği (kayıt)

Aşama 5 (okunabilirlik) planlanandan önce yapıldı. Gerekçe: en büyük "hissettirme" açığı yaşamın görünmemesiydi ve
iş görece düşük riskliydi; arazi ve savaş işleri daha riskli. Arazi (3) ve savaş (4) sonra geliyor.

## Durum

- Aşama 0 (temizlik): tamam.
- Aşama 1 (demografi): tamam, kapı geçti. Doğumda beklenen ömür 31-34, 15'e ulaşma %61-68, evli kadın %81-84.
- Aşama 2 (makro yeniden ayar): **kısmen**. 8 seed × 15.000 adımda hata 0, yok olma 0, ortalama canlı krallık 2.88, geç dönem
  savaş/fetih 6/8 seed'de var. **Açık borç: geç dönemde isyan hiç çıkmıyor** (8 seed'de 0).
- Aşama 5 (okunabilirlik): birey takibi, yakınlaştırma, aile ağı çizgileri, hayat çizgisi, yıllık hikâye akışı, nüfus piramidi, mevsim.
