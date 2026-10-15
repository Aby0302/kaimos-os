# kaimos-os

KaimOS dağıtım deposu: **yalnızca imzalı ikili dosyalar**. Kaynak kod özel depoda tutulur.
Cihazlar bu depodan token olmadan indirir; güvenlik gizlilikten değil **Ed25519 imzasından** gelir.

## Paket türleri (GitHub Releases)
| Tür | Dosya | Ne için |
|---|---|---|
| Dashboard (uygulama) | `kaimos-app-<sürüm>-linux-x64.tar.gz` | `/data/kaimos/app` üzerine kurulan uygulama; hafif, sık güncellenir |
| Sistem (A/B) | `kaimos-os-<sürüm>-rootfs.ext4.zst` | pasif rootfs slotuna (rootA/rootB) yazılan sistem imajı; seyrek güncellenir |
| Tam imaj | `kaimos-os-<sürüm>-disk.img.zst` | ilk kurulum / yeniden yazma (USB, VM) |
| Doğrulama | `SHA256SUMS`, `SHA256SUMS.sig` | tüm dosyaların özeti ve Ed25519 imzası |

Her sürümde `SHA256SUMS.sig`, imaja gömülü `/etc/kaimos/update.pub.pem` genel anahtarıyla doğrulanır.
İmzasız ya da eski sürüm cihazda reddedilir (`/usr/libexec/kaimos-update`).

## Güncelleme akışı
- **Uygulama:** indirilir → imza + SHA-256 doğrulanır → `app.new` → servis başlarken yerleştirilir → 30 sn içinde doğrulanmazsa geri alınır.
- **Sistem (A/B):** pasif slota yazılır → bir kez denenerek açılır → sağlıklı değilse otomatik eski slota dönülür.

## Depo kuralları
- Bu depoya kaynak kod, anahtar ya da sır konmaz.
- Özel imza anahtarı yalnızca kaynak deponun GitHub Secrets'ında (`UPDATE_SIGNING_KEY`) durur.
