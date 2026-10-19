# QNB Emulator Capture — ARM64 GitHub Runner

Bu repo, QNB Mobil APK'sını GitHub Actions ARM64 runner'ında (KVM'li) gerçek Android
emülatörde çalıştırıp tüm güvenlik katmanı trafiğini (Codevo Octopus + Seal) mitmproxy
ile yakalar.

## Workflow
- `.github/workflows/emulator-capture.yml` — workflow_dispatch ile tetiklenir
- `run_login=true` verilirse login ekranı denemesi de yapar

## Çıktılar (artifact: capture)
- `flows.dump` — mitmproxy flow kaydı (tüm HTTPS trafiği)
- `mitm.log` — mitmproxy ayrıntılı log (URL + body özetleri)
- `screen*.png` — ekran görüntüleri
- `ui.xml` — uiautomator dump
- `logcat*.txt` — cihaz logları
- `emu.log` — emulator log

## Notlar
- APK 160MB > GitHub 100MB limiti → `apk-parts/` ve `split-parts/` altında parçalı,
  workflow'da `cat` ile birleştirilir.
- System CA: google_apis imajı adb root destekler, CA `/system/etc/security/cacerts`'e kopyalanır.
- Proxy: `10.0.2.2:8080` (host'taki mitmproxy).
