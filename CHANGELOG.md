# Changelog

## Unreleased

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
