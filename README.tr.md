# Streakly App

**Diller:** [English](README.md) | Türkçe | [Deutsch](README.de.md)

[Yapılacaklar ve mimari önerisi](TODO.md)

[Use case'ler](docs/use-cases.md)

Streakly App, insanların günlük eylemlerini, serilerini ve zaman içindeki ilerlemelerini takip ederek düzenli alışkanlıklar geliştirmelerine yardımcı olmak için tasarlanmış bir üretkenlik ve alışkanlık takip uygulamasıdır.

## Projenin amacı

Uygulama kullanıcıların şunları yapmasına yardımcı olmayı amaçlar:

- Tekrarlanan alışkanlıklar oluşturmak ve yönetmek
- Günlük kontrol ve tamamlanma kayıtları girmek
- Serileri, ilerlemeyi ve geçmişi görüntülemek
- Hafif motivasyon ve sorumluluk hatırlatmaları almak
- Mükemmeliyet yerine sürdürülebilir rutinlere odaklanmak

## Teknoloji

Streakly, iOS ve Android için aşağıdaki teknolojilerle geliştirilen platformlar arası bir mobil uygulamadır:

- C# ve .NET
- Paylaşılan mobil arayüz ve yerel platform entegrasyonları için .NET MAUI
- Yerel depolama gerektiğinde cihaz üzerinde veri saklamak için SQLite

Ortak bir arka uç, kullanıcı hesapları veya eşitleme gerektiren özellikler ortaya çıkarsa ileride bir ASP.NET Core servisi eklenebilir. Ürün gereksinimleri aksini belirtmedikçe temel alışkanlık takibi arka uca bağımlı olmamalıdır.

## Depo durumu

Depoda Android/iOS için başlangıç .NET MAUI uygulaması ve ilk ürün dokümantasyonu bulunur. Alışkanlık ve görev özellikleri henüz uygulanmamıştır.

## Planlanan yapı

- `src/StreaklyApp/` — .NET MAUI uygulaması, platform giriş noktaları ve paylaşılan arayüz
- `tests/` — otomatik testler
- `docs/` — tasarım ve ürün dokümantasyonu
- `.github/` — depo otomasyonu ve Copilot yönergeleri

## Yerel kurulum

Desteklenen .NET SDK'sını, .NET MAUI iş yükünü ve Git'i yükleyin. Windows'ta .NET MAUI iş yüküyle Visual Studio'yu; macOS'te ise .NET araçlarını ve Xcode'u kullanın.

Uygulamayı geliştirmek ve çalıştırmak için:

1. Visual Studio'da `StreaklyApp.slnx` çözümünü açın.
2. .NET bağımlılıklarını geri yükleyin.
3. Başlangıç projesi olarak `StreaklyApp`'i seçip uygulamayı Android emülatöründe veya cihazında çalıştırın.
4. iOS için derleme, çalıştırma veya imzalama işlemlerinde Xcode yüklü bir Mac kullanın. Windows'taki Visual Studio, iOS geliştirme için eşleştirilmiş bir Mac'e bağlanabilir.

## Geliştirme ilkeleri

- Karmaşık soyutlamalar yerine basit ve okunabilir kodu tercih edin
- Özellikleri küçük ve test edilebilir tutun
- Varsayımları ve kararları kodda veya dokümantasyonda belirtin
- Değişiklikleri birleştirmeden önce davranışı doğrulayın
- Gizli bilgileri ve ortama özel değerleri kaynak kontrolüne eklemeyin

## Yol haritası

- Ürün gereksinimlerini ve kullanıcı akışlarını tanımlamak
- Tekrarlayan alışkanlık ve tek seferlik görev akışlarını uygulamak
- Alışkanlık ve günlük kayıt alan modellerini tanımlamak
- Alışkanlık oluşturma ve takip özelliklerini uygulamak
- Seri hesaplamaları ve ilerleme ekranlarını eklemek
- Verileri yerel olarak saklamak ve ardından bulut eşitleme ihtiyacını değerlendirmek

## Katkıda bulunma

Kısa ve odaklı dal adları kullanın; kod birleştirme isteklerini gözden geçirmeyi kolaylaştırın. Açık commit mesajları yazın ve davranış değişiklikleri için test ekleyin.

## Lisans

Dağıtım ve ticari kullanım için nihai bir karar verilene kadar bu proje lisanslanmamıştır.
