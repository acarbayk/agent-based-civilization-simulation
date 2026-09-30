# Changelog

## Unreleased

### Added
- Seed'li rastgelelik (`mulberry32`): aynı seed = aynı tarih. Arayüzde seed kutusu, 🎲 yeni seed düğmesi ve `?seed=` URL parametresi
- `docs/RESEARCH.md`: araştırma notları ve yol haritası

### Fixed
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
