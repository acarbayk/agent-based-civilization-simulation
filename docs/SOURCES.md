# Kaynak kaydı ve doğrulama

Projede kullanılan her kaynak için: ne iddia için kullanıldığı, güvenilirlik türü ve **nasıl doğrulandığı**.

**Doğrulama düzeyleri**

- **A**: Tam metin okundu (kullanıcının verdiği dosyalar).
- **B**: Künye (yazar, yıl, yayın yeri) ve ana iddia birden çok bağımsız arama sonucuyla doğrulandı. Birincil sayfa
  açılamadı: bu oturumun ağ politikası `WebFetch`'i bu alan adlarına kapatıyor (ourworldindata.org, pnas.org,
  arxiv.org, journals.uchicago.edu, jasss.soc.surrey.ac.uk, wikipedia.org, ...). Bu yüzden **B, sayfanın
  kendisini okuduğum anlamına gelmez.** Sayısal iddialar için bu bir sınırdır.
- **C**: Yalnızca arama özeti veya ikincil kaynak. Sayı vermek için kullanılmaz.
- **X**: Doğrulanamadı; kullanımdan kaldırıldı.

**Güvenilirlik türü:** H = hakemli dergi/kitap bölümü, K = kitap, P = ön baskı (hakemsiz olabilir),
E = uzman/endüstri yazısı, W = topluluk vikisi.

