# Araştırma notları

Bu belge, "her birey kendi hayatını yaşayan bir ajan olsun" hedefi için yapılan araştırmayı ve
bundan çıkan yol haritasını özetler. Web araştırması Eylül 2026'da yapıldı; kodda zaten adı geçen
klasikler (Sugarscape, Axelrod, Turchin, Epstein) için kaynaklara yeniden bakılmadı, onlar
mevcut kod yorumlarındaki referanslardır.

## 1. Tekrarlanabilirlik (Adım 1'in dayanağı)

- Deterministik bir simülasyon için seed'li bir PRNG şarttır. JavaScript'te `Math.random()` seed
  alamaz; küçük ve hızlı seçenekler `mulberry32`, `sfc32`, `xoshiro128**`. Başlangıçta `mulberry32` kullanıldı; yazarı 2022'de artık önermediğini belirtti (tüm 32-bit değerleri üretmiyor), bu yüzden `sfc32`'ye geçildi (bkz. SOURCES.md #21).
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

## 6. Adım 2: kişilik, değerler, ilişkiler, aile

**Araştırma.** Sosyal simülasyonlarda Big Five kişilik ve homofili ile bağ oluşumu
([LLM ajanlarla ortaya çıkan bağlar](https://arxiv.org/html/2510.19299); ön baskı, yalnızca ilham); Dunbar katmanları (kuramsal 5-15-50-150; telefon verisinde ölçülen 4-11-30-129, kat ≈2.7),
yani ilişki sayısı sınırlı ([MacCarron ve ark. 2016](https://arxiv.org/pdf/1604.02400)); evlilik piyasası
ABM'leri: benzer olanı arama, yaş farkı tercihi ([MADAM/KAMA özeti](https://www.jasss.org/11/4/5.html),
[Age-at-marriage](https://csde.washington.edu/downloads/toddbillarisimao.pdf)); Dwarf Fortress'ta davranış
(facet) ve inanç (değer) ayrımı ([wiki](https://dwarffortresswiki.org/index.php/Personality_facet), oyun mekaniği; bilimsel kanıt değil).

**Tasarım.** Her birey 5 kişilik özelliği (açıklık, sorumluluk, dışadönüklük, uyumluluk, kaygı) ve 4 değer
(gelenek, cemaat, onur, merak) taşır; ebeveyn ortalamasının %50'si + yeni şans. Görüş tek yönlüdür
(A→B ≠ B→A), birey başına 14 kayıtla sınırlıdır, yakınlarla temas ve değer/kişilik benzerliğiyle (homofili)
artar. Evlilik: iki yetişkin, akraba değil, yaş farkı ≤12 yıl, karşılıklı görüş yeterli. Doğumlarda çocuk
çoğunlukla evli bir çiftten gelir. Kişilik; meslek seçimini, isyan risk-kaçınmasını, korku tepkisini ve
cesareti etkiler.

**Ölçüm** (6000 adım, 4 seed):

| Ölçüt | Sonuç |
|---|---|
| Arkadaşlık ağı kümelenmesi | 0.29-0.50 (aynı yoğunlukta rastgele ağda 0.02-0.03) |
| Karşılıklı olumlu bağ oranı | 0.67-0.81 |
| Akraba evliliği ihlali | 0 |
| İki ebeveynli doğum oranı | %65-86 |
| Evli yetişkin oranı | %12-19 → ayar sonrası %24-42 |
| Arkadaşların uyumu / rastgele çift | 0.49-0.52 / 0.46 (homofili var ama zayıf; bağlar çoğunlukla yakınlıktan) |
| Ebeveyn-çocuk kişilik korelasyonu | 0.15-0.29 (tasarım hedefi ≈0.25) |

## 7. Adım 3: hafıza ve hedefler

**Araştırma.** ACT-R taban aktivasyonu: `B = ln Σ t^-d`, `d=0.5`
([Anderson & Schooler 1991; tanıtım: arXiv](https://arxiv.org/pdf/1306.0125)); Dwarf Fortress'ta sınırlı hafıza yuvaları,
aynı olaya kişiliğe göre farklı tepki ve anıların kişiliği kalıcı değiştirmesi
([bellek](https://dwarffortresswiki.org/index.php/DF2014:Memory_%28thought%29),
[duygu](https://dwarffortresswiki.org/index.php/DF2014:Emotion)); The Sims tarzı ihtiyaç tabanlı yapay
zekâ ([Zubek](https://robert.zubek.net/publications/Needs-based-AI-draft.pdf)); Millington §5.7 hedef *insistence*.

**Tasarım.** Birey başına 10 anı yuvası (en düşük aktivasyonlu düşer). Yas, evlilik, doğum, öldürme, açlık,
fetih ve intikam anılarına dönüşür; kaygılı kişi acıyı daha şiddetli yaşar. Anılar ruh halini ve korku tabanını
belirler, büyük yas kaygıyı kalıcı artırır. Hedefler: intikam, eş arama, aileye dönme, dost arama; her birinin
*insistence* değeri var ve hedef eylemleri rol eylemleriyle yarışır (histerezisli seçim).

**Ölçüm** (6000 adım, 4 seed, hedefler açık ve kapalı):

| Ölçüt | Hedef kapalı | Hedef açık |
|---|---|---|
| Eylemlerin hedef güdümlü oranı | 0 | %16-19 |
| Evli yetişkin oranı | %35-39 | %40-45 |
| Aynı roldeki bireyler arası davranış farkı (JSD, yüksek = daha ayrışmış) | 0.133 | 0.160 (seed'ler arası gürültü büyük) |
| Tamamlanan intikam | 0 | 5-13 / seed (eşik düzeltmesinden sonra; ilk sürümde 0'dı) |

## 8. Adım 4: hükümdar bir bireydir

**Araştırma.** Hermann'ın liderlik özellik analizi (LTA, 7 özellik): kişilik dış politika yönelimini belirler (bizim eşlememiz Big Five'dan, LTA'dan türetilmedi); düşük uyumluluk ve
yüksek güvensizlik saldırganlığı artırır ([Hermann](https://www.researchgate.net/publication/253070340_Assessing_Leadership_Style_A_Trait_Analysis)).
Veraset kuralları: net kural (primogeniture) istikrarlı, tanistry ve seçim kriz üretir
([Kokkonen & Sundell 2014, APSR](https://www.cambridge.org/core/journals/american-political-science-review/article/abs/delivering-stabilityprimogeniture-and-autocratic-survival-in-european-monarchies-10001800/2399079C174599A840E5230E8827609C): primogeniture 960 hükümdarlık veride görevde kalma olasılığını 2 kattan fazla artırıyor); Crusader Kings veraset yasaları.

**Tasarım.** Krallığın hırs, kurnazlık, açgözlülük, sadakat ve yenilikçilik değerleri hükümdarın kişilik ve
değerlerinden türer. Veraset sırası: ilk çocuk, kardeş, sonra popülerliğe göre seçim (seçim = veraset krizi,
meşruiyet düşer). Darbe ve isyan yeni hükümdar getirir; liderler arası kişisel kimya ilişkiyi etkiler.

**Ölçüm.** İlk sürümde savaş başlatan kralların hırsı genel ortalamadan farksızdı (0.52 ile 0.54, 0.64 ile
0.62): yani kişilik neredeyse etkisizdi. Karar ağırlıkları güçlendirilip barış eşiği liderin uyumluluğuna
bağlandı. Sonuç (8 seed × 12000 adım), 1000 krallık-örneği başına savaş:

| Hükümdarın hırsı | Savaş / 1000 örnek |
|---|---|
| Yüksek (>0.65) | 43.9 |
| Orta | 12.1 |
| Düşük (<0.50) | 0.0 (210 örnek) |

## 9. Adım 5: durgun denge, iklim ve rejim yaşı

**Bulgu.** Adım 4 sonunda dünya 2.500-3.500 adımda (3× hızda ~15-20 sn) 1-2 krallığa iniyor ve sonsuza dek
donuyordu: iki devlet kalıcı müttefik, teknoloji ve kültür biriktikçe meşruiyet 0.6-0.67'ye çıkıp huzursuzluk ≈0.
Nedenler: yiyecek üretimi ihtiyacın ~20 katı (110 nüfus için adım başına ~0.14 gerekir, üretim ~2.85), haritada
biriken ~3000 yiyecek ve ambarlar kuraklığı tamponlar.

**Denenenler.**
1. Hafif kuraklık (%20-45 verim): hiçbir etki.
2. Şiddetli kuraklık (%3-10 verim, 2-6 yıl): nüfus çöküyor (bir seed'de 440 → 82) ve toparlanıyor, ama siyasi
   olay yine çıkmıyor (kuraklık yine de bırakıldı: "☀ KURAKLIK" evreleri ve açlık anıları üretiyor).
3. **Rejim yaşı:** meşruiyet hedefi, rejimin yaşıyla 70 yılda −0.30'a kadar düşer (Ibn Haldun/Turchin: asabiya
   çürümesi; [Gavrilets ve Turchin](http://volweb2.utk.edu/~gavrila/papers/turchin-gavr.pdf)). Darbe ve bölünmede saat sıfırlanır.

| 4 seed, 15.000 adım | Geç dönem olay (t>4000) | Ortalama canlı krallık |
|---|---|---|
| İklim yok, rejim yaşı yok | 36 / 56 / 114 / 69 | 1.67 / 1.97 / 4.0 / 2.03 |
| **Kuraklık + rejim yaşı (son)** | 82 / 92 / 184 / 122 | 2.27 / 3.13 / 4.17 / 2.87 |

Geç dönemde savaş, isyan ve fetih geri geldi (bir seed'de 9 savaş, 2 isyan, 3 fetih).

## 10. Gerçekçilik turu (demografi, okunabilirlik, arazi, makro yeniden ayar, savaş)

Plan ve ölçüt: `PLAN.md`, `REALISM.md`. Kaynaklar ve doğrulama düzeyleri: `SOURCES.md`.

### 10.1 Test düzeneği hatası (düzeltildi)

Başsız test ortamı tuvali 900×700 sanıyordu; gerçek sayfada 900×520. Tüm erken başsız ölçümler (demografi, churn,
geç dönem siyaseti) %35 daha geniş bir haritada yapılmıştı. Düzeltildi ve **bu turdaki tüm ölçümler 900×520'de
yeniden yapıldı**. Önceki bölümlerdeki (5-9) sayılar 900×700 haritadadır, bu yüzden tarihsel referans olarak okunmalı.

### 10.2 Demografi

Cinsiyet, yaşa bağlı ölüm tehlikesi (Gompertz-Makeham: bebek 0.15, 1-4 yaş 0.03, 5-14 yaş 0.008, yetişkin
`0.008+0.00004·e^(0.095·yaş)` yıllık, kişisel kırılganlık, açlık, Malthus, salgın, mevsim), evli kadından gebelik ve
doğum aralığı, doğumda ölüm, eş eşleştirmesi (yılda iki kez), kurucu aileler. Son sürüm, 4 seed × 120 yıl:

| Ölçüt | Sonuç | Hedef aralık (SOURCES.md #7, #8) |
|---|---|---|
| Doğumda beklenen ömür | 35.0-39.4 (tur 4: 35.0-37.9) | 28-38 (bağımsız ölçümler: tur 2 kohort 37-40; tur 3 dönem 31.8-35.8, kohort 33.7-37.5) |
| Bebek ölümü | %15-18 (bağımsız tur 3: %16-21) | %13-25 |
| 15 yaşa ulaşma | %63-67 | %50-70 |
| Toplam doğurganlık | 4.4-5.2 | ön sanayi ≈4-7 (kesin kaynak doğrulanamadı) |
| 15-45 yaş kadınlardan evli | %78-85 | yüksek (kesin sayı doğrulanamadı) |

Not: bu sayılar savaşın barış döneminde öldürmesini düzelttikten sonra ölçüldü; öncesinde beklenen ömür 27.7-34.1 idi.
Hedef aralıkların gerekçesi zayıf (birincil kaynaklar okunamadı), bu yüzden "hedefi aştı" yerine "aralığın üst ucunda" demek daha doğru.

### 10.3 Okunabilirlik

Birey takibi (kamera, fare tekeri), seçili bireyin aile ve dost/hasım ağı çizgileri, hayat çizgisi (evlilik, çocuk,
hükümdarlık, ölüm), yılda en ilginç 3 olayı türe göre seçen hikâye akışı, nüfus piramidi ve yıllık doğum/ölüm,
mevsim etiketi ve tonu, hükümdar yıldızı. Tarayıcıda hatasız çalıştığı ve bağlantıların gezildiği test edildi.
Bir hikâye akışı kusuru (tek türe boğulma) ölçülüp düzeltildi.

### 10.4 Arazi

Seed'li yükseklik ve nem gürültüsünden ova, orman, tepe, dağ; üç nehir; başkent çevresinde verimli vadi; verime
ve mevsime bağlı yiyecek yeri; araziye göre hareket hızı; şehir çevresi verimine bağlı taşıma kapasitesi; verimli yere
yönelen dolaşma. Ölçüm (4 seed, 85 yıl): yiyecek yoğunluğu ile verim arasında r=0.38-0.57; şehir çevresi verim
çarpanı 1.02-1.38. **İnsan dağılımı ile verim arasındaki ilişki zayıf** (r=0.0-0.23): yerleşimler sabit başkent ve
şehirlerin çevresinde toplanıyor; arazi yiyeceği ve kapasiteyi belirliyor, yerleşim desenini zayıf belirliyor.

### 10.5 Makro yeniden ayar

Demografi sonrası siyaset durgunlaştı: geç dönemde isyan hiç çıkmıyordu. Teşhis: huzursuzluk oranı çocukları
paydaya katıyordu (nüfusun %35-40'ı çocuk) ve bölünme eşiği (%28) çocuk azlığıyla ayarlıydı; meşruiyet 85 yıl tabanda
kalırken yetişkinlerin yalnızca %12-16'sı isyan ediyordu. Huzursuzluk yetişkinler üzerinden hesaplandı, eşik %14'e çekildi.

| 8 seed × 15.000 adım | Geç dönem isyan | Ortalama canlı krallık |
|---|---|---|
| Öncesi | 0 (toplam) | 2.48 |
| Sonrası | 59 (toplam), 8 seed'in hepsinde ≥4 | 4.22 |

### 10.6 Savaş: bağımsız gözden geçirmenin bulduğu hata

İlk savaş turunda savaş ölümlerinin toplam ölümler içindeki payı ortalama %24'tü (%13.6-34.8) ve bunu "baskın ve
yağma modellenmedi" diye açıklamıştım. **Bu teşhis yanlıştı.** Bağımsız gözden geçirme kodu ve 3 seed'i inceleyip asıl
nedeni buldu: `enemy` seçimi yalnızca `o.tribe !== a.tribe` koşuluna bakıyordu; savaş, ittifak veya ticaret durumu hiç
kontrol edilmiyordu. Askerler barış ve ittifak dönemlerinde de öldürüyordu (askerlerin öldürmelerinin ~%90'ı savaşta
olmayan devletler arasında).

Düzeltme: düşman yalnızca savaştaki devletin bireyi; silahlılar tercih ediliyor; çocuklar hedef değil; yaşlı askerler
emekli oluyor. Bağımsız turlar devletler arası savaş-dışı öldürmeyi toplam ölümün %0.1-0.4 (tur 2) ve %0.2-1.7 (tur 3) ölçtü (ittifak 0); fark, gecikmiş yara ölümlerinin ayrılıp ayrılmamasından olabilir. Aynı devletten şiddetli ölümler (kan davası) şiddetli ölümlerin %20-31'i. Savaşın toplam
ölümlerdeki payı seed'e göre çok değişken: benim 4 seed'imde %5.4-10.2, bağımsız turlarda %1.6-14 (tur 3: şiddet %3.0-10.0, fetih ek %0.8-3.8). Tek bir aralık verilemez.
Kalan savaş-dışı ve devlet-içi öldürmeler büyük olasılıkla intikam avları (kan davası); bu doğrulanmadı.

Denenip geri alınan: barış zamanı asker payını düşürmek (Epstein kuralında asker sayısı baskıyı belirlediği için
isyan 177'ye, savaş 530'a fırladı). Sefer mevsimi (kışın yürüyüş yok), çarpışma hasarı 4→2.2, fetihte öldürme %20→%8
ve yaralı asker geri çekilmesi korundu.

### 10.7 Ekonomi

Yiyeceğin haritada hemen her zaman tavanda (~3000) durduğu ve açlığın ihmal edilebilir olduğu bulgusu üzerine üretim
ölçeği 0.35 → 0.22 → 0.17'ye düşürüldü. Sonuç: açlık ölümleri seed ve koşu uzunluğuna göre **toplam ölümün %0.2-6.0'ı**
(benim ölçümüm %0.2-2.8; bağımsız tur 3: %0.2-4.7; bağımsız tur 4: %0.7-6.0, 250 yıllık koşularda %4.7-6.0).

**Nüfusu belirleyen ağırlıkla sabit sayı:** `totalCap=700` doğumu, sayı 595'ten itibaren yumuşakça (son %15'te) azaltıyor.
Bağımsız tur 4 bunu iki yoldan gösterdi: 6 seed'in hepsi yaklaşık yıl 100'de 537-690'da platoya oturuyor; `totalCap`
2000 yapılınca aynı seed'lerde nüfus 762-960'a çıkıyor, Malthus terimi (kişi/K ≈ 0.3-0.4) neredeyse hiç devreye girmiyor.
Önceki notumdaki "tavana dayanma yok" ifadesi **yanlıştı**, geri çekildi. "Nüfusu yiyecek/arazi sınırlıyor" iddiası da
geri çekildi: ekonomi zayıf bir kısıt, ticaret ve kaynak modeli yok.

### 10.8 Bağımsız gözden geçirme turları

| Boyut | Tur 1 | Tur 2 | Tur 3 |
|---|---|---|---|
| Demografi | 7 | 7 | 7.5 |
| Mekân ve ekonomi | 5 | 4.5 | 4.5 |
| Toplumsal yapı | 7 | 7 | 7 |
| Birey zihni | 6 | 5.5 | 5 |
| Savaş ve siyaset | 3.5 | 6 | 5.5 |
| Okunabilirlik ve his | 7 | 7 | 6.5 |
| Doğrulama ve dürüstlük | 5.5 | 6 | 7 |
| Tekrarlanabilirlik ve kararlılık | 8.5 | 8 | 8 |
| **Ortalama** | **6.2** | **6.4** | **6.4** |

Üç tur da "talepkâr bir gözden geçirme 8 vermez" sonucuna vardı. Tekrarlayan bulgular: ekonomi zayıf, bireyler rolleriyle
tanımlı (kişilik etkisi ılımlı), siyaset seed'e göre ya donuyor ya kaos, varsayılan hız çok yüksek. Tur 3'ün öne çıkan
bulguları: roller birbirine benziyor (çiftçi/doktor/tüccar/casus ~%48 dolaşma + ~%38 arama), tavanda doğumlar aniden 0,
kadın askerler %21, siyaset uçlara ayrışıyor. Bunlara tur 4 ile yanıt verildi (10.11). Tur 3 puanı, tur 4 öncesindedir.

### 10.9 Tur 3 (tur 2 bulgularına yanıt)

| Değişiklik | Ölçüm (4 seed, 5000 adım, öncesi → sonrası) |
|---|---|
| Çocuk ebeveynin yakınında kalır | ebeveyne 70 birim yakın çocuk %28-35 → %96-98 |
| Hamile/emziren kadın eve yakın kalır, riskli işleri azaltır | emziren annenin şehre yakın olması: ölçülemedi (bayrak yoktu) → %90-98 |
| Kadınlarda askerlik eğilimi ×0.35 (tarihsel iş bölümü normu; `CFG.femSoldier`, 1 yapılırsa kapanır) | askerlerde kadın payı %42-53 → %7-27 |
| Kişilik eylem puanlarına katıldı (sorumluluk→toplama/teslim, kaygı→çarpışma, dışadönüklük→devriye, uyumluluk→iyileştirme, açıklık→dolaşma) | çiftçilerin dolaşma oranı standart sapması 0.144-0.157 → 0.151-0.195 (~%12); rol-içi davranış farkı (JSD) 0.15-0.21 → 0.17-0.19 (**değişmedi**) |
| Kardeşlere benzersiz ad | tur 2'de görülen çift ad giderildi (test edilmedi, koda bakıldı) |

Kişiliğin davranış üzerindeki etkisi **ılımlı** kaldı; bireyler hâlâ ağırlıklı olarak rolleriyle tanımlanıyor.

**Tur 3'te bulunan iki gerçek hata (yok olma):** tek bir seed'de tüm nüfus yok oldu.
1. Fethedeni olmadan çöken krallığın halkı sahipsiz kalıyordu ve ölü krallığa bağlı kadınlar doğum yapamıyordu.
   Düzeltme: sahipsiz kalanlar en yakın canlı krallığa katılır; hiç krallık kalmadıysa mülteciler yenisini kurar.
2. Tek şehre inmiş, aşırı kalabalık bir krallıkta Malthus terimi doğurganlığı sıfıra indiriyordu (60 yıl doğum yok,
   yaş yapısında delik, çöküş). Düzeltme: doğurganlık alt sınırı 0.25, ölüm artışı üst sınırı ×1.8.

**Denge ayarı yolu (dürüst kayıt):** aşırı genişleme 0.40 + kıtlık 0.14 → siyaset aşırı çalkantılı (ortalama 7.4 krallık,
250 yılda 45-194 savaş, 20-100 darbe). 0.32 + 0.17 → yok olma hatası ortaya çıktı (parametre değil, yukarıdaki iki
hata). Hatalar düzeltilince son ayar 0.32/0.17'de kaldı.

### 10.10 Doğrulama (tur 3 sonrası, tur 4 öncesi)

12 seed × 17.500 adım (250 yıl): hata 0, nüfus yok olması 0, en düşük nüfus 121, ortalama canlı krallık 5.85,
sondaki canlı krallık (`pop>0` ve `!_dead`) sıralı: 1,1,1,1,2,3,5,5,5,6,7,9 (medyan 4), tüm seed'lerde geç dönem savaş
(15-172), isyan (5-55) ve fetih; darbe 1-65. Aynı seed aynı sonucu veriyor.

### 10.11 Tur 4 (tur 3 bulgularına yanıt)

| Değişiklik | Ölçüm |
|---|---|
| Rol-özel boşta işler (çiftçi: yiyecek arama; doktor: klinikte bekleme; tüccar: pazarda bekleme; casus: gölgede bekleme) | 3 seed, 5000 adım: doktorda en sık eylem "arama" %33 → "klinik" %27-32; çiftçide "yiyecek arama" %12-18 + "toplama" %9-12; casusta "gölgede bekleme"/"casusluk" %13-19; tüccarda "ticaret seferi" %28-32. Dolaşma+arama payı yaklaşık %65-85 → %40-55 |
| Kadın askerlik eğilimi ×0.1 (`femSoldier`) | askerlerde kadın payı %16 → %1-3 |
| Doğum tavanı sert kesme yerine son %15'te yumuşak azalma | nüfus 467-622'de, tavana dayanma yok |
| Ayrılıkçı devlete asgari boyut (ana devlet ≥40 nüfus, ≥10 kurucu) ve iki-devletli dünyada bölünme eşiği ×0.6 | 16 seed × 250 yıl: sondaki canlı krallık 2-10 (medyan 5); tek devlete inen seed 0 (öncesi 12'de 4) |
| Varsayılan hız 3× → 1× | (arayüz; ölçülmedi) |

### 10.12 Son doğrulama (tur 4 sonrası)

16 seed (12 kendi seçtiğim + tur 3 gözden geçiricisinin `falcon`, `pixel9`, `zebra`, `mango` seed'leri) × 17.500 adım
(250 yıl): hata 0, nüfus yok olması 0, en düşük nüfus 137, ortalama canlı krallık 6.38, sondaki canlı krallık (`pop>0`
ve `!_dead`) sıralı: 2,2,3,4,4,5,5,5,5,5,6,7,7,7,10,10 (medyan 5), geç dönem savaş 24-180 (bir seed <25, beş seed >150),
isyan 13-60. En çalkantılı seed'lerde 10 krallık tavanı (`maxKingdoms`) bağlayıcı. Demografi (4 seed × 120 yıl):
beklenen ömür 35.0-37.9, 15'e ulaşma %61-66, doğurganlık 4.1-5.1, savaş payı %5.9-10.2, açlık %0.2-2.8.



### 10.13 Bağımsız tur 4 (6.3/10) ve bulunan doğruluk hatası

Puanlar: demografi 7.5, mekân/ekonomi 4.5, toplumsal yapı 7, birey zihni 5, savaş/siyaset 5, okunabilirlik 6.5, doğrulama
6.5, tekrarlanabilirlik 8.5. **Ortalama 6.3; "8 verir mi" cevabı yine hayır.** Dört turun puanları: 6.2, 6.4, 6.4, 6.3
(gözden geçirme gürültüsü içinde sabit).

**Bulunan gerçek hata:** `_killer` hiç sıfırlanmıyordu. Vurulup iyileşen biri yıllar sonra yaşlılık veya hastalıktan ölünce
yine eski saldırgana yazılıyordu: bağımsız ölçümde şiddetli ölümlerin %35-45'i yalan; bu, `kills`, "…'in elinde öldü"
hikâyeleri, akrabaların nefreti ve intikam hedeflerini şişiriyordu. Düzeltme: vuruşa zaman damgası; ölümde yalnızca son 40
adım içindeki bir vuruş ve doğal/doğum ölümü olmayan durum "öldürme" sayılır. Doğrulama (aynı seed): atfedilen öldürme
toplamı 20 → 13. **Bu düzeltmeden önce yazılan tüm savaş payı, `kills` ve "şiddetli ölüm" sayıları (§10.6, §10.7, §10.10,
§10.12) şişirilmiş atıf içeriyordu; güncel sayılar §10.14'te.**

Diğer bulgular ve durumları: cinsiyet-rol ayrışması aşırı (asker yalnızca erkek, doktor/casus/tüccar çoğunlukla kadın;
`femSoldier` bir modelleme kararı) → sınır olarak kaldı; çocuklar oyun/çıraklık yapmıyor, yaşlılar yetişkinle aynı dağılımda
→ sınır olarak kaldı; sürekli savaş (her seed'de yılların %77-93'ünde aktif savaş) ve mikro-krallıklar → sınır olarak kaldı;
hikâyeler evlilik/doğum ağırlıklı → kaldı; çift simge (Vakayiname) ve canlı bireyin geçmiş zamanlı özeti → **düzeltildi**.

### 10.14 Doğrulama (`_killer` düzeltmesi sonrası)

**Demografi** (`_killer` düzeltmesinden sonra, 4 seed × 120 yıl; bölünme kalibrasyonundan önce, o ayar siyaseti etkiler):
beklenen ömür 35.6-37.9, bebek ölümü %16.5-17.3, 15'e ulaşma %63-64, toplam doğurganlık 4.3-4.9, 15-45 yaş kadınlardan
evli %75-84, **savaş ölümü payı %3.5-7.6** (düzeltme öncesi %5.9-10.2: yani şişme gerçekmiş), açlık ölümü %1.1-4.9.

**Siyaset** (16 seed × 250 yıl: 12 kendi seçtiğim + `falcon`, `pixel9`, `zebra`, `mango`), iki adım:

| Ayar | Ortalama canlı krallık | Sondaki canlı krallık (sıralı) | Not |
|---|---|---|---|
| `_killer` düzeltmesi, bölünme asgarisi 40 nüfus/10 kurucu/6 süre | 7.28 | 2,2,6,6,7,7,8,8,8,8,8,9,9,10,10,10 (medyan 8) | aşırı parçalı, 6 seed 9-10'luk tavana dayanıyor |
| **Son:** asgari 70 nüfus, 14 kurucu, 8 süre | **4.63** | 1,2,2,3,3,3,3,3,3,3,4,4,4,5,6,6 (**medyan 3**) | dengeli |

Son ayar, 16 seed × 250 yıl: hata 0, nüfus yok olması 0, en düşük nüfus 137, geç dönem savaş 37-121, isyan 13-44, darbe 0-22,
hiçbir seed 10 krallık tavanına dayanmıyor, hiçbir seed donmuyor (en sessiz seed'de bile 37 savaş), bir seed sonda tek devlete
iniyor. Aynı seed aynı sonucu veriyor.

**Bu son iki ayar (`_killer` düzeltmesi ve bölünme kalibrasyonu) bağımsız bir turdan geçmedi;** puanlar (6.2, 6.4, 6.4, 6.3)
bunlardan önceki sürümlere aittir.

### 10.15 Kalan zayıflıkların sırayla düzeltilmesi (tur 5)

Bağımsız turların tekrarlayan zayıflıkları tek tek ele alındı; her adım ölçümle kapatıldı.

**1. Ekonomi: gerçek kök neden enerji sızıntısıydı.** Yiyecek üretimi (~0.34/adım) talebin (~0.8/adım) çok altındayken nüfus
ayakta duruyordu; sebep "bedava enerji" kaynaklarıydı: teslim eden çiftçiye +5, sefer bitiren tüccara +6 enerji, iyileştirirken
doktora −0.02 (hastaya +0.9: enerji yaratıyordu), öldürene +22. Bunlar kaldırıldı/küçültüldü (teslimde ve seferde 0, iyileştirme
doktora 0.6, ganimet enerjisi 8): kapalı bir enerji hesabı. Ardından üretim ölçeği yeniden ayarlandı:

| foodScale | Nüfus (son 30 yıl) | Açlık ölümü payı |
|---|---|---|
| 0.18 (seçilen) | 600-702 | %3.7-8.4 |
| 0.26 | 671-902 | %0-0.8 (yiyecek sınırlamıyor) |
| 0.36 | 695-967 | %0-0.3 |

Gözden geçiricinin karşı-testi (tavanı 2500 yapmak) bu sürümde: nüfus farkı **%0.0** (4/4 seed), yani nüfusu artık sabit tavan
değil yiyecek belirliyor (tavan performans emniyeti olarak 1100'e yükseltildi). Tahıl ticareti: tüccar ortağın en dolu ambarından
fazla yiyeceği kendi başkentine taşır; 120 yılda 265-860 birim (toplam talebe göre küçük, bu yüzden etkisi **ılımlı**; ticaret
açık/kapalı karşılaştırması seed'ler arası tutarsız çıktı, yani kanıtlanmış bir fayda iddiasında bulunmuyorum).
Bu, §10.7'deki "ekonomi zayıf bir kısıt" bulgusunun çözümüdür; o bölüm geçmiş durumu anlatır.

**2. Bireysel davranış (4 seed, 5000 adım, öncesi → sonrası):** 8+ yaş çocuklar aileye yardım eder (yiyecek toplayıp taşır):
eylemlerin %0 → %5-10'u; yaşlılar hafif iş görür ve ocak başında oturur: %0 → %9-13; yaşlı ve yetişkin çiftçilerin davranış
mesafesi (JSD) 0.08-0.12 → 0.14-0.17; meslek mirası (ebeveynin işi daha cazip, çarpan 3.0): yetişkinin ebeveyniyle aynı meslekte
olma oranı %29-39 → %42-49 (rastgele düzey ≈%35; etki pazar doygunluğu tarafından sınırlanıyor, tasarım gereği).

**3. Baskın ve yağma:** asker düşman şehre ulaşınca ambardan 2 birim yiyecek yağmalar ve eve taşır (ganimet düşmanın ambarından
eksilir; çiftçi verimliliği çarpanı uygulanmaz). Büyük bir yan etki bulundu: yağma siyaseti kaotikleştirdi (8 seed × 250 yıl,
ortalama canlı krallık): yağma kapalı 6.4, %8 → 7.7, %30 → 7.8. Sebep: yağmalanan ambar boşalır, o devlet "çaresiz" savaşa girer
(zincirleme). Çözüm: yalnızca güvenli stokun (60) üstündeki ambarlar yağmalanır, olasılık %8; bölünme asgarisi 70/14 → 90/18.
Sonuç: 7.12 (yalnızca güvenlik) → **6.19** (8 seed). 120 yılda yağma 136-272 birim. Yağma bir ekonomik mekanizma ve hikâye
kaynağı olarak duruyor, ama **siyaseti kuvvetle besleyen bir etken; bu yüzden sınırlandırıldı.**

**4. Hikâye çeşitliliği:** yeni türler: yağma, kıtlık (yılda ≥5 açlık ölümü), salgın (yılda ≥5 ölüm), çıraklık (çocuk ebeveyninin
mesleğini seçti). Yıllık akışta (son 40 hikâye) 8-9 tür görünüyor (öncesi ağırlıkla evlilik/doğum).

**5. Kaynak doğrulaması:** yeniden denendi; arxiv.org, ourworldindata.org, api.crossref.org, api.semanticscholar.org dahil tüm
akademik alan adları ağ politikasıyla kapalı. **Düzeltilemedi**; çözüm ortam ayarında ağ erişimini genişletmek (ortamı
yapılandıran kişide). SOURCES.md'deki B düzeyleri değişmedi.

**Son doğrulama (tüm düzeltmeler açık):** 16 seed (12 kendi seçtiğim + `falcon`, `pixel9`, `zebra`, `mango`) × 17.500 adım
(250 yıl): hata 0, nüfus yok olması 0, en düşük nüfus 147, ortalama canlı krallık 6.27, sondaki canlı krallık sıralı
3,4,5,5,5,5,5,6,6,6,7,7,8,9,9,10 (medyan 6; yalnızca 1 seed 10'luk tavanda), geç dönem savaş 74-169, isyan 30-57, darbe 4-22,
nüfus 411-681. Demografi (4 seed × 120 yıl): beklenen ömür 34.6-37.4, bebek ölümü %16.3-17, 15'e ulaşma %64-65, doğurganlık
4.5-5.1, evli kadın %77-80, açlık ölümü %1.9-9.7, savaş ölümü %6.1-10.5. **Bu tur (5) bağımsız bir turdan geçmedi.**

## 10.16 Tur 6: bağımsız değerlendirme 5 ve düzeltmeler

Kör değerlendirme (tur 5 kodu): ortalama **6.1** (demografi 7, mekân/ekonomi 5.5, toplum 6, birey zihni 5, savaş/siyaset 5, okunabilirlik 7, doğrulama 5, tekrarlanabilirlik 8). Hedef ≥8 hâlâ karşılanmadı.

Yapılan düzeltmeler ve ölçümler (8 seed × 250 yıl, hata 0, yok oluş 0, en düşük nüfus 149):
- Hükümdar tutarlılığı: isyanda eski hükümdar yeni krallığı kurarsa eski tahtı boşaltıp veraset başlıyor; her 50 tickte ölü/başka krallıktaki hükümdar temizleniyor. Bir seed'de tutarsız kayıt %24 → %0,7.
- Kişilik → seçim (`CFG.persBehav`): aynı rol içinde çalışkanlık~çalışma/boş payı |r| 0,6-0,77, meraklılık~gezinme 0,2-0,8, sosyallik~buluşma 0,4-0,5. Savaş/risk korelasyonu zayıf (eylem nadir). Bu bağ tasarım gereği kurulan bir kural; "kişilik davranışı belirler" iddiasının ispatı değil, kuralın çalıştığının ölçümü.
- Ekonomi: hekim bakımı ölüm riskini azaltıyor (`healWin`, `healProt`); hekim/tüccar hedef oranları yüke göre kayıyor; eksi ambar hatası giderildi. Hâlâ yok: ekim-hasat, hane mülkiyeti, bireysel servet.
- Makro siyaset etkilenmedi: ortalama yaşayan krallık 6,27 (öncesi 6,27), geç savaş 56-145.
- Arayüz: Ölçümler paneli (örnek seed, 80. yıl: beklenen ömür 36,6, 15'e ulaşma %66, doğurganlık 5,2). Panel dönem yaşam tablosu kullanıyor; "ortalama ölüm yaşı" büyüyen nüfusta beklenen ömürden çok düşük çıktığı (21,6) için kullanılmadı.
- Türkçe ek uyumu (`ek()`).

Açık kalanlar: köy/hane/gün döngüsü (1 tick ≈ 5 gün olduğu için gün döngüsü anlamlı değil), ekim-hasat ve mülkiyet, gerçek savaş taktiği, ihanet zinciri, isyan parçalanması, birincil kaynak doğrulaması (ağ engeli), nüfus üst sınırı (performans), yapılan değişikliklerin bağımsız yeniden değerlendirmesi.

## 11. Bilinen sınırlar

- Savaş: yalnızca savaştaki devletlere saldırılıyor ve asker ambar yağmalıyor; seferberlik ve gerçek baskın taktiği yok. Yağma
  siyaseti kaotikleştirdiği için sınırlandı (§10.15). %20-31 şiddetli ölüm aynı devlet içinde (kan davası).
- Ekonomi sade: tek kaynak (yiyecek); nüfusu artık yiyecek belirliyor (tavan karşı-testi %0 fark, açlık ölümü %1.9-9.7) ama tahıl
  ticareti küçük (120 yılda 265-860 birim), para/zanaat/mülkiyet modeli yok.
- Bireyler hâlâ ağırlıklı olarak rolleriyle davranıyor; kişilik/cinsiyet/yaş etkisi ılımlı (çocuk yardımı, yaşlı ocağı ve meslek
  mirası eklendi, §10.15). Cinsiyete dayalı askerlik normu bir modelleme kararı, ayarlanabilir; rol-cinsiyet ayrışması aşırı (asker
  neredeyse yalnızca erkek, doktor/casus/tüccar çoğunlukla kadın).
- Köy, hane ve günlük döngü yok; yerleşim sabit şehirlerde toplanıyor, arazi insan dağılımını zayıf belirliyor (r=0.0-0.23).
- Siyasetin canlılığı seed'e çok bağlı (savaş 74-169); çok sayıda 1-3 şehirlik mikro-krallık kurulup ölüyor; ortalama canlı krallık
  (6.3) hedeflediğimden (4-6) yüksek.
- Beklenen ömür hedef aralığın üst ucunda; hedef aralıkların gerekçesi zayıf (SOURCES.md, çoğu düzey B) ve birincil kaynaklar ağ
  politikası yüzünden açılamıyor.
- Eşikler (isyan, rejim yaşı, kuraklık, doğurganlık, kıtlık, yağma, bölünme asgarileri) tek parametreli süpürmelerle seçildi,
  duyarlılık analizi yok.
- Akraba evliliği yalnızca birinci derece ve kardeşler için engelleniyor.
- LLM anlatı katmanı yok; hikâyeler şablon tabanlı, davranış kural tabanlı.
- Bireyin hafızası 10 yuvalı ve az çeşitli; hedefler 4 türle sınırlı.
- Hız: 1.8-2.5 ms/adım (küçük nüfus), ~7.5 ms/adım (700 kişi, tek süreç), en kötü tek adım 32-510 ms; ölçümler Node `vm` +
  Chromium düzeneğinde, gerçek cihaz başarımı ölçülmedi.
- Hikâyelerde "Yıl 13: casus oldu" gibi satırlar takvim yılıdır (yaş değil); bir kusur değil, ama okuyanı yanıltabilir.
- Gözden geçiricinin "sayfa açılınca İncele paneli boş" bulgusu bu ortamda **tekrarlanamadı**.
