# PharmacyApp — yayın manifesti

Eczane kutularının hangi sürümü çalıştıracağını söyleyen tek dosya: **`releases.json`**.

Bu depo **public** ve öyle olmak zorunda: kutular manifesti kimlik doğrulaması olmadan
okuyor. İçinde sır yok — yalnızca sürüm numarası, yayın tarihi ve eczacıya gösterilen not.
Kaynak kod ayrı bir private depoda.

## Nasıl çalışıyor

Kutular saatte bir bu dosyayı okuyor (`scripts/update.sh`) ve **kendi kanalındaki** sürüm
farklıysa **ve** `releaseAt` geçmişse güncelliyor. Çekmeyi kutu başlatır; merkez kutuya
bağlanmaz — elli eczane makinesinin sabit adresi ve açık portu yok.

## Kanallar

| Kanal | Kim | Ne zaman |
| --- | --- | --- |
| `canary` | Kendi eczanemiz | Her sürüm ilk gün buraya düşer |
| `early` | 2-3 pilot eczane | Canary'de bir hafta sorunsuzsa |
| `stable` | Geri kalan kurulumlar | Early'de bir hafta sorunsuzsa |

**Kademeli yayın hatalı bir sürümün elli eczaneye aynı anda gitmesini engelleyen tek
mekanizma.** Migration'lar açılışta uygulanıyor ve geri alınamıyor.

## Alanlar

| Alan | Ne işe yarıyor |
| --- | --- |
| `version` | **Üç parçalı olmak zorunda** (`1.3.1`). İki parçalı (`1.3`) hareketli bir etiket: bir sonraki yamada başka bir imajı işaret ediyor ve kutu yeni yamayı hiç almaz. |
| `releaseAt` | Kutu bu tarihten **önce** güncellemiyor. İleriye yazmak yayının ne zaman olacağını önceden bilinir kılıyor. |
| `notes` | Eczacıya gösterilen tek cümlelik özet. **İş dilinde** yazılmalı. |
| `highlights` | Madde madde değişiklikler. Teknik değil kullanıcı gözünden. |

> **Sürüm notları eczacının kararına giriyor:** güncelleme penceresinde "Şimdi güncelle"
> ile "Daha sonra" arasında seçim yapıyor ve bu ancak neyi ertelediğini biliyorsa anlamlı.
> "ExpiryRisk üreticisi eklendi" değil, "Miadı yaklaşan ilaçlar için uyarı geliyor".

## Bu dosya elle düzenlenmiyor

Kaynağı ana depodaki `deploy/releases.json`; oradaki değişiklik bir workflow ile buraya
kopyalanıyor (`publish-manifest.yml`). Buraya elle yazılan bir değişiklik bir sonraki
yayında üzerine yazılır.

**İmaj basmak ile yayına almak ayrı iki adım:** etiket atmak imajı üretiyor, yayın ise bu
dosyada o kanalın sürümünü değiştirmek demek.
