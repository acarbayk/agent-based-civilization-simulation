# Uygulama planı

Kaynak: `REALISM.md` (boşluklar, ölçüt), `SOURCES.md` (doğrulama düzeyleri). Sıra bağımlılığa göre seçildi:
demografi her şeyin zeminidir (nüfus eğrisi değişince siyaset, isyan ve savaş eşikleri yeniden ayarlanır), bu
yüzden ilk o yapılır; arayüz en son, çünkü gösterilecek şeyler oturduktan sonra anlamlı olur.

## Kural: her aşama bir "kapı"dan geçer

Bir aşama ancak şu üçü tamamsa kapanır: (1) **ölçüm** hedef aralıkta, (2) **regresyon** yok (aynı seed aynı sonuç,
hata yok, 8 seed × 285 yıl), (3) **belge** güncel (ölçüm tablosu + bilinen sınır). Uygulamada kapı 12 seed × 250 yıl olarak koşuldu (planda 8 × 285 yazıyordu). Kapıdan geçmezse bir sonraki
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

- Aşama 0 (temizlik), 1 (demografi), 2 (makro yeniden ayar), 5 (okunabilirlik): yapıldı.
- Aşama 3 (arazi): **kısmen.** Arazi haritası, mevsim, verime bağlı yiyecek var; ama nüfusu yiyecek değil sabit performans
  tavanı sınırlıyor ve insan dağılımı araziye zayıf bağlı. Kapıdaki "verim haritası nüfusu sınırlıyor" **tutmadı.**
- Aşama 4 (savaş): **kısmen.** Yalnızca savaştaki devletlere saldırı, sefer mevsimi, düşük ölümcüllük. Baskın/yağma/seferberlik yok.
- Aşama 6 (doğrulama): bağımsız gözden geçirme tur 1 = 6.2, tur 2 = 6.4, tur 3 = 6.4 (hedef ≥8, tutmadı). Tur 3 ve 4 düzeltmeleri yapıldı
  (yok olma hataları, rol çeşitliliği, yumuşak doğum tavanı, siyaset uçları).
- Güncel sayılar `RESEARCH.md` §10'da.
