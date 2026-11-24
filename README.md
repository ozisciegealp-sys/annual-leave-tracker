# Yıllık İzin Takip Sistemi

**Türkçe** | [English](README.en.md)

İK'dan periyodik gelen personel verisinden, 6 bölgeli bir perakende operasyonunda **biriken yıllık izin bakiyelerini** ve **yaklaşan izin hak edişlerini** izleyen, bölge bazında alarm özeti ve öncelikli planlama listesi üreten Excel çalışma kitabı.

> **Gizlilik ve Sentetik Veri Notu:** Bu çalışma kitabı, 6 bölgenin saha personeli için kurduğum ve yürüttüğüm yıllık izin takip sisteminin **tamamen sentetik (kurgu) veriyle** yeniden oluşturulmuş sürümüdür. Kurumsal gizlilik ve KVKK gereği hiçbir gerçek çalışan bilgisi, sicil numarası, mağaza veya marka adı yer almaz; tüm kayıtlar örnekleme amacıyla üretilmiştir.

## Hangi problemi çözüyor

Çok şubeli saha operasyonlarında kullanılmayan yıllık izinler zamanla birikir; biriken bakiyeler hem yasal yükümlülük oluşturur hem de toplu kullanıldığında mağaza vardiya planlarını zorlar. Bu sistem:

- Her çalışanın kıdemine ve yaşına göre yıllık izin hak edişini **4857 sayılı İş Kanunu (Md. 53)** dilimlerine göre hesaplar.
- Önümüzdeki **60 gün içinde** yeni yıllık izin hak edecek personeli önceden işaretler.
- İki dönem İK verisi arasındaki farktan dönem içinde kullandırılan izin gününü çıkarır; veri tutarsızlıklarını (`Kontrol et`) ve yeni giriş / ayrılışları otomatik ayıklar.
- Bakiyesi **14 gün ve üzeri** olan çalışanları büyükten küçüğe sıralar ve bölge/mağaza yöneticilerine iletilecek bilgilendirme metnini otomatik üretir.
- Bölge özetinde 20 gün üstü personel sayısına göre **▲ ALARM** veya **● Planla** durumu üretir; ülke geneli ham veri yapıştırıldığında kapsam dışı bölgeleri hesaplardan otomatik filtreler.

## Öncesi

İzin bakiyeleri ve hak ediş tarihleri ham İK listeleri üzerinden elle süzülerek takip edilmeye çalışılıyordu; tüm hesaplar, alarmlar ve bölge listeleri formül yapısına bağlandı.

## Kullanım akışı

Yeni dönem İK verisi geldiğinde:

1. `Veri_Guncel` sayfasındaki mevcut tabloyu (A–H sütunları) `Veri_Onceki` sayfasına **değer olarak** yapıştırın ve `Ayarlar` sayfasında önceki veri tarihini güncelleyin.
2. Yeni İK verisini `Veri_Guncel` sayfasına aynı sütun düzeniyle yapıştırıp güncel veri tarihini girin.
3. `Ozet` sayfasından 6 bölgenin bakiye yükünü, alarm durumunu, önceki döneme göre değişimi ve grafikleri inceleyin.
4. `Ayarlar` > **Liste bölgesi** hücresinden bölge seçerek `Liste_14_Gun` sayfasındaki öncelikli personel listesini ve hazır bilgilendirme metnini ilgili yöneticilerle paylaşın.

## Sayfalar

| Sayfa | Ne yapar |
|---|---|
| `Nasil_Kullanilir` | Dönemsel iş akışı, yasal hesap kuralları ve renk kodu |
| `Ozet` | 6 bölge bazında personel sayısı, biriken izin, 14+ ve 20+ gün kişi sayısı, alarm durumu, yaklaşan hak ediş, kullandırılan izin, dönem farkı ve iki karşılaştırma grafiği |
| `Liste_14_Gun` | Seçili bölgede bakiyesi eşik üstü personelin büyükten küçüğe sıralı listesi ve hazır yönetici bilgilendirme taslağı |
| `Takip` | Personel bazında kıdem, yasal hak, yıldönümü ve dönem farkı hesapları |
| `Ayarlar` | Veri tarihleri, planlama/alarm eşikleri (14 / 20 gün), takip edilen 6 bölge ve liste filtresi |
| `Veri_Guncel` / `Veri_Onceki` | Güncel ve bir önceki dönem İK verilerinin yapıştırıldığı sayfalar |

## Hesaplar

**Yıllık izin hakkı** (4857 sayılı İş Kanunu, Madde 53):

| Kıdem / Yaş Koşulu | Yıllık Hak (Gün) |
|---|---|
| 1 yıldan az | Hak yok |
| 1–5 yıl (5 hariç) | 14 |
| 5–15 yıl (15 hariç) | 20 |
| 15 yıl ve üzeri | 26 |
| 18 yaş ve altı veya 50 yaş ve üzeri | En az 20 |

- **Sonraki hak ediş tarihi** = işe giriş tarihinin bir sonraki yıldönümü
- **Dönemde kazanılan** = işe giriş yıldönümü iki veri tarihi arasına düşüyorsa yıllık hak, düşmüyorsa `0`
- **Dönemde kullanılan** = `önceki bakiye + dönemde kazanılan − güncel bakiye` (eksi sonuç çıkarsa `Kontrol et`, önceki veride yoksa `Yeni personel` olarak işaretlenir)
- **Bölge alarmı**: 20 gün üstü personel sayısı eşiğe (varsayılan `5`) ulaşırsa **▲ ALARM**, altında kalıp 14 gün üstü personel varsa **● Planla**

## Kurulum ve veri formatı

Makro içermez; `izin-takip.xlsx` Excel 2010+ ve LibreOffice'te doğrudan açılır. Ham veri `A:H` sütunlarına (`Sicil No | Ad Soyad | Bölge | Mağaza | İşe Giriş Tarihi | Doğum Tarihi | Görev | Kalan İzin`) yapıştırılır.

## Lisans

MIT. Ayrıntılar için `LICENSE` dosyasına bakın.
