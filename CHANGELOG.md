# Changelog

## Unreleased

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
