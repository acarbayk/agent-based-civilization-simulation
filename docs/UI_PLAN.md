# UI / UX planı

Hazırlanma biçimi: benzer projeler ve oyunlar için kullanıcı yorumları ve tasarım yazıları arandı, mevcut arayüzün ekran görüntüleri (masaüstü 1366×900, mobil 390×844) incelendi.
**Doğrulama sınırı:** Steam, Paradox forumu, Game Developer, NN/g ve benzeri sayfalar bu oturumun ağ politikasıyla açılamadı. Aşağıdaki oyun yorumları **arama özetlerinden** (düzey B/C) gelir; yalnızca GitHub'daki *Common Ground* README'si birincil olarak okundu (düzey A). Ham kullanıcı yorumu sayıları (yüzdeler vb.) tahmindir, karar dayanağı olarak kullanılmadı.

## 1. Benzer projeler ve ne yapıyorlar

| Proje | Türü | UI'dan çıkan ders | Düzey |
|---|---|---|---|
| [Common Ground (fraferra/agents-world)](https://github.com/fraferra/agents-world) | Tarayıcıda toplum simülasyonu (bize en yakın) | Harita katmanları (Manzara, Toplumlar, Yiyecek, Fikirler, İlişkiler); aranabilir **kişi listesi**; birey kartında *şu anki düşünce, hedef, kişilik, ihtiyaçlar, değerler, plan* ve **karar faktörü dökümü**; Boşluk tuşuyla duraklat, 1×-100× hız; "dünya koşulları" menüsü (afet, salgın, festival); yorum yok, anlatı şablonla | A |
| [AEON](https://lordbasilaiassistant-sudo.github.io/aeon/) , [Civil Zones](https://github.com/beautifulplanet/Civil-Zones) | Tarayıcı tanrı/medeniyet oyunları | Gözlem + kısmi müdahale; kaynak/nüfus göstergeleri | C |
| Dwarf Fortress (Steam) | Karakter odaklı simülasyon | Karakter psikolojisi (kişilik, ilişki, hafıza) bağlanmayı güçlendiriyor; **Legends** modu geçmişi gezilebilir kılıyor; ama Steam arayüzü "fare merkezli", **duyurular fark edilmeyen bir pencerede**, iş ekranı "anlaşılmaz", küçük metin eleştirisi var | B |
| RimWorld | Kolonist simülasyonu | Kolonist çubuğu (isim, görünüm, durum ikonları, ruh hâli rengi) beğeniliyor ama yetersiz bulunup çok sayıda mod yazılmış (sıralama, ölçek, renkli ruh hâli, silah göstergesi, genel bakış/tıbbi sekme); "hikâye üreteci" + insanların olaylara anlam yüklemesi (*apophenia*) | B |
| Crusader Kings 3 | Hanedan/harita | Harita **tıklamadan okunabilir** olmalı: arazi, nehirler (ordu geçerken geçip geçmediğini gösterecek kadar belirgin), üç yakınlaştırma seviyesi (en uzakta kâğıt harita), harita modları, yumuşak harita yazıları. Şikâyet: kötü ölçekleme, üst üste binen pencereler, koyu zeminde düşük kontrast | B |
| Songs of Syx | Şehir kurma | Derinlik çok seviliyor; arayüz en zayıf nokta: çok menü/alt menü, birbirine benzeyen ikonlar (100 saatte bile karıştırılıyor), bilgiye ulaşmak zor | B |
| WorldBox | Tanrı simülasyonu | Özerk toplumlar, isyanlar, imparatorlukların çöküşü izlenerek eğleniliyor; "biraz sığ" ve "ortaçağdan sonrası yok" istekleri | B |
| Ant Farm / koloni sandbox'ları | Gözlem oyunları | Hız ve çevre parametreleri; rahat izleme modu | C |

## 2. İnsanların beklentileri (tekrar eden temalar)

1. **Harita ilk bakışta okunmalı.** Arazi, nehir, sınır, yerleşim tıklamadan anlaşılmalı (CK3 harita ilkesi). Zoom'a göre ayrıntı düzeyi değişmeli.
2. **Bireye bağlanma.** İsim, kişilik, ilişki, hafıza, "şimdi ne düşünüyor / neden bunu yapıyor" (DF, RimWorld, Common Ground). Karakteri listeden bulabilmek ve yakında tutmak (kolonist çubuğu).
3. **Olaylar kaçmamalı.** DF'nin gizli duyuru penceresi şikâyet konusu; RimWorld'de olay uyarıları ekranda. Önemli olay görünür olmalı, tıklayınca ilgili yere/kişiye götürmeli.
4. **Özelleştirilebilir bilgi.** RimWorld modlarının çoğunluğu "ne gösterileceğini seçeyim" isteği.
5. **Aşamalı bilgi gösterimi.** Gerekli bilgi önde, ayrıntı isteğe bağlı (ipucu kutuları, açılır paneller); ilk dakikalarda öğretici oyunun içinde.
6. **Fare/dokunma önce, kısayol ek.** DF Steam eleştirisi tersini gösteriyor (yalnız fareye güvenmek dizüstünde kötü); biz ikisini de destekleyelim.
7. **Ayırt edilebilir ikonlar.** Songs of Syx şikâyeti: benzer ikonlar. Rol ikonlarımız şekil + renk + işaretle ayrışmalı, açıklamalı.
8. **Hız ve duraklatma kontrolü**, gözlemci modu.
9. **Derinlik isteniyor ama anlaşılır olmalı.** Karmaşıklık sevilir, arayüz onu taşımak zorunda.

## 3. Mevcut arayüzün tanısı (ekran görüntülerinden)

- Masaüstünde harita ~680 px; **altında büyük boş alan** ve sağ sütun 2600 px uzunluğunda kaydırma. Harita ekranın asıl alanı olmalı.
- Üst başlıkta 6 satırlık metin; öğreticiyi gömülü paragraf yapmış.
- **Hikâyeler** (en çekici içerik) sağ sütunda ikinci bloğa gömülü, ekranı kaplayan bir uyarı/toast yok.
- Birey kartı uzun ve yoğun (kişilik çubukları, 5 satır akraba, düz metin yaşam özeti); "şu an neden bunu yapıyor" yok.
- Kişi listesi/arama yok; "takip et" yalnızca hikâye bağlantısından. Çoklu takip yok.
- Harita modu yok (siyasi/yiyecek/huzursuzluk gibi). Yerleşim ve krallık adı haritada görünmüyor, yalnızca yan panelde.
- Fare üstü ipucu (tooltip) yok; ikon açıklaması (efsane) tek satır ve yeni rolleri içermiyor.
- Hız: tek kaydırıcı; duraklat/1×/2×/4× düğmesi yok; kısayol yok.
- Mobil: harita küçük, kontroller 3 satır, paneller alt alta uzun kaydırma; dokunma hedefleri 44 px'e yakın değil.
- Başlıkta ve panellerde küçük/gri metin, düşük kontrast (CK3 ve DF şikâyetleriyle aynı sorun).

## 4. Plan (sırayla, her aşamada ölçülebilir bitiş ölçütü)

**Aşama A: iskelet (düzen)**
- Tam genişlikte harita; sağda **sekmeli çekmece**: *Birey · Hikâye · Krallıklar · Ölçümler · Vakayiname · Ayarlar*; çekmece daraltılabilir. Mobilde alttan açılan **sayfa** (bottom sheet).
- Üst çubuk: tarih + mevsim ikonu, nüfus, **duraklat / 1× / 2× / 4× / 8×** düğmeleri, tohum, sıfırla. Öğretici başlıktan çıkıp ilk açılışta 3 adımlık oyun içi tur.
- Bitiş ölçütü: 1366×768 ve 390×844'te kaydırma olmadan harita + temel kontroller; dokunma hedefleri ≥44 px; konsol hatası yok.

**Aşama B: harita okunabilirliği**
- Üç yakınlaştırma seviyesi: uzak (krallık adları + sınır, figürler nokta), orta (şehir adları, figür ikonları), yakın (isim etiketleri, hareket animasyonu).
- Harita modları: *Siyasi · Arazi · Yiyecek/Hasat · Huzursuzluk · İlişkiler* (Common Ground ve CK3 örneği).
- Fare üstü ipucu (birey, şehir, tarla), küçük **mini harita**, açılır **efsane** (rol ikonları açıklamalı), şehir tıklayınca kart (nüfus, ambar, hasat, hükümdar).
- Bitiş ölçütü: kör bir değerlendirici, tıklamadan "kim nerede güçlü, nerede kıtlık var"ı doğru söyleyebilmeli.

**Aşama C: birey ve bağlanma**
- Birey kartı yeniden tasarım: büyük sprite, isim/rol/yaş, **şu anki düşünce ve nedeni** ("tarlaya gidiyor çünkü: çalışkan, ekim mevsimi, ambar boş"), ihtiyaç çubukları, kişilik özeti (en belirgin 3 özellik), aile mini ağacı, yaşam çizgisi, anılar; ayrıntılar açılır.
- **Kişi listesi + arama + filtre** (rol, krallık, yaş); "ilginç birey" önerisi; **takip edilen kişiler çubuğu** (RimWorld kolonist çubuğu gibi, ruh hâli rengiyle). Takip edilen kişi hakkında olay olunca toast.
- Bitiş ölçütü: bir kişiyi 3 tıkta bulup takip etmek; "neden bunu yapıyor" sorusunu kart yanıtlıyor.

**Aşama D: hikâye ve tarih**
- Ekranın köşesinde kapanabilir **toast akışı** (savaş, isyan, hanedan, kıtlık, doğum/ölüm takip edilenlerde); tıklayınca olay yerine kamera.
- Vakayiname sekmesi: filtre (savaş/siyaset/aile/doğa), yıl çubuğu; **Legends** benzeri gezinti (kişi ↔ krallık ↔ olay bağlantıları).
- Yıl sonu özeti kartı ("Yıl 42: 3 evlilik, 1 savaş, kötü hasat").
- Bitiş ölçütü: kullanıcı bir olayı kaçırmadan akışı izleyebilir; filtreler çalışır.

**Aşama E: cila ve erişilebilirlik**
- Kısayollar (Boşluk duraklat, +/− hız, F takip, M harita modu, / arama), kontrast ve yazı boyutu ayarı, hareket azaltma, tema (koyu / parşömen), Türkçe metinlerin sadeleştirilmesi.
- Başarım: büyük nüfusta kare süresi ölçümü; LOD ile 1000+ ajan akıcı.
- Bitiş ölçütü: temel kontrast ≥4.5:1, yazı boyutu ≥12 px, 1000 ajanda ≥45 fps (ölçülecek).

**Aşama F: doğrulama**
- Taze bir ajanla **kullanılabilirlik testi** (görevler: "en büyük krallığı bul", "bu krallıkta kıtlık var mı", "bir kişiyi bul ve neden evlendiğini söyle", "yeni bir olayı yakala"); tıklama sayısı ve başarı oranı kaydı.
- Eski kör değerlendirmeye "okunabilirlik ve his" ve yeni "kullanılabilirlik" boyutları eklenir. Hedef: iki boyutta ≥8 (bağımsız puanla kanıtlanana dek iddia edilmez).

## 5. Kararlar (kullanıcı)

1. **Masaüstü öncelikli** (mobil sonra).
2. **Tema:** mevcut koyu çizgi korunur ama daha **medieval** (ceviz/bronz/altın yaldız, serif, çerçeveli harita). *Uygulandı (Aşama A).*
3. **Legends benzeri tarih gezintisi:** kavram açıklandı; ilk sürüm Kişiler + Krallıklar + Olaylar + çapraz bağlantı, harita kopyaları sonra. Kullanıcı kararı bekleniyor.
4. **Teknoloji/kültür ağaçları ana arayüzde kalır**, tasarımı yenilenir. *Uygulandı: parşömen kartlar, çağ şeritleri, ön koşul ipuçları.*

## 5b. İlk soru listesi (arşiv)

1. Öncelik: masaüstü mü mobil mi? (Plan ikisini de kapsıyor ama tasarım kararları farklı.)
2. Görsel tema: bugünkü koyu arayüz mü, harita çevresinde "parşömen/ortaçağ" teması mı?
3. Tarih ve efsane ekranı (Legends benzeri) bu turda mı, sonraya mı?
4. Teknoloji/kültür ağacı pencereleri ana arayüzde kalsın mı, "Gelişmiş" altına mı taşınsın?

## 6. Kaynaklar

- [Common Ground, fraferra/agents-world (README, okundu)](https://github.com/fraferra/agents-world)
- [Dwarf Fortress Steam tartışmaları ve yorumlar (arama özeti)](https://steamcommunity.com/app/975370/discussions/0/3709307511570113977/), [Metacritic](https://www.metacritic.com/game/dwarf-fortress/user-reviews/)
- [RimWorld arayüz modları ve tartışmalar (arama özeti)](https://www.nexusmods.com/rimworld/mods/93), [RimWorld Wiki: arayüz](https://rimworldwiki.com/wiki/User_interface)
- [CK3 harita günlüğü (arama özeti)](https://forum.paradoxplaza.com/forum/developer-diary/ckiii-dev-diary-25-map-features-and-map-modes.1388210/), [CK3 UI geri bildirimi](https://forum.paradoxplaza.com/forum/threads/crusader-kings-iii-ui-balance-functionality-bugs-launch-reality-check.1417859/)
- [Songs of Syx incelemesi (arama özeti)](https://www.realityremake.com/articles/songs-of-syx-review-a-brutally-complex-colony-sim-with-endless-replay), [GamingOnLinux](https://www.gamingonlinux.com/2023/01/dwarf-fortress-too-retro-songs-of-syx-is-worth-looking-at-with-another-big-beta-update/)
- [WorldBox yorumları (arama özeti)](https://steambase.io/games/worldbox-god-simulator/reviews)
- [RimWorld ve Dwarf Fortress hikâye anlatımı (arama özeti)](https://www.gamedeveloper.com/design/dwarf-fortress-and-rimworld-tell-very-different-stories)
- [Oyun UX ve aşamalı bilgi gösterimi (genel)](https://www.uxpin.com/studio/blog/game-ux/), [NN/g oyunlara uygulanan kullanılabilirlik ilkeleri](https://www.nngroup.com/articles/usability-heuristics-applied-video-games/)
