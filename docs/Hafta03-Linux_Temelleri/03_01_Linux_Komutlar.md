# **🧩 BÖLÜM 1 – Linux Komutlar**

## 🎯 **Bölümün Öğrenme Hedefleri**

Bu bölümün sonunda öğrenciler:

- Linux’un siber güvenlikte neden tercih edildiğini açıklayabilecek,
- Terminal arayüzünü etkin şekilde kullanabilecek,
- Temel komutlarla dosya sistemi içinde gezinebilecek,
- Basit dosya işlemleri (oluşturma, listeleme, görüntüleme) yapabilecek.

## 📖 **1.1 Linux’un Siber Güvenlikteki Önemi**

- Linux, açık kaynaklı bir işletim sistemidir.
- Siber güvenlikte tercih edilmesinin 3 ana nedeni vardır:
    1. **Açık Kaynak Kod** → Herkes tarafından denetlenebilir, arka kapı riski düşüktür.
    2. **Terminal Gücü** → Komut satırıyla derin sistem erişimi sağlar.
    3. **Dağıtım Esnekliği** → Kali, Parrot gibi özel güvenlik dağıtımları mevcuttur.

> 💡 Not: Kali Linux, Debian tabanlıdır ve sızma testleri için özel araçlarla gelir.

Araştırma Sorusu: Debian tabanlı ne demektir?

**Güvenlik Perspektifiyle Örnek:**

- Bir sızma testçisi (penetration tester), ağdaki zayıflıkları tararken `nmap`, `netcat` gibi araçları kullanır — bu araçlar genellikle Linux ortamında çalışır.
- Sunucu güvenliği sağlanırken, sistem yöneticileri log dosyalarını analiz eder (`/var/log`), firewall yapılandırmaları yapar (`ufw`, `iptables`) — yine Linux üzerinde.

## 💻 **1.2 Terminale Giriş – CLI vs GUI**

- GUI (Graphical User Interface): Görsel arayüz (pencereler, ikonlar).
- CLI (Command Line Interface): Komut satırı.

→Siber güvenlikte **CLI tercih edilir**, çünkü:

- Otomasyon mümkündür (bash scriptler),
- Ağ üzerinde uzaktan yönetim kolaydır (SSH),
- Log analizi, ağ taraması gibi işlemler terminal komutlarıyla yapılır.

**Uygulama (Terminal Açılışı):**

- Öğrencilere terminal açtır:

`Ctrl + Alt + T`

```bash
# Output:
# user@kali:~$
```

- `user`: oturum açan kullanıcı
- `@`: ayraç
- `kali`: makine adı
- `~`: home dizinini temsil eder
- `$`: normal kullanıcı (root olsaydı `#` olurdu)

### ⚡ 1.2.1  Linux terminal (bash) klavye kısayolları

En çok kullanılan **bash terminal kısayolları**

| Kısayol | Açıklama |
| --- | --- |
| `Ctrl + C` | Çalışan komutu durdurur (SIGINT sinyali gönderir) |
| `Ctrl + Z` | Çalışan komutu askıya alır (background’a atar) |
| `fg` | Askıya alınan işlemi öne getirir |
| `Ctrl + D` | Terminal oturumunu kapatır (EOF gönderir) |
| `Ctrl + L` | Ekranı temizler (`clear` komutuna eşdeğer) |
| `Ctrl + A` | Satırın başına gider |
| `Ctrl + E` | Satırın sonuna gider |
| `Ctrl + U` | İmleçten satır başına kadar olan kısmı siler |
| `Ctrl + K` | İmleçten satır sonuna kadar siler |
| `Ctrl + W` | İmleçten önceki kelimeyi siler |
| `!!` | Son komutu yeniden çalıştırır |
| `!ls` | En son `ls` komutunu yeniden çalıştırır |
| `Tab` | Otomatik tamamlama |
| `↑ / ↓` | Komut geçmişinde gezme |
| `history` | Komut geçmişini gösterir |
| `Ctrl + R` | Geçmişte arama yapar (örnek: “ssh” yazarsan geçmişteki ssh komutlarını bulur) |
| `q` | `man`, `less`, `more` gibi sayfa görüntüleyicilerde çıkış yapmak için vb. durumlarda aktif edilir. |

## **⚙️ 1.3 Temel Linux Komutları**

### 📁 **1.3.1 Dizin Yönetimi Komutları**

| Komut | **Description** | Açıklama | Örnek |
| --- | --- | --- | --- |
| `pwd` | Print current working directory. | Şu an bulunduğun dizini gösterir. | `/home/student` |
| `ls` | List files and directories. | Klasördeki dosyaları listeler. | `ls -la` gizli dosyaları da gösterir. |
| `cd` | Change directory. | Dizin değiştirir. | `cd /etc` veya `cd ..` (geri git) |
| `mkdir` | Create a new directory. | Yeni klasör oluşturur. | `mkdir testdir` |
| `rmdir` | Safely remove empty directories | Boş klasörü siler. | `rmdir testdir` |

