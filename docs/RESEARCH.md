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
doğum aralığı, doğumda ölüm, eş eşleştirmesi (yılda iki kez), kurucu aileler. 8 seed × 120 yıl:

| Ölçüt | Sonuç | Hedef aralık (SOURCES.md #7, #8) |
|---|---|---|
| Doğumda beklenen ömür | 27.7-34.1 | 28-38 (bir seed sınırın hemen altında) |
| Bebek ölümü | %17-19 | %13-25 |
| 15 yaşa ulaşma | %57-65 | %50-70 |
| Toplam doğurganlık | 4.8-6.6 | ön sanayi evli-doğal doğurganlık ≈5-7 (kesin kaynak doğrulanamadı) |
| 15-45 yaş kadınlardan evli | %74-85 | yüksek (tarihsel olarak evrensele yakın; kesin sayı doğrulanamadı) |

Not: ilk denemede evli oranı %33-41'de kaldı (toplam doğurganlık 3.1-4.1) ve nüfus 4 seed'in 3'ünde eriyordu; eşleştirme
eklenince doğurganlık aşırı yükseldi (5.3-7.4) ve ayar iki turda yapıldı.

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

### 10.6 Savaş: negatif sonuç dahil

- Sefer mevsimi eklendi (askerler kışın yürümez).
- Savaş ölümlerinin toplam ölümler içindeki payı %21-37 çıktı (hedefim ≤%15). Çarpışma hasarı 4→2.2, fetihte öldürme
  %20→%8, yaralı asker geri çekilme eklendi: pay ortalama %24'e indi (aralık %13.6-34.8).
- **Denenen ve geri alınan:** barış zamanı asker payını düşürmek. Epstein kuralında asker sayısı baskıyı belirlediği
  için isyan 177'ye, savaş 530'a fırladı, ortalama 8 krallık, kaotik dünya; savaş payı da düşmedi. Geri alındı.
- Sonuç: **savaş ölüm payı hedefi tutmadı.** Devlet düzeyinde bir toplum için hâlâ yüksek olduğunu düşünüyorum; ancak
  tarihsel referans sayıyı doğrulayamadım (SOURCES.md #9), bu yüzden "yüksek" yargım bir varsayım.

### 10.7 Son doğrulama

12 seed × 15.000 adım (214 yıl): hata 0, nüfus yok olması 0, ortalama canlı krallık 4.89, sondaki canlı krallık
1-7 (medyan 3), tüm seed'lerde geç dönem savaş (13-61), isyan (3-24) ve fetih var. En düşük nüfus 147. Aynı seed aynı
sonucu veriyor.

## 11. Bilinen sınırlar

- Savaş ölüm payı (ortalama %24) yüksek; baskın, yağma ve seferberlik modellenmedi (savaş "asker şehre yürür" biçiminde).
- Köy, hane ve günlük döngü yok; yerleşim sabit şehirlerde toplanıyor, arazi insan dağılımını zayıf belirliyor.
- Bazı seed'lerde dünya sonunda tek devlete iniyor (12'de 1).
- Eşikler (isyan, rejim yaşı, kuraklık, doğurganlık) tek parametreli süpürmelerle seçildi, duyarlılık analizi yok.
- Sayısal hedef aralıkların birincil kaynakları bu oturumdan açılamadı (SOURCES.md).
- Akraba evliliği yalnızca birinci derece ve kardeşler için engelleniyor.
- LLM anlatı katmanı yok; hikâyeler şablon tabanlı, davranış kural tabanlı.
- Ölçümler Node `vm` + Chromium düzeneğinde; gerçek cihaz başarımı ölçülmedi.
