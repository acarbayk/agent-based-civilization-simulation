# Changelog

## Unreleased

### Oyun testi düzeltmeleri (bağımsız acemi oyuncu ajanı: genel 5,5/10)
- Danışman kartı gelince oyun **otomatik durur**, Danışman sekmesi açılır; cevaplayınca devam eder. Yönetmen kamerası kart açıkken sahne değiştirmez (sekme sıçraması giderildi)
- Kart tekrarı azaldı: aynı kart 12 yıl boyunca ağırlığı %12'ye düşer; "Savaş sürüyor" kartı 2 yerine 5 yılda bir; "Hazine taşıyor" daha seyrek
- İlahi eller: puan yılda +6 → +2,5, başlangıç 50; maliyetler 30/35/35/45/60/50; deprem daha ölümcül ve şehirlere daha çok zarar; yıldırım kimi vurduğunu (krallık, hükümdar mı) söyler; kullanılan yerde 1,8 sn'lik halka + simge efekti
- Tura "İlahi eller" adımı eklendi; sekme değişince sağ panel başa kayar (Dünya/Ayarlar kayma sorunu)
- Anlatı kalıpları genişledi (keşif, gelenek, şehir kurma 10'ar tat cümlesi; tahta çıkış için ek cümleler)
- Bilinen kalanlar: etki hissi (çarpanlar görünmez), şans yüzdeleri kartlarda yok, hazine şişince maliyetlerin ölçeklenmemesi, Tarih grafiği etiket çakışması; gerçek insan testi hâlâ yapılmadı

### Anlatı çeşitliliği
- Olay metinlerine durumdan seçilen "tat" cümleleri eklendi (ticaret, ittifak, salgın, yeni şehir, keşif, gelenek, savaş, barış, bereketli hasat, tahta çıkış biçimine göre not); seçim deterministik (rastgele sayı tüketmez), mevcut anahtar sözcükler (savaş açtı, salgın başladı, tahtına çıktı...) korunur
- Yeni yıllık minik hikâyeler: **dostluk** (karşılıklı güçlü bağ, akraba olmayan), **husumet** (karşılıklı derin nefret), **varis** (yaşlanan hükümdarın vârisi ve mizacı), **alamet** (kuyruklu yıldız, güneş tutulması, taş yağmuru... halk yorumlar)
- Ölçüm (3 tohum × 100 yıl, 2055 günlük kaydı): benzersiz metin oranı %89 (önce 80 yılda 22 kalıp, olayların yarısı iki kalıptı)

### İlahi eller (oyuncu müdahalesi)
- **Dünya** sekmesinde "İlahi eller": inanç puanı (başta 60, yılda +6, en çok 100) harcayarak haritada tıklanan yere müdahale: 🌧 Yağmur (20; yiyecek yağar, kuraklığı bitirir), ✚ Şifa (25; yakındakileri iyileştirir, salgını söndürür), ✨ İlham (25; krallığa araştırma, meşruiyet, moral), ⚡ Yıldırım (30; en yakın kişiyi öldürür, hükümdar olursa veraset/siyaset zinciri tetiklenir), 🌋 Deprem (40; şehir surları ve ambarlar zarar görür, yaralı/ölü), ☠ Salgın (35; krallıkta salgın başlatır)
- Güç seç → haritada hedefe tıkla (Esc iptal). Ölenlerin Tarih Kitabı'ndaki ölüm nedeni "tanrının gazabıyla öldü"; olaylar günlüğe ve yönetmen kamerasına düşer. Shift+tık yiyecek yağdırma bedava kalır
- Kullanılmadıkça benzetim değişmez (aynı seed aynı geçmiş)

### Okunabilirlik: varsayılan yakınlık
- Uzaktan bakışta (zoom <2,2×) kişi figürleri %50'ye kadar büyür, hükümdar ayrıca %12 daha büyük; yakınlaşınca normal boyuta döner (yalnız çizim, benzetim değişmedi)

### Savaş oyuncunun kararında + yavaş hız
- **Danışmanlı krallıkta savaşı oyuncu yönetir:** hükümdar artık kendi başına savaş/ihanet başlatmaz; hevesi olduğunda "Hükümdar savaş istiyor" kartı gelir (onay ver / ordu hazırla ve bekle / elçi gönder, vazgeçir). Barış da kartla yapılır; savaş başlayınca (biz ya da karşı taraf açsa da) "Savaş sürüyor" kartı hemen gelir ve her 2 yılda tekrarlanır. Yalnız çöküşte (nüfus <%40 / başkent surları <30) hükümdar zorla barışır. Diğer krallıklar eskisi gibi kendi savaşlarını açar
- **Yeni yavaş hızlar:** ¼× ve ½× kademeleri (1× ≈ 0,86 yıl/sn idi, danışman kararları için fazla hızlıydı); danışman olunca hız otomatik ½×'e iner, kart gelince hızlıysa 1×'e düşer; kart cevapsız kalma süresi 2→3 yıl; +/- tuşları kademeli
- Danışman olunca kamera krallığın başkentine yaklaşır (2×); yönetmen modu kapalıysa açılışta en büyük krallığın başkenti çerçevelenir

### Danışman modu (hedef: büyüme)
- **Danışman** sekmesi: bir krallığa hizmet et; hükümdar 4-6 yılda bir ikilem kartı getirir (15 kart: tarla, nüfus, ticaret, bilim, zayıf komşu, tehdit, vergi, kıtlık, salgın, huzursuzluk, şehir, göç, hazine, veraset, savaş); her kartta 2-3 seçenek, maliyet ve risk etiketi
- **Hükümdarın güveni:** tavsiyen ~%30-95 ihtimalle dinlenir (güven + hükümdarın uyumluluğu); iyi sonuç güveni artırır, kötü sonuç ve reddedilme düşürür; 2 yıl cevap vermezsen hükümdar kendi bildiğini yapar
- **Büyüme hedefi:** nüfus + 40·şehir + 3·toprak + 12·buluş indeksi, başlangıca göre rütbe (Beylik → Yükselen Krallık → Büyük Krallık → Bölge Gücü → İmparatorluk), dünya sıralaması, sparkline; danışmanlı krallık 6 şehre kadar büyüyebilir
- Seçimler gerçek benzetim durumunu değiştirir (hazine, hasat, doğurganlık, taşıma kapasitesi, bilim, ticaret, savunma, ordu, ittifak/ticaret/barış/savaş, şehir kurma, göç); her karar olay günlüğüne ve Tarih Kitabı'na düşer; haritada 🎩 işareti; yönetmen modu danışmanlı krallığın olaylarını önceler

### Hızlı başlangıç + Yönetmen modu
- **Hızlı başlangıç:** açılışta seed'li dünyanın ilk 30 yılı önceden oynanır (ilerleme çubuğu, "Atla"); ~600 kişilik değil, erken yıllar olduğu için ~12 sn; Ayarlar'dan 0/15/30/50 yıl seçilir. Aynı seed aynı geçmişi verir. Otomasyon tarayıcılarında kapalı (`?boot=N` ile açılır)
- **Yönetmen modu (🎬, kısayol D):** kamera olay puanına göre (fetih, isyan, ihanet, savaş ilanı, hanedan, salgın...) ilgili krallığa/kişiye gider, savaşçı kümelerini (≥5 kişi) bulur, aksi halde ilginç bir kişiyi izler, ara sıra genel bakış; alt yazı gerekçe ve olayı yazar; fare/tekerlek/mini harita kullanılınca 25 sn duraklar; `?director=0/1`
- Birey kartı isyan gerekçesi düzeltildi (hoşnutluk yerine meşruiyet ve geçim)

### Kullanılabilirlik testi 7 düzeltmeleri (ortalama 6,3)
- Sekme çubuğu 1366×768'de çöküyordu (8 px) → sabit yükseklik; yazı boyutu ayarı artık üst çubuk, çekmece, olay akışı, izleme listesi, harita düğmeleri ve pencerelere de uygulanır; yüksek kontrast paketi genişletildi; panel metinleri en az 11,5 px
- Birey kartı: isyancıda "isyanda · yapılan iş" ve isyan gerekçesi; hükümdarda krallığın tutumu ve hükümdarlık süresi; yinelenen özellik rozetleri kaldırıldı
- Savaş kayıtları: **Savaşlar** sekmesi (gerekçe, süre, iki taraftaki kayıp, sonuç, olay geçmişi), krallık sayfalarında savaş listesi; olaylar "daha fazla" ile sayfalanır
- Krallıklar sekmesi nüfusa göre sıralı, ☠ kıtlık / ⚑ huzursuz / ✚ salgın rozetleri; yıllık özet sütun açıklaması; Arazi modu efsanesi
- "Takip" iki anlamdan ayrıldı: 🔍 Kamera takibi ve ☆ İzleme listesi; ölen izlenen kişi † görünür; yinelenen toast kaldırıldı; Esc seçimi bırakır;  kısayol sayfası; şehir isabet alanı büyütüldü
- Haritanın çözünürlüğü ekrana göre 1×/2× (1920'de bulanıklık giderildi); "gerçek hız" göstergesi yumuşatıldı

### UI Aşama C, D, E
- Birey kartı yeniden tasarlandı: büyük sprite, unvan, krallık bağlantısı, **şu an ne yapıyor ve neden** (durumdan okunan gerekçeler + kişilik etkisi), karar puanları (seçili kişi için), enerji/hoşnutluk/korku çubukları, en belirgin 3 özellik, açılır bölümler (aile, kişilik, anılar, yaşam çizgisi)
- **Kişiler** sekmesi: yaşayan herkes, arama, krallık ve meslek filtresi, "ilginç birey" önerisi (🎲); **takip listesi çubuğu** (en çok 8 kişi, ruh hâli renkli çerçeve)
- **Toast akışı:** savaş, isyan, hanedan, salgın gibi büyük olaylar ve takip edilen kişilerin olayları (tıklayınca ilgili kişiye/krallığa gider); **yıllık özet** tablosu (nüfus, doğum, ölüm, evlilik, savaş, isyan, tahta çıkış, kıtlık/salgın)
- Ayarlar: yazı boyutu, yüksek kontrast, hareketi azalt, bildirimleri kapat (tarayıcıda hatırlanır); 3 adımlık ilk açılış turu
- Performans: komşu taraması yalnızca savaşta/hekim/sosyalleşme anında, savaş tablosu adım başına bir kez, hızlı mesafe; 613 kişide adım 9,97 → 8,06 ms (−%19). Kare çizimi 2,3 ms. 8× düğmesi ~600 kişide gerçekte ≈2× (gösterge "gerçek hız" yazar)

### Bağımsız değerlendirme 6 düzeltmeleri (isimlendirme 5/10, yeterlilik 6/10)
- İsimlendirme: 6 isim kültürü (kuzey, kelt, latin, bozkır, peri, frank); her krallık bir kültürden alır; kişi (cinsiyete göre), soyad, şehir ve krallık adı ayrı havuzlardan; tam ad çakışması %7,6-15 → %0,03; şehir/soyad çakışması 0; yeni hanedan kolu (%4 yeni soyad); sayılı adlar ("Belka 2") kalktı; hekim/bilim terimleri tutarlı
- Hatalar: şehir simgesine tıklayınca kart açılır (figür önceliği kalktı); boş zemine tıklamak seçimi kaldırır, Shift+tık yiyecek yağdırır (eskiden sessizce benzetimi bozuyordu); Geri düğmesi her bağlantıda; olaylar kronolojik; efsane unvanları çeşitli; 1366×768 ve 1280×720'de kaydırma yok
- Savaş açma olaylarına gerekçe (ihanet, sınır gerginliği, zengin ambar, hırs, eski husumet...); kıtlık, salgın ve hasat olayları krallık bazlı ve krallığa bağlı; Yiyecek ve Huzur modlarına renk ölçeği ve ambar çubukları
- Hız: kare bütçesi eklendi; bilgisayar istenen hızı yetiştiremezse "gerçek hız ≈N×" göstergesi

### UI Aşama B + Legends
- Harita: yakınlaştırmaya göre krallık adları (uzak) → şehir adları → kişi adları (yakın); harita modları Siyasi / Arazi / Yiyecek (toprak verimi + ambar çubukları) / Huzur (huzursuzluk renk skalası) / İlişki (savaş-ittifak-ticaret çizgileri); mini harita (tıkla/sürükle); fare üstü ipucu (kişi, şehir); şehir kartı (ambar, sur, tarla durumu, hükümdar); başkent adları benzersiz
- Legends (📜 Tarih): Dünya (krallık nüfus zaman çizelgesi + olay işaretleri + dönüm noktaları), Kişiler (yaşayan + ölen kayıtları, arama, filtre, hayat sayfası, aile bağlantıları, katil/ölüm nedeni, hükümdarlık), Krallıklar (hükümdar listesi, nüfus grafiği, şehirler, olay günlüğü, ayrıldığı krallık), Olaylar (tür filtreli, aranabilir, bağlantılı), Efsaneler
- Veri: ölen kişi arşivi (en çok 6000), yapılandırılmış olay günlüğü (tür, bağlı krallık/kişi), hükümdar kaydı, yıllık nüfus geçmişi

### UI Aşama A: iskelet + medieval tema
- Yeni düzen: üst çubuk (tarih, duraklat/adım/1×-8× hız, Bilim/Kültür/Tarih/Vakayiname, seed), tam genişlikte çerçeveli harita, sağda sekmeli çekmece (Birey, Hikâye, Krallıklar, Dünya, Ayarlar), haritanın altında "Son olaylar"
- Medieval tema: ceviz/bronz/altın, Cinzel + Crimson Pro yazı tipleri (çevrimdışıysa Palatino/Georgia), çerçeveli harita ve kartlar
- Kısayollar: Boşluk duraklat, +/− hız, F takip, Esc pencereleri kapat; harita/hikâye bağlantıları Birey sekmesini açar
- Teknoloji ve Kültür ağaçları yeniden tasarlandı: çağ şeritleri, parşömen kartlar (edinildi / araştırılabilir / kilitli), etki yazıları, üzerine gelince ön koşul ipucu

### Tur 9: kullanıcı geri bildirimi (sınırlar, meslekler, aynılık)
- Başlangıç: başkentler köşeler yerine araziye göre seçiliyor (verimli toprak, nehir kıyısı; dağ/orman/nehir üstü yasak); oto modda 3-6 krallık; nehir sayısı/yönü ve dağ oranı seed'e göre; krallık adları ve renkleri seed'e göre karışıyor; isyan krallıklarının adı/rengi çakışmıyor; yeni şehirler en uygun araziye kuruluyor
- Sınırlar: etki alanı dağ/nehir/ormandan zayıflıyor (sınırlar doğal engelleri izliyor); çizim hücre karesi yerine yumuşak kontur
- Meslekler: zanaatkâr (alet düzeyi → tarım verimi +%25'e kadar, silah hasarı +%12'ye kadar) ve rahip (inanç düzeyi → öfke −%18'e kadar, tapınakta ayin); yeni sprite'lar
- Makro ölçüm (8 seed × 250 yıl): hata 0, yok oluş 0, en düşük nüfus 126, ortalama yaşayan krallık 5,5 (önceki 6,8); seed'ler arası yörüngeler belirgin biçimde farklı (ör. alpha9: 3 krallıkta hegemonya, geç savaş 26; m7q: 10 krallığa parçalanma, geç savaş 109)

### Tur 7-8: tarım ve görsel yenileme
- Tarım: bahar ekim, yaz büyüme, sonbahar hasat, kış çürüme; üretimin yarısı tarlalardan (`fieldShare` 0,5); `foodScale` 0,24; "kötü/bereketli hasat" hikâyeleri; şehir çevresinde sıralı tarla yamaları
- Makro ölçüm (8 seed × 250 yıl): hata 0, yok oluş 0, en düşük nüfus 150, ortalama yaşayan krallık 6,8
- Arazi: yumuşak biyom geçişleri, kabartma gölgelendirme, çam/dağ/tepe/ot/çiçek süsleri, katmanlı nehirler (yalnızca çizim, benzetim RNG'sine dokunmaz)
- İkonlar: rol/yaş/cinsiyete göre figürler (çiftçi şapkalı, asker kask-kalkan-mızraklı, hekim haç işaretli, tüccar çuvallı, casus kukuletalı, çocuk, bastonlu yaşlı, tacı olan hükümdar); kule ve evli yerleşim ikonları

### Tur 6 (bağımsız değerlendirme 5: 6.1)
- Hata: isyanda hükümdar yeni krallığa geçince eski krallığın tahtı bayat kalıyordu (örnek seed'de kayıtların %24'ü tutarsız, şimdi %0,7)
- Kişilik → seçim: çalışkanlık, cesaret/onur, sosyallik, meraklılık, uyumluluk eylem puanlarını belirgin biçimde değiştiriyor; gevşek kişiler ocak başında oyalanıyor (aynı rolde çalışkanlık ~ çalışma payı |r| ≈ 0,6-0,77; yorumcunun ölçtüğü en fazla 0,21). Risk (savaş) korelasyonu zayıf kaldı: savaş eylemi nadir
- Ekonomi: hekim bakımı 40 tick boyunca ölüm riskini ×0,6 yapıyor; hekim ve tüccar oranları sabit değil, hasta yükü/salgın/ticarete göre kayıyor; eksi ambar hatası (küsuratlı stoktan 1 birim yeme) düzeltildi
- Arayüz: "Ölçümler" paneli (dönem yaşam tablosundan doğumda beklenen ömür, 15'e ulaşma, doğurganlık, ölüm nedenleri, hedef aralıklarla); Türkçe ek uyumu (Mirka'nın, Kadûr'un)

### Realism pass
- Demografi: cinsiyet, yaşa bağlı ölüm tehlikesi, evli kadından doğum, eş eşleştirmesi, salgın, kurucu aileler
- Okunabilirlik: birey takibi, yakınlaştırma, hayat çizgisi, ağ çizgileri, yıllık hikâye akışı, nüfus piramidi, mevsimler
- Arazi: seed'li ova/orman/tepe/dağ/nehir, verimli yerde yiyecek, mevsimsel yiyecek, araziye göre hız
- Siyaset: huzursuzluk yetişkinler üzerinden, isyan eşiği yeniden ayarlandı (geç dönem isyan 0 → var)
- Savaş: sefer mevsimi, düşük ölümcüllük, yaralı asker geri çekilmesi
- RNG: mulberry32 → sfc32; kaynak kaydı `docs/SOURCES.md`, plan `docs/PLAN.md`
- Test düzeneği hatası düzeltildi (tuval yüksekliği 700 → 520)
- Savaş hatası: askerler barışta ve ittifak döneminde de öldürüyordu; artık yalnızca savaştaki devletler, çocuklar hedef değil
- Çocuklar ebeveynin yakınında, hamile/emziren kadınlar eve yakın, kişilik eylem puanlarında, kardeşlere benzersiz ad
- Ekonomi: kapalı enerji hesabı (bedava enerji kaldırıldı), yiyecek nüfusu belirliyor, tahıl ticareti, ambar yağması
- Bireyler: 8+ yaş çocuk yardımı, yaşlılar hafif iş, meslek mirası; yeni hikâye türleri (yağma, kıtlık, salgın, çıraklık)
- Doğruluk hatası: `_killer` hiç sıfırlanmıyordu (şiddetli ölümlerin %35-45'i yanlış atfediliyordu); artık yalnızca son 40 adımdaki vuruş sayılır
- Rol-özel boşta işler, yumuşak doğum tavanı, bölünme asgarileri, varsayılan hız 1×
- Yok olma hataları: sahipsiz halk yeni/yakın krallığa katılır; Malthus doğurganlığı sıfırlamaz

### Added
- Bireyin zihni: Big Five kişilik + 4 değer (ebeveynden kalıtım), tek yönlü görüş haritası, homofili, evlilik, gerçek ebeveyn/çocuk bağı
- Hafıza (sınırlı yuva, güç yasasıyla sönme), yas, intikam; hedefler (intikam, eş arama, aile, dost)
- Hükümdar bir bireydir: krallığın tutumu liderin kişiliğinden türer; veraset, hanedan, darbe
- İklim: nadir büyük kuraklıklar; rejim yaşına bağlı meşruiyet erimesi
- Arayüz: kişilik çubukları, aile/dost/hasım bağlantıları, anılar, hedef, hükümdarlar ve hanedanlar
- Seed'li rastgelelik (`mulberry32`): aynı seed = aynı tarih. Arayüzde seed kutusu, 🎲 yeni seed düğmesi ve `?seed=` URL parametresi
- `docs/RESEARCH.md`: araştırma notları ve yol haritası

### Changed
- İsyan kuralı Epstein (2002) modeline göre yeniden yazıldı (görüşteki asker/isyancı oranı, tutuklanma olasılığı); `riskK`/`rebelThresh` artık kullanılıyor
- Bölünme için ~1.4 yıl sürekli huzursuzluk ve bölünme sonrası ~6 yıl soğuma gerekiyor
- Tek şehirli devlet bölünmek yerine saray darbesi yaşar
- Büyük devletlerde meşruiyet "aşırı genişleme" ile düşer (Turchin asabiya fikri)

### Fixed
- Bazı seed'lerde yüzlerce krallığın doğup çökmesi (churn): 12.000 adımda 66-77 krallık yerine 8-26 (20.000 adımda)
- İsyan tavanı (`maxKingdoms`) ölü krallıkları da sayıyordu; 10 krallık kurulduktan sonra isyanla yeni krallık doğmuyordu
- Sıfırlamada eski koşunun tick'i yeni bireylerin doğum zamanına sızıyordu
- Fetihte ölen bireyler efsane kontrolünden geçmiyordu
- Krallık kimlikleri 127'yi aşınca `Int8Array` taşabiliyordu (`Int16Array`)

### Performance
- Uzamsal ızgara: sayısal indeks, dizi yeniden kullanımı
- Tek adımda tekrarlanan `filter`/`indexOf`/`splice` çağrıları tek geçişe indirildi

## v1.0.0

### Added
- Autonomous AI agents
- Diplomacy system
- Territory expansion
- Cities
- Dynasties
- Rebellions
- Legends
- GitHub Pages deployment