**🧠Ekstra Bilgi:** Linux’ta klasör ayırıcı `/` , Windows’ta `\` işaretidir.

### 📄 **1.3.2 Dosya Görüntüleme ve İçerik Komutları**

| Komut | | Açıklama | Kullanım |
| --- | --- | --- | --- |
| `cat` | View the contents of a file. | Dosya içeriğini gösterir. | `cat notes.txt` |
| `less` | | Uzun dosyaları sayfa sayfa gösterir. | `less /etc/passwd` |
| `head` | | Dosyanın ilk 10 satırını gösterir. | `head file.log` |
| `tail` | | Son 10 satırı gösterir. | `tail file.log` |
| `echo` | | Ekrana veya dosyaya yazı yazar. | `echo "Merhaba Linux"` |

### 🔍 **1.3.3 Bilgi Alma Komutları**

| Komut | Açıklama | Kullanım |
| --- | --- | --- |
| `whoami` | Aktif kullanıcı adını gösterir. | `whoami` |
| `history` | Komut geçmişini listeler. | `history` |
| `man <komut>` | Komutun kullanım kılavuzunu gösterir. | `man ls` |
| `clear` | Terminal ekranını temizler. | `clear` |

### ⚠️ **1.3.4 Yaygın Hatalar ve Açıklamaları**

| Hata | Açıklama | Çözüm |
| --- | --- | --- |
| `bash: command not found` | Komut yanlış yazılmış. | Yazım hatasını düzelt. |
| `Permission denied` | Yetki yok. | `sudo` ile çalıştır veya dosya izinlerini kontrol et. |
| `No such file or directory` | Belirtilen dosya yok. | Yolun doğru olduğundan emin ol. |

### ❓ 1.3.5 Help Komutu

`help` komutu, **kabuk (shell)** içinde yerleşik (built-in) komutlar hakkında bilgi verir.

Yani bu komut, **bash’in kendi komutları** için yardım sağlar — sistemdeki harici programlar için değil.

```bash
# Bu, Bash shell’deki tüm yerleşik komutların listesini gösterir.
help 

# cd (change directory) komutu hakkında bilgi verirs
help cd
```

### ❓ 1.3.6 Neden `help ls` çalışmaz?

Çünkü `ls` bir shell built-in komutu değildir. `ls`, aslında `/bin/ls` konumunda bulunan **ayrı bir programdır** (GNU coreutils paketi içinde).
Yani:

- `help` sadece **bash’in içindeki** komutlar için geçerlidir.
- `ls` gibi komutlar **dış programlardır**, dolayısıyla `help` onlarda işe yaramaz.

🔹 Bunun yerine şunları kullanabilirsin:

```bash
man l
ls --help
```

### 🧩 1.3.7. `ls` gibi komutlar nasıl çalışır, nasıl öğrenebilirim?

```bash
# bu programın hangisinin kullanıldığı gösterilir.
which ls
which python 
```

| Komut | Açıklama | Örnek |
| --- | --- | --- |
| `type <komut>` | Komutun türünü gösterir (built-in mi, external mi?) | `type ls` |
| `which <komut>` | Komutun tam dosya yolunu gösterir | `which ls` |
| `file $(which ls)` | Komut dosyasının türünü gösterir | `file $(which ls)` |
| `man <komut>` | Manual (kılavuz) sayfasını açar | `man ls` |
| `<komut> --help` | Hızlı yardım mesajı verir | `ls --help` |

Örnek:

```bash
type ls
# Çıktı: ls is /bin/ls
```

Yani `ls`, `/bin` dizininde bulunan bir **binary program**.

### 🧩 1.3.8. Neden `help` komutu veya bu komutları kullanmak önemli?

Siber güvenlikte **bilgiye hızlı erişim ve self-learning (kendi kendine öğrenme)** çok önemlidir.

Çünkü her sistem farklıdır — her zaman internet erişimin olmayabilir (örneğin bir izole laboratuvar veya CTF ortamında).

### 🗒️ 1.3.6 Peki Bu Kadar Komutu Nereden Bileceğim?

CheatSheet’ler: Online ve Offline(PDF, image vb.) şeklinde kopya kağıdı denen şeyler mevcut.

İnternette “Linux CheatSheet” olarak arayabilirsiniz. Web sayfası olarak bir kaç örnek:

- [https://www.geeksforgeeks.org/linux-unix/linux-commands-cheat-sheet/](https://www.geeksforgeeks.org/linux-unix/linux-commands-cheat-sheet/)
- [https://hostafrica.co.za/blog/linux/linux-commands/](https://hostafrica.co.za/blog/linux/linux-commands/)

> Chatgpt vb. araçlara danışmadan, bu ve benzeri dökümanlardan kendi çabamızla bulmak öğrenmemizi pekişitirir.

## 🧪 **1.4 Uygulamalı Alıştırma**

### 🎯 **Görev 1:**

Kendi kullanıcı adınızı içeren bir dosya oluşturun ve içeriğini görüntüleyin.

```bash
cd ~                     # Ana dizine git
mkdir Documents          # Documents klasörü oluştur
cd Documents             # Klasöre gir
echo "Ben $(whoami)" > notes.txt  # Kullanıcı adını yaz
cat notes.txt            # Dosya içeriğini görüntüle

# Output:
# Ben student
```

### 🎯 **Görev 2:**

```bash
# Dosyayı Desktop klasörüne taşı:
mv ~/Documents/notes.txt ~/Desktop/

# Dosyayı sil:
rm ~/Desktop/notes.txt
```

### 🎯 **Görev 3:**

Hangi python sürümü bilgisayarınızda yüklü?

```bash
which python
```
