# Neltra E2E

Android emulator + [Maestro](https://maestro.mobile.dev) smoke testləri.

Bu repo **yalnız test üçündür** — tətbiq kodu ayrı (private) repodadır.
Build burada aparılmır: release APK bu repo-nun **release asset**-i kimi
yüklənir, workflow onu emulyatora quraşdırıb flow-ları işlədir.

## Nə üçün ayrı public repo?

GitHub Actions private repo-larda hesab limitinə düşür; public repo-larda
limitsiz və pulsuzdur. Emulyator (KVM) yalnız GitHub-hosted runner-də
mümkündür — Modal-da `/dev/kvm` yoxdur.

## Axın

1. Private repoda APK build olunur (Modal və ya CI).
2. APK bu repo-ya release asset kimi yüklənir (`app-release.apk`).
3. **Actions → E2E → Run workflow** (`apk_tag` = `latest` və ya tag adı).
4. Emulator qalxır → APK quraşdırılır → `flows/*.yaml` işləyir.
5. Nəticələr (screenshot + log) `maestro-results` artifact-ində.

## Flow-lar

| Fayl | Nə yoxlayır |
|---|---|
| `flows/01-onboarding.yaml` | İlk açılış → dil seçimi → ana ekran |
| `flows/02-settings.yaml` | Parametrlər ekranı → bölmələr → geri |
| `flows/03-ui-language.yaml` | Arayüz dili AZ → TR → AZ |

Flow-lar private repodaki `.maestro/` qovluğundan **avtomatik** sinxronlaşır
(`scripts/sync-e2e.sh`). Burada əl ilə düzəliş etmə — növbəti sinxronda
üzerinə yazılacaq.

## Selektor qaydası

Maestro mətni **tam uyğunluqla regex** kimi yoxlayır. Flutter bir kartın bütün
mətnlərini tək accessibility node-unda birləşdirir, ona görə nümunə:

```yaml
- tapOn: "(?s).*Azərbaycanca.*"
```
