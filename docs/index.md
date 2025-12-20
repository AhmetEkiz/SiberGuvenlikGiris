# Siber Güvenlik Giriş 🛡️

Bu depo, siber güvenliğe yeni başlayanlar için **temel işletim sistemi becerileri** ve **otomasyon mantığını** adım adım kazandırmayı amaçlar. İlk haftalar Linux komut satırı hakimiyeti ve Bash ile betik yazma (scripting) temellerine odaklanır; sonraki haftalarda güvenlik odaklı uygulamalara (ağ tarama, log analizi, temel zafiyet kavramları, otomasyon senaryoları) geçilecek.

## Mevcut Bölümler

#### **Hafta 03 – Linux Temelleri:**
  - Dosya sistemi, temel komutlar (`ls`, `pwd`, `cd`, `echo`, `cat`, `history`), komut yardım mekanizmaları (`man`, `--help`, `type`, `which`), terminal verimlilik kısayolları.
#### **Hafta 04 – Bash Scripting:**
  - Shebang, çalıştırma izinleri, değişkenler, `$(komut)` ile komut çıktısı kullanma, koşullar (`if`), döngüler (`for`, `while`), basit etkileşim (`read`).
#### **Hafta 05 - Pentesting:**
  - Pentesting temelleri: metodoloji, ağ keşfi ve tarama, zafiyetli laboratuvar kurulumu ve ilk exploit denemeleri.

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