| # | Kaynak | Tür | Ne için kullanıldı | Düzey | Notlar / düzeltme |
|---|---|---|---|---|---|
| 1 | Epstein (2002), *Modeling civil violence*, PNAS 99(suppl 3) | H | İsyan kuralı: `G=H(1−L)`, `P=1−exp(−k·C/A)`, `k=2.3`, `T=0.1` | **A** | PDF'ten okundu. Eleştiri: Thron & Jackson (2015), arXiv:1501.05838 (B): model olgusal bir betimleme, kuramsal açıklama değil |
| 2 | Millington & Funge (2009), *AI for Games* 2. baskı | K | GOB, *insistence*, AI LOD | **A** | §5.7 ve §9.3 okundu. 3. baskı (2019, Millington) var; farkları doğrulanamadı |
| 3 | Swink (2008), *Game Feel* | K | Kullanılmadı (ilgisiz) | B (yalnız künye) | İçeriği okunmadı |
| 4 | Kokkonen & Sundell (2014), APSR 108(2):438-453 | H | Primogeniture hükümdarın görevde kalma olasılığını 2 kattan fazla artırıyor (960 hükümdar, 42 devlet, 1000-1800) | B | **Wikipedia bağlantılarının yerine geçti (W → H)** |
| 5 | Falk & Hildebolt (2017), Current Anthropology 58(6):805-813 | H | Savaş ölümlerinin nüfusa oranı nüfus büyüdükçe azalır; mutlak sayı artar | B | **Önceki belgede yanlış iddiaya bağlanmıştı** ("%10-40, çoğu baskın"); düzeltildi |
| 6 | Oka ve ark. (2017), PNAS 114(52):E11101 | H | Savaş grubu büyüklüğü ve kayıplar nüfusla ölçeklenir | B | Yazarlar önceki notta yanlış anılmıştı |
| 7 | Volk & Atkinson (2013), Evol. Hum. Behav. 34; OWID özeti (Ritchie & Roser) | H + E | Tarihsel çocuk ölümü: 17 avcı-toplayıcı toplumda ortalama %49 | B | OWID sayfası açılamadı, arama özetiyle doğrulandı. Önceki "15'e ulaşma %69" İngiltere'ye özgü, iyimser uç |
| 8 | Clark (2007), *A Farewell to Alms*, böl. 5 | K | Beklenen ömür 1800'e dek avcı-toplayıcılardan yüksek değil: 30-35 yıl | B | Sayısal tablolar doğrulanamadı |
| 9 | Bowles (2009), Science 324:1293 | H | Avcı-toplayıcı savaş ölümü | **X (sayı)** | "%14" rakamı doğrulanamadı; alan-bazlı tahminler geniş aralıkta (özet: %8-30). Sayı kullanılmıyor |
| 10 | Axtell ve ark. (2002), PNAS; Janssen (2009), JASSS 12(4):13 | H | Hane tabanlı, arkeolojik veriyle karşılaştırılan model | B | **Janssen: uyumun büyük kısmı çevresel taşıma kapasitesinden geliyor, ajan dinamiklerinden değil.** Eklendi |
| 11 | Park ve ark. (2023), *Generative Agents*, UIST (arXiv:2304.03442) | H | Hafıza akışı, yansıma, plan; maliyet: 25 ajan × 2 gün = binlerce dolar | B | Maliyet cümlesi makalenin sınırlılıklar bölümünde, çoklu kaynak doğruladı |
| 12 | Anderson & Schooler (1991); Taatgen, Lebiere, Anderson (ACT-R kaynak kitapçığı) | H | `B = ln Σ t^-d`, `d=0.5` varsayılan | B | arXiv:1306.0125 bir ACT-R tanıtımı (P); birincil kaynak Anderson & Schooler |
| 13 | MacCarron, Kaski, Dunbar (2016), Social Networks 47:151-155 | H | İlişki katmanları | B | **Ölçülen katmanlar 4.1 / 11 / 30 / 129 (kat ≈2.7)**; "5-15-50-150" Dunbar'ın kuramsal değeri, kesin değil |
| 14 | Turchin & Gavrilets (2009), Social Evolution & History 8(2) | H | Asabiya, savaşın karmaşıklığı seçmesi | B | |
| 15 | Turchin (2009), *A theory for formation of large empires*, J. Global History | H | Büyük imparatorlukta çekirdeğin bağı gevşer | B | |
| 16 | Tainter (1988), *The Collapse of Complex Societies* | K | Azalan verim | C | Yalnızca özet düzeyinde |
| 17 | Hermann (1999), *Assessing Leadership Style* (LTA) | H/E | Lider kişiliği ve dış politika | B | **Bizim lider eşlememiz Big Five'dan, LTA'dan türetilmedi**; LTA 7 farklı özellik kullanır |
| 18 | Hills & Todd (2008), JASSS 11(4):5 (MADAM); Todd, Billari, Simão (2005), Demography 42:559 | H | Benzerlik ve yaşla gevşeyen beklenti ile eş bulma | B | |
| 19 | Adams (2019), *Emergent Narrative in Dwarf Fortress*, Procedural Storytelling in Game Design (CRC) | K | Hikâyenin görünür kılınması, karakter özellikleri | B | Wiki sayfaları (W) yalnızca oyun mekaniğini gösterir, bilimsel kanıt değildir |
| 20 | Zubek (2010), *Needs-based AI*, Game Programming Gems 8, s. 302-311 | E | İhtiyaç tabanlı yapay zekâ | B | |
| 21 | Ettinger, Mulberry32 gisti; bryc/code tartışması | E | Seed'li RNG | B | **Yazar 2022'de artık önermiyor**, tüm 32-bit değerleri üretmiyor. `sfc32`'ye geçildi |
| 22 | agentpy belgeleri; *Taming randomness in ABMs using CRN* (arXiv:2409.02086) | E + P | Rastgele akışları ayırma | B | |
| 23 | *Learning to Make Friends* (arXiv:2510.19299) | P | LLM ajanlarda kişilik ve bağ | C | Ön baskı, LLM tabanlı; bizim kural tabanlı modelimiz için yalnızca ilham |
| 24 | *Affordable Generative Agents* (arXiv:2402.02053) | P | LLM ajan maliyeti | B | |
| 25 | Crusader Kings II tasarım yazısı (Game Developer) | E | Tek yönlü "görüş" sistemi | C | Endüstri yazısı; mekanik fikir için, sayı için değil |

## Bilinen doğrulama açığı

Birincil sayfaların çoğu bu oturumdan açılamadı. Sayısal aralıklar (bebek ölümü, beklenen ömür, savaş ölümü)
bu yüzden **hedef aralık** olarak kullanılıyor, kesin değer olarak değil. Ağ erişimi açılırsa (ortam ayarlarında
ağ politikası) ilk iş #5, #7, #8, #10, #13'ün birincil metinlerini okumak olmalı.
