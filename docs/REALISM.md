# Gerçekçilik planı ve değerlendirme ölçütü

Hedef: izleyene "burada gerçekten insanlar yaşıyor" hissini veren bir yapay toplum. Oyun değil, simülasyon.
"10 üzerinden 8" öznel bir ölçüdür; bu yüzden aşağıda her boyut için ölçülebilir bir kanıt ve bağımsız bir
gözden geçirme adımı tanımlıdır. Puanlar baştan dürüstçe yazıldı.

## Araştırmadan çıkan boşluklar

| Konu | Kaynak | Şu anki durum |
|---|---|---|
| Ön sanayi demografisi: bebek ölümü %15-25, 15 yaşa ulaşma ≈%69, doğumda beklenen ömür 25-40 ([Our World in Data](https://ourworldindata.org/child-mortality-in-the-past), [Clark](https://faculty.econ.ucdavis.edu/faculty/gclark/Farewell%20to%20Alms/FTA-chapter5-a.pdf)) | demografi | Sabit ömür (52-88), bebek ölümü yok, cinsiyet yok, doğum = "şehirde yiyecek var" |
| Arazi, hane, gerçek verim haritası, nüfusu arkeolojik veriye karşı doğrulama ([Artificial Anasazi](https://jasss.soc.surrey.ac.uk/12/4/13.html)) | mekân ve ekonomi | Düz zemin, eve ait birey yok, mevsim yok |
| Küçük ölçekli toplumlarda savaş ölümü %10-40'a kadar, çoğu baskın ([Annual war deaths](https://www.journals.uchicago.edu/doi/abs/10.1086/694568), [war group size](https://www.pnas.org/doi/10.1073/pnas.1713972114)) | savaş | Asker şehre yürür ve vurur; baskın, yağma, sezon yok |
| Hikâyenin izleyiciye görünür kılınması ([Dwarf Fortress](https://www.researchgate.net/publication/356686095_Characterization_and_Emergent_Narrative_in_Dwarf_Fortress), [hikâye keşfi](https://dl.acm.org/doi/fullHtml/10.1145/3555858.3555909)) | okunabilirlik | Tıklayınca bilgi var; kendiliğinden öne çıkan hikâye, takip, zaman çizgisi yok |

## Aşamalar

1. **Demografi:** cinsiyet, yaşa bağlı ölüm tehlikesi (Gompertz-Makeham), evli kadından gebelik ve doğum aralığı,
   açlık ve kalabalığa bağlı verimlilik, salgın. Başlangıç nüfusu ve sınırlar buna göre yeniden kurulur.
2. **Mekân:** tohumlu arazi (verimli vadi, nehir, dağ, orman), mevsimler, köyler ve haneler, günlük döngü.
3. **Savaş:** baskın ve yağma, mevsimsel seferberlik, savaş nedenleri; ölüm payı tarihsel aralıkta.
4. **Okunabilirlik:** birey takibi, hayat çizgisi, kendiliğinden öne çıkan hikâye akışı, aile ağı, nüfus piramidi.
5. **Doğrulama:** ölçülen metrikler + bağımsız gözden geçirme (taze bir ajan ekran görüntülerini ve çıktıları
   ölçüte göre puanlar) + bilinen sınırların yazılması.

## Değerlendirme ölçütü (0-10)

Her boyutun yanındaki kanıt ölçülür. İlk sütun, bu belge yazılırken (Adım 1-5 tamamlandıktan sonra) benim
dürüst puanımdır. Hedef: ortalama ≥8 ve hiçbir boyut <6.

| Boyut | Kanıt | Şimdi | Hedef |
|---|---|---|---|
| Demografik gerçekçilik | doğumda beklenen ömür 30-40, bebek ölümü %15-25, 15'e ulaşma %60-75, nüfus piramidi | 2 | 8 |
| Mekân ve ekonomi | arazi, mevsim, hane, yerleşim | 2 | 7 |
| Toplumsal yapı | aile, eş, dost, hasım, hanedan | 7 | 8 |
| Birey zihni | kişilik, hafıza, hedef | 7 | 8 |
| Savaş ve siyaset | savaş ölüm payı, baskın, canlı siyaset | 4 | 7 |
| Okunabilirlik ve his | takip, hikâye akışı, ilk izlenim | 3 | 8 |
| Doğrulama | ölçüm, bağımsız gözden geçirme, dürüst sınırlar | 6 | 8 |
| Tekrarlanabilirlik ve kararlılık | seed, hata yok, geniş seed taraması | 7 | 8 |
