# Streakly — Yapılacaklar

Bu liste, Streakly'nin iOS ve Android'de çalışan basit bir alışkanlık takip uygulaması olarak ilk sürümünü kapsar.

## P0 — MVP / ilk kullanılabilir sürüm

- [ ] .NET MAUI projesini Android ve iOS hedefleriyle oluştur; İngilizce, Türkçe ve Almanca yerelleştirmeyi ekle
- [ ] Turuncu-siyah, okunaklı ve erişilebilir temel arayüzü oluştur
- [ ] Tekrarlayan alışkanlık ekleme, düzenleme ve arşivleme akışlarını; haftanın günlerini seçme desteğiyle ekle
- [ ] Tarih ve isteğe bağlı saat verilebilen tek seferlik görev ekleme ve listeleme akışını ekle
- [ ] Bugün planlanmış alışkanlıkları ve tarihli görevleri gösteren ana ekranı oluştur
- [ ] Alışkanlıkları yerel SQLite veritabanında sakla; uygulama çevrimdışıyken çalıştığını doğrula
- [ ] Alışkanlıklar ve tek seferlik görevler için tamamlama kaydı oluştur; yinelenen kayıtları engelle
- [ ] Tamamlama bilgisini ana kayıt tut; kitap adı, okunan sayfa/süre, yoga pratiği veya bahçe işi gibi bağlamı isteğe bağlı kısa not olarak sakla
- [ ] Her alışkanlık için ayrı seriyi hesapla; boşta kalan planlanmamış günler seriyi bozmasın
- [ ] Tek seferlik görevlerin tamamlanmasının veya kaçırılmasının alışkanlık serisini değiştirmediğini doğrula
- [ ] Yerel tarih ve saat kurallarını test et: gece yarısı öncesi/sonrası, kaçırılan planlı gün ve planlanmamış gün
- [ ] Yaz/kış saati geçişi, saat dilimi değişikliği ve uygulamanın gece yarısında kapalı olması senaryolarını test et
- [ ] Visual Studio Android emülatörünü kur; arayüzü geliştirirken uygulamayı emülatörde çalıştır ve XAML Hot Reload ile görsel değişiklikleri kontrol et
- [ ] MVP öncesinde temel akışları en az bir gerçek Android telefonda doğrula
- [ ] MVP öncesinde iOS Simulator'da veya fiziksel iPhone'da temel akışları doğrula (Mac ve Xcode gerekir)

## P1 — MVP sonrasında

- [ ] Hatırlatma bildirimleri ekle
- [ ] Tamamlanma geçmişi/takvim ve ilerleme özetleri ekle
- [ ] Kullanılabilirlik ve erişilebilirlik testleri yapıp iyileştirmeleri uygula

## P2 — ürün ihtiyacı doğrulanınca

- [ ] Hesap, yedekleme ve cihazlar arası eşitleme ihtiyacını değerlendir
- [ ] Abonelik ve paywall ihtiyacını değerlendir; MVP'de zorunlu abonelik yok
- [ ] Mağaza dağıtımı ve yayınlama adımlarını yapılandır
- [ ] Tek seferlik görevler için seri veya tekrar kurallarına ihtiyaç olup olmadığını kullanıcı geri bildirimiyle değerlendir
- [ ] Süre, sayfa, mesafe ve tekrar sayısı gibi yapılandırılmış ölçüm alanlarını kullanıcı geri bildirimiyle değerlendir

## Önerilen basit mimari

İlk sürümde uygulamayı tek bir .NET MAUI projesinde tut. Ayrı bir sunucu veya mikroservis ekleme; alışkanlık takibi internet bağlantısı olmadan çalışsın.

```text
MAUI ekranları (Views)
        ↓
ViewModel'ler (ekran durumu ve kullanıcı eylemleri)
        ↓
Uygulama servisleri (alışkanlık ve seri kuralları)
        ↓
SQLite veri erişimi (cihazda kalıcı depolama)
```

- **Views:** XAML ile ekrandaki görünüm ve kontroller.
- **ViewModels:** MVVM yaklaşımıyla ekran durumunu ve kullanıcı eylemlerini yönetir.
- **Models:** `Habit` ve günlük tamamlanma kaydı gibi temel veri tipleri.
- **Services:** Alışkanlık işlemlerini ve seri hesaplamalarını yürütür.
- **SQLite:** Alışkanlıkları ve tamamlanma geçmişini cihazda saklar.

Başlangıçta bu bölümleri tek projede ve anlaşılır klasörlerde tutmak yeterlidir. Gereksiz katmanlar, API, kullanıcı hesabı ve bulut eşitlemesi ekleme. Ekranlar ve özellikler çoğaldığında yapıyı ihtiyaca göre genişlet.

## Görsel yön

- Koyu arka plan ve yüzeyler; turuncuyu vurgu ve ana eylem rengi olarak kullan.
- Örnek palet: arka plan `#121212`, kart `#1E1E1E`, turuncu `#FF8A00`, ana metin `#FFFFFF`, ikincil metin `#B3B3B3`.
- Metin ve butonlarda yeterli kontrastı koru; turuncuyu geniş metin alanlarının arka planı olarak kullanma.
