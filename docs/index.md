# Siber Güvenlik Giriş 🛡️

Bu repo, siber güvenliğe yeni başlayanlar için **temel işletim sistemi becerileri**, **Siber Güvenlik Araçları**, **otomasyon mantığını** adım adım kazandırmayı amaçlar.

![Anasayfa Banner](./imgs/Banner_generetedbyChatGPT_2.png)

## Mevcut Bölümler

### Hafta 03 – Linux Temelleri

  Dosya sistemi, temel komutlar (`ls`, `pwd`, `cd`, `echo`, `cat`, `history`), komut yardım mekanizmaları (`man`, `--help`, `type`, `which`), terminal verimlilik kısayolları.

### Hafta 04 – Bash Scripting

Shebang, çalıştırma izinleri, değişkenler, `$(komut)` ile komut çıktısı kullanma, koşullar (`if`), döngüler (`for`, `while`), basit etkileşim (`read`).

### Hafta 05 - Pentesting

Pentesting temelleri: metodoloji, ağ keşfi ve tarama, zafiyetli laboratuvar kurulumu ve ilk exploit denemeleri.

### Hafta 06 – Web Güvenliği

- Web uygulamalarının mimarisi, HTTP temelleri, cookie/session/token yönetimi ve tarayıcı güvenlik mekanizmaları.
- OWASP Top 10 kavramları ve yaygın web zafiyetlerine genel giriş (Injection, XSS, CSRF, Broken Auth, vs.).
- Web pentest araçları: Burp Suite, OWASP ZAP, Gobuster/Dirbuster, Nikto ve proxy kullanımına dair rehber.
- Uygulamalı laboratuvarlar ve Burp Suite Academy kaynakları ile pratik alıştırmalar.
- Detaylı ders notları: [Giriş](Hafta06-WebGuvenligi/06_01_WebGuvenligineGiris.md), [Araçlar](Hafta06-WebGuvenligi/06_02_WebPentestAraclariTanitimi.md), [Proxy](Hafta06-WebGuvenligi/06_03_01_ProxyNedir.md), [Burp Suite](Hafta06-WebGuvenligi/06_03_BurpSuiteNedir.md), [Lablar](Hafta06-WebGuvenligi/06_04_BurpSuiteLabUygulamalari.md).

## Hızlı Başlangıç

1. Linux ortamı veya WSL açın.
2. Terminalde temel komutları deneyin: `pwd`, `ls -la`, `whoami`, `history`.
3. İlk Bash scriptinizi oluşturun:

```bash
nano hello.sh
#!/usr/bin/bash
echo "Merhaba, $(whoami)!"
chmod +x hello.sh
./hello.sh
```
