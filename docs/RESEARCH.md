# Araştırma notları

Bu belge, "her birey kendi hayatını yaşayan bir ajan olsun" hedefi için yapılan araştırmayı ve
bundan çıkan yol haritasını özetler. Web araştırması Eylül 2026'da yapıldı; kodda zaten adı geçen
klasikler (Sugarscape, Axelrod, Turchin, Epstein) için kaynaklara yeniden bakılmadı, onlar
mevcut kod yorumlarındaki referanslardır.

## 1. Tekrarlanabilirlik (Adım 1'in dayanağı)

- Deterministik bir simülasyon için seed'li bir PRNG şarttır. JavaScript'te `Math.random()` seed
  alamaz; küçük ve hızlı seçenekler `mulberry32`, `sfc32`, `xoshiro128**`. Bu projede `mulberry32`
  kullanıldı.
  ([Mulberry32 açıklaması](https://www.4rknova.com/blog/2026/03/01/mulberry32-rng),
  [Blobs in Games: PRNG](https://simblob.blogspot.com/2022/05/upgrading-prng.html))
- Ajan tabanlı modellerde iyi uygulama: rastgele akışlarını karar türüne göre ayırmak, böylece bir
  sistemdeki değişiklik diğerlerinin rastgele sayı dizisini kaydırmasın.
  ([agentpy: Randomness and reproducibility](https://agentpy.readthedocs.io/en/latest/guide_random.html),
  [Common Random Numbers in ABMs](https://arxiv.org/html/2409.02086v1))
- Şimdilik iki akış var (`rng` simülasyon, `rngUI` arayüz tıklamaları). Sosyal katman gelince
  akışları alt sistem başına (`ilişki`, `doğum`, `savaş`…) ayırmak iyi olur.

## 2. Bireyi "yaşayan" yapmak

| Kaynak | Bu proje için çıkarım |
|---|---|
| [Dwarf Fortress: kişilik yüzeyleri ve değerler](https://dwarffortresswiki.org/index.php/Personality_facet) | *Facet* nasıl davrandığını, *değer* neye inandığını belirler. Değerleri çok farklı iki kişi arasında kin oluşur. Anılar zamanla kişiliği değiştirir. Bireyde iki ayrı vektör tutmak yeterli ve ucuz. |
| [Crusader Kings II tasarımı](https://www.gamedeveloper.com/design/the-surprising-design-of-i-crusader-kings-ii-i-) | Tek bir "görüş" (-100…+100) değeri, nedenlerin toplamıdır ve **tek yönlüdür** (A→B ≠ B→A). Yapay zekâ bu değerlere bakarak karakterinde kalır. İlişki katmanı için doğrudan uygulanabilir. |
| [Generative Agents (Park ve ark. 2023)](https://ar5iv.labs.arxiv.org/html/2304.03442) | Hafıza akışı (yenilik, önem, ilgililik ile puanlama) + yansıma + plan. Yansıma çıkarılınca davranış 48 simüle saat içinde tekrarlayan hale geliyor. Ancak 25 ajan × 2 gün binlerce dolar tutmuş. |
| [Affordable Generative Agents](https://arxiv.org/pdf/2402.02053) | LLM maliyetini düşürmeye yönelik çalışmalar var; yine de yüzlerce ajanı her adımda LLM'e bağlamak pratik değil. |

**Sonuç:** Bireyin zihni kural tabanlı (hafıza + ilişki + kişilik + hedef) kurulmalı, LLM yalnızca
nadir anlarda (efsane biyografisi, isyan manifestosu, kral gerekçesi) ve isteğe bağlı olarak
kullanılmalı.

## 3. Yol haritası

1. **Temel** (bu commit): seed'li RNG, hata düzeltmeleri, ızgara performansı.
2. **Sosyal katman:** kişilik (facet) + değer vektörü, tek yönlü ilişki haritası, gerçek ebeveyn/eş.
3. **Hafıza ve hedef:** olay hafızası (önem + yenilik çürümesi), kararı etkileyen kısa vadeli hedefler.
4. **Liderler gerçek birey:** kral kararları krallığın sayılarından değil, liderin kişiliğinden çıkar.
5. **Modülerleştirme** ve isteğe bağlı LLM anlatı katmanı.

## 4. Paylaşılan kaynakların doğrulanması

Üç dosya paylaşıldı (10-12 kitaptan yalnızca bunlar geldi). Her biri açılıp künyesi ve ilgili bölümleri
kontrol edildi; kitapların tamamı okunmadı.

| Kaynak | Doğrulama | Bu proje için değeri |
|---|---|---|
| Epstein (2002), *Modeling civil violence*, PNAS 99(suppl. 3) | Makale metni okundu. Formüller: `G = H(1−L)`, `P = 1−exp(−k·C/A)`, `N = R·P`, isyan ⇔ `G−N > T`; `k=2.3`, `T=0.1`. Hâlâ kanonik referans (Mesa ve NetLogo'da örnek modeli var). Literatürdeki eleştiriler: gerçekçi olmayan hareket, kaba "polis" davranışı, ampirik doğrulama yok, hafıza yok ([değerlendirme](https://arxiv.org/pdf/1501.05838)). | Yüksek. Mevcut kod bu modeli **eksik** uyguluyordu: `P` terimi yoktu, `riskK` ve `rebelThresh` ayarları tanımlı ama kullanılmıyordu. |
| Millington & Funge, *Artificial Intelligence for Games*, 2. baskı (CRC Press, 2009) | Künye doğrulandı. Daha yeni bir baskı var: 3. baskı (2019, yalnızca Millington, [Routledge](https://www.routledge.com/AI-for-Games-Third-Edition/Millington/p/book/9780367670566)); 3. baskıda ne değiştiği doğrulanamadı. 17 yıllık olsa da algoritmalar (GOB, durum makineleri) kalıcı. | Yüksek. §5.7 Hedef Yönelimli Davranış (hedef *insistence* değeri, eylemlerin hedefleri karşılaması, memnuniyetsizlik) 2. adımın bireysel zihni için doğrudan şablon. §9.3 "AI Level of Detail": önemli bireylere tam, kalabalığa basit hesap. Kodda anılan "Dave Mark response curve" bu kitapta **yok** (Dave Mark'ın ayrı bir kitabı); kontrol edilemedi. |
| Swink, *Game Feel* (Morgan Kaufmann/Elsevier, 2008) | Yalnızca künye kontrol edildi: 1. baskı, 2008, ikinci baskı bulunamadı. İçeriği okunmadı. | Düşük. Kitap oyuncu girdisine gerçek zamanlı tepkiyi (his, cilâ) anlatıyor; özerk ajanları değil. En fazla izleyici arayüzü için dolaylı. |

## 5. Krallık churn deneyi

Sorun: bazı seed'lerde 12.000 adımda (~170 yıl) 66-77 krallık doğuyordu (baseline: 8 seed'in
üçünde; diğerleri 4-8). Teşhis: (a) tek şehirli devletin "bölünmesi" aslında kendi başkentini
yeniden adlandırmaktı, (b) isyan ölçütü anlık tek bir eşikti, (c) bölünmeden sonra soğuma yoktu,
(d) büyük imparatorluğu yıpratan bir mekanizma yoktu (sonunda tek devlet donup kalıyordu).

Uygulanan tasarım (parametreler `CFG` içinde):
- **Epstein kuralı (korku terimi bizim eklememiz):** görüşteki asker (`C`) ve aktif isyancı (`A`) oranından
  tutuklanma olasılığı `P`; `N = risk·P + korku·0.18`.
- **Sürekli huzursuzluk:** eşik üstünde `secedeSustain=6` ardışık kontrol (~1.4 yıl) gerekir.
- **Soğuma:** bölünmeden sonra ana ve yeni devlet için `secedeCool=420` adım (~6 yıl).
- **Saray darbesi:** tek şehirli devlet bölünemez; yeni hanedan gelir, meşruiyet yükselir.
- **Aşırı genişleme:** meşruiyet hedefinden `overreach·(şehir−2)/5` düşer (Turchin'in asabiya
  fikri: bir devlet büyüdükçe çekirdeğin bağı gevşer; [Gavrilets ve Turchin](http://volweb2.utk.edu/~gavrila/papers/turchin-gavr.pdf),
  [büyük imparatorluklar kuramı](https://www.researchgate.net/publication/46545172_A_theory_for_formation_of_large_empires);
  Tainter'in azalan verim argümanı).

Ölçüm (20.000 adım ≈ 285 yıl, 4 seed; "ever" = toplam kurulan krallık, başlangıçtaki 4 dahil):

| Varyant | ever | sonda canlı |
|---|---|---|
| Eski kod (12.000 adım) | 4-77 | 2-5 |
| Epstein, T=0.10 | 15-34 | 2 |
| Epstein, T=0.12 | 15-24 | 2 |
| **Epstein, T=0.13 (seçilen)** | 8-26 | 1-4 |
| Epstein, T=0.14 | 6-13 | 1-3 |
| Epstein, T=0.18 | 4-6 | 1-4 (çoğu seed'de hiç olay yok) |

`T` keskin bir faz geçişi gibi davranıyor (makalenin de gösterdiği gibi): altında sürekli çalkantı,
üstünde donmuş dünya. Makaledeki 0.1 yerine 0.13 seçildi; çünkü bu modelde zorluk (`H`) sabit
dağılımlı değil, açlık ve hoşnutluktan hesaplanıyor. Bilinen sınır: bazı seed'lerde 285 yılın
sonunda tek devlet kalabiliyor; bu doğal çeşitlilik sayıldı, ama izlenmeli.

## 6. Henüz incelenmeyenler

Proje sahibinin verdiği diğer kaynaklar (toplam 10-12 olduğu söylendi, 3'ü geldi) görülemedi.
Turchin'in yapısal-demografik kuramına yönelik eleştiriler de aranmadı; arama sonuçlarında çıkmadı.
