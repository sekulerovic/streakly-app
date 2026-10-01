# Streakly — Yapılacaklar

Bu liste, Streakly'nin iOS ve Android'de çalışan basit bir alışkanlık takip uygulaması olarak ilk sürümünü kapsar.

## MVP — ilk kullanılabilir sürüm

- [ ] .NET MAUI projesini Android ve iOS hedefleriyle oluştur
- [ ] Turuncu ve siyah tonlarında, okunabilir ve sade bir ekran tasarımı hazırla
- [ ] Bugünün alışkanlıklarını gösteren ana ekranı oluştur
- [ ] Alışkanlık ekleme, düzenleme ve silme akışlarını ekle
- [ ] Bir alışkanlığı bugün için tamamlandı olarak işaretleme özelliği ekle
- [ ] Alışkanlıkları ve günlük tamamlanma kayıtlarını cihazda SQLite ile sakla
- [ ] Uygulama yeniden açıldığında verilerin korunduğunu doğrula
- [ ] Seri hesaplamaları için testler yaz
- [ ] Android emülatöründe temel akışları test et

## Daha sonraya bırak

- [ ] Hatırlatma bildirimleri ekle
- [ ] Haftalık ve aylık ilerleme görünümü ekle
- [ ] iOS'ta test et ve dağıtım için yapılandır
- [ ] İhtiyaç netleşirse hesap, yedekleme veya cihazlar arası eşitlemeyi değerlendir

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
