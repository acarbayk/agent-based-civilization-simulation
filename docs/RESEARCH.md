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

## 4. Henüz incelenmeyenler

Proje sahibinin terminaldeki oturuma verdiği 10-12 kaynak kitap bu oturumda görülemedi. Listesi
paylaşılırsa bu belgeye eklenip yol haritası buna göre güncellenebilir.
