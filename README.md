# Yıllık İzin Takip Sistemi

İK'dan gelen personel datasından, çok bölgeli bir perakende operasyonunda **biriken yıllık izinleri** ve **yaklaşan izin hak edişlerini** izleyen, bölge bazında özet ve planlama listesi çıkaran Excel çalışma kitabı.

> **Not:** Bu dosya, bir kahve zincirinde 6 bölgenin personeli için kurduğum ve yürüttüğüm sistemin **yeniden kurulmuş sürümüdür**. Orijinal dosya elimde olmadığı için yapı, sistemin nasıl çalıştığı hatırlanarak baştan oluşturulmuştur. Tüm personel verileri (isimler, sicil numaraları, tarihler, bakiyeler) uydurmadır.
>
> Orijinalde özetler pivot tablolardan çıkarılıp bölge müdürleri ve operasyon müdürleri için infografiğe dönüştürülüyordu. Bu sürümde aynı özetler formülle (`COUNTIFS`, `SUMIFS`) kurulmuştur ve grafikler bu özetten çizilir; yeni data yapıştırıldığında pivot yenilemeye gerek kalmadan güncellenir.

## Hangi problemi çözüyor

Bölgelerde kullanılmayan izin birikir. Biriken izin, hem personelin yasal hakkıdır hem de ileride bir anda kullanıldığında vardiya planını zorlar. Bu sistem:

- Her personelin kıdemine ve yaşına göre yıllık izin hakkını İş Kanunu'na göre hesaplar.
- Önümüzdeki 60 gün içinde yeni izin hak edecekleri işaretler.
- İki İK datası arasındaki farktan dönem içinde kullanılan izni çıkarır.
- Bakiyesi 14 gün ve üzeri olanları büyükten küçüğe listeler ve bölge müdürüne gönderilecek bilgilendirme metnini hazırlar.
- Bölge bazında biriken izni, 20 gün üstü kişi sayısına göre alarmı ve önceki döneme göre değişimi gösterir.
- Tüm Türkiye datası geldiğinde takip edilen bölgeler dışındaki kayıtları hesaplardan otomatik ayıklar.

## Öncesi

İzin bakiyeleri elle takip edilmeye çalışılıyordu. Bu sistemle hesaplar formüle bağlandı.

## Kullanım akışı

İK datası operasyon müdürüne geldiğinde:

1. `Veri_Guncel`'deki veriyi `Veri_Onceki`'ye taşıyın (A–H sütunları, **değer olarak**). `Ayarlar`'da önceki data tarihini güncelleyin.
2. Yeni datayı `Veri_Guncel`'e aynı sütun sırasıyla yapıştırın. `Ayarlar`'da güncel data tarihini girin.
3. `Ozet` sayfası bölge karşılaştırmasını ve grafikleri gösterir.
4. `Ayarlar` > **Liste bölgesi**'nden bir bölge seçin. `Liste_14_Gun` o bölgenin listesini ve bilgilendirme metnini verir. Metin ve liste bölge müdürüne iletilir; bölge müdürü de ilgili mağaza yöneticilerine aktarır.

## Sayfalar

| Sayfa | Ne yapar |
|---|---|
| `Nasil_Kullanilir` | Akış, hesaplar ve renk kodu |
| `Ozet` | Bölge bazında personel, biriken izin, 14+ ve 20+ gün kişi sayısı, alarm, yaklaşan hak ediş, dönemde kullanılan, önceki döneme göre değişim, yeni ve ayrılan personel; iki grafik |
| `Liste_14_Gun` | Bakiyesi eşiğin üzerindekiler, büyükten küçüğe; bilgilendirme metni taslağı |
| `Takip` | Kişi bazında tüm hesaplar |
| `Ayarlar` | Data tarihleri, eşikler, takip edilen bölgeler, liste filtresi |
| `Veri_Guncel` / `Veri_Onceki` | İK datasının yapıştırıldığı sayfalar |

## Hesaplar

**Yıllık izin hakkı** (4857 sayılı İş Kanunu, madde 53):

| Kıdem | Gün |
|---|---|
| 1 yıldan az | Hak yok |
| 1–5 yıl (5 hariç) | 14 |
| 5–15 yıl (15 hariç) | 20 |
| 15 yıl ve üzeri | 26 |
| 18 yaş ve altı ya da 50 yaş ve üzeri | En az 20 |

- **Sonraki hak ediş tarihi** = işe giriş tarihinin bir sonraki yıldönümü
- **Dönemde kazanılan** = işe giriş yıldönümü iki data tarihi arasına düşüyorsa yıllık hak, düşmüyorsa 0
- **Dönemde kullanılan** = önceki bakiye + dönemde kazanılan − güncel bakiye
  - Sonuç eksi çıkarsa kayıt **Kontrol et** olarak işaretlenir (veri hatası ya da elle düzeltme).
  - Önceki datada olmayan kişi **Yeni personel** olarak işaretlenir.
- **Bakiye durumu**: 14 gün ve üzeri **Planla**, 20 gün ve üzeri **ALARM**
- **Bölge alarmı**: 20 gün üzerindeki kişi sayısı eşiğe (varsayılan 5) ulaşırsa bölge **▲ ALARM**, eşiğin altında ama 14 gün üzeri kişi varsa **● Planla**

Tüm eşikler `Ayarlar` sayfasından değiştirilebilir.

## Kurulum ve veri formatı

Makro yoktur, kurulum gerekmez. `izin-takip.xlsx` dosyası Excel 2010 ve sonrası ile LibreOffice'te açılır.

İK datası şu sütun sırasıyla yapıştırılmalıdır:

| A | B | C | D | E | F | G | H |
|---|---|---|---|---|---|---|---|
| Sicil No | Ad Soyad | Bölge | Mağaza | İşe Giriş Tarihi | Doğum Tarihi | Görev | Kalan İzin (gün) |

Datanın sütun sırası farklıysa yapıştırmadan önce bu sıraya getirilmelidir. Doğum tarihi yoksa F sütunu boş bırakılır; yaş kuralı o kişi için uygulanmaz.

`Veri_Onceki` sayfasının I sütunu yardımcı formüldür (ayrılan personeli bulur); yapıştırırken yalnızca A–H sütunları kullanılmalıdır.

## Kendi verinize uyarlama

| Nerede | Ne değiştirilir |
|---|---|
| `Ayarlar` A15–A20 | Takip edilen bölge adları; İK datasındaki yazımla aynı olmalı |
| `Ayarlar` B6–B9 | Planlama ve alarm eşikleri, bölge alarmı kişi sayısı, hak ediş penceresi |

Sistem 500 personele kadar hesaplar. Daha büyük datada `Takip` sayfasındaki formül satırları aşağı doğru kopyalanmalıdır.

## Sınırlar

- Kısmi yıl, ücretsiz izin veya şirkete özel ek izin hakları hesaba katılmaz.
- Bakiye İK datasından alınır; sistem bakiyeyi kendisi hesaplamaz, değişimini izler.
- Bilgilendirme e-postası otomatik gönderilmez; metin ve liste kopyalanarak iletilir.

## Lisans

MIT. Ayrıntılar için `LICENSE` dosyasına bakın.
