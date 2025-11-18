# 🧩 **BÖLÜM 2 – Dosya Sistemi, Yetkiler ve Root Gücü**

---

## 🎯 **Bölümün Öğrenme Hedefleri**

Bu bölümün sonunda öğrenciler:

- Linux dosya sisteminin yapısını ve dizinlerin amacını açıklayabilecek,
- Dosya izinlerini (`rwx`) ve sahiplik kavramını anlayabilecek,
- Kullanıcı ve grup yönetimi yapabilecek,
- Root kullanıcı haklarının farkını uygulamalı olarak deneyimleyecek.

---

## 📁 **2.1 Linux Dosya Sistemi Hiyerarşisi**

Linux dosya sistemi, bir **ağaç yapısındadır**. En tepede `/` (root dizini) bulunur.

### 📂 **Temel Dizinler:**

| Dizin | Açıklama |
| --- | --- |
| `/` | En üst dizin (root of filesystem) |
| `/home` | Kullanıcı dizinleri (`/home/student`) |
| `/etc` | Sistem yapılandırma dosyaları |
| `/bin` | Temel sistem komutları (`ls`, `cp`, `mv` vb.) |
| `/sbin` | Yönetici komutları (`reboot`, `ifconfig` vb.) |
| `/var` | Değişken veriler (log, cache, spool) |
| `/tmp` | Geçici dosyalar |
| `/root` | Root kullanıcısının ana dizini |
| `/usr` | Kullanıcı programları ve araçlar |
| `/lib` | Sistem kütüphaneleri |

**Uygulama:**

```bash
cd /
ls
```

## 🔐 **2.2 Dosya İzinleri (rwx) Mantığı**

🔗 https://www.redhat.com/en/blog/linux-file-permissions-explained

Linux’ta her dosyanın **üç ayrı kullanıcı tipi** için izinleri vardır:

| Kullanıcı Türü | Açıklama |
| --- | --- |
| **User (u)** | Dosyanın sahibi |
| **Group (g)** | Dosyanın ait olduğu grup |
| **Others (o)** | Diğer herkes |

Her biri için 3 temel izin tipi vardır:

| İzin | Kısaltma | Sayısal Karşılık | Açıklama |
| --- | --- | --- | --- |
| Read | r | 4 | Okuma hakkı |
| Write | w | 2 | Yazma hakkı |
| Execute | x | 1 | Çalıştırma hakkı |

**Toplam:**

İzinler sayısal olarak toplanır → örnek: `rwx = 7`, `rw- = 6`, `r-- = 4`

---

### 🧩 **Örnek:**

Komut:

```bash
ls -l

```

Çıktı:

```
-rw-r--r-- 1 student student 28 Oct 30 12:40 notes.txt
```

**Açıklama:**

- Dosya Tipi `-` → normal dosya
- `rw-` → kullanıcı (okuma + yazma)
- `r--` → grup (okuma)
- `r--` → diğerleri (okuma)

Yani:

> Sadece dosya sahibi yazabilir, herkes okuyabilir.
> 

---

```bash
-rw-r--r-- 1 student student 28 Oct 30 12:40 notes.txt

- : [ilk harf] normal dosya tipi
rw- [sonraki 3 harf]: dosya sahibi hakları
r-- [2. 3 harf]: user group
r-- [3. 3 harf]: diğerleri
1 : Bağlantı sayısı (hard link count). Dosyaya kaç farklı adla erişilebildiğini gösterir.

student: Dosya sahibi (User Owner)
student (2. student): Grup Kullanıcı (Group Owner)
```

## 🧰 **2.3 chmod, chown ve chgrp Kullanımı**

### 🧱 **chmod – izin değiştirme**

**Sözdizimi:**

```bash
chmod [izin] [dosya]

```

### 🔹 Sayısal Yöntem:

```bash
chmod 755 script.sh

```

→ `rwxr-xr-x` anlamına gelir (kullanıcı:7, grup:5, diğer:5)

- Owner: rwx = 4+2+1 = 7
- Group: r-x = 4+0+1 = 5
- Others: r-x = 4+0+1 = 5

### 🔹 Harfsel Yöntem:

```bash
chmod u+x file.txt   # Kullanıcıya çalıştırma izni ver
chmod g-w file.txt   # Gruptan yazma iznini al
chmod o-r file.txt   # Diğerlerinden okuma iznini al

```

---

### 👑 **chown – dosya sahibini değiştirme**

```bash
sudo chown root file.txt
sudo chown student:student file.txt

```

### 👥 **chgrp – grubu değiştirme**

```bash
sudo chgrp admins file.txt

```

---

### 🧪 **Uygulama: Dosya İzni Deneyi**

### Görev:

Bir dosya oluştur, izinlerini değiştir, farklı kullanıcıyla erişmeyi dene.

**Adımlar:**

```bash
cd ~/Documents
touch secret.txt
echo "Bu özel bir dosyadır." > secret.txt

ls -l secret.txt     # izinleri incele
chmod 600 secret.txt # sadece sahibi erişebilsin
ls -l secret.txt

```

**Yeni Kullanıcıyla Deneme:**

```bash
sudo adduser testuser
su testuser
cd /home/student/Documents
cat secret.txt

```

📌 **Beklenen Hata:**

```
cat: secret.txt: Permission denied

```

💡 **Açıklama:**

`chmod 600` → sadece dosya sahibi okuyabilir/yazabilir, diğer herkes yasaklıdır.

Bu tür izinler, hassas parola dosyaları için yaygın olarak kullanılır (örneğin `/etc/shadow`).

---

## 👥 **2.4 Kullanıcı ve Grup Yönetimi**

### 👤 **Kullanıcı Oluşturma:**

```bash
sudo adduser ali
```

→ Sistem şifre ister ve kullanıcı oluşturur.

### 🔑 **Parola Değiştirme:**

```bash
sudo passwd ali
```

### 🔄 **Kullanıcı Değiştirme:**

```bash
su ali
whoami
```

### 👥 **Grup Görüntüleme:**

```bash
groups
```

### 🧹 **Kullanıcı Silme:**

```bash
sudo deluser ali
```

---

## ⚡ **2.5 Root Kullanıcısı ve sudo Komutu**

**Anlatım:**

- **root** Linux’taki en güçlü kullanıcıdır.
- Her şeyi yapabilir — bu da güvenlik riski taşır.
- Günlük kullanımda **sudo** tercih edilir (super user do).

### 🧩 **Örnek:**

```bash
sudo apt update
```

→ Sistem paketlerini günceller.

`sudo` kullandığınızda şifre istenir, böylece yetkisiz işlemler engellenir.

---

### 🧪 **Mini Uygulama: Root Yetkisini Görmek**

**1. Normal kullanıcı olarak:**

```bash
cat /etc/shadow
```

📌 Beklenen çıktı:

```
Permission denied
```

**2. sudo ile:**

```bash
sudo cat /etc/shadow
```

📌 Şifre ister ve dosyayı gösterir.

💡 **Açıklama:**

`/etc/shadow` sistemdeki tüm kullanıcıların şifre hash’lerini içerir, bu yüzden sadece root erişebilir.

https://www.cyberciti.biz/faq/understanding-etcshadow-file/

https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/

---

## ⚠️ **2.6 Yaygın Hatalar ve Güvenlik Notları**

| Hata | Açıklama | Çözüm |
| --- | --- | --- |
| `Permission denied` | Yetkisiz işlem | `sudo` kullan veya dosya izinlerini kontrol et |
| `User already exists` | Kullanıcı zaten mevcut | Farklı bir isim seç |
| `chown: Operation not permitted` | root izni gerek | `sudo chown` kullan |
| `su: Authentication failure` | Parola yanlış | Doğru kullanıcı/şifre kombinasyonu gir |

---

## 🧪 **2.7 Uygulama Görevi**

**Görev Adı:**

🔐 *“Yetkili Dosya Deneyi”*

**Senaryo:**

Kullanıcınızın erişebildiği özel bir dosya oluşturun, sonra diğer kullanıcıyla erişmeyi deneyin.

**Adımlar:**

```bash
cd ~
touch gizli.txt
echo "Bu dosya yalnızca bana ait." > gizli.txt
chmod 600 gizli.txt
sudo adduser deneme
su deneme
cat /home/student/gizli.txt

```

**Beklenen Sonuç:**

> “Permission denied” hatası alınmalı.
> 

**Ek Görev:**

Root olarak dosyayı açın (`sudo su`) ve içeriğini okuyun.

Sonra `chmod 644 gizli.txt` yapıp tekrar test edin — artık herkes okuyabilir.

---

## 🧠 **2.8 Tartışma ve Değerlendirme**

**Tartışma Soruları:**

1. `chmod 777` neden güvenli değildir?
2. `sudo` ile her şeyi yapabiliyorsak neden root hesabı kullanmak tehlikelidir?
3. Hangi sistem dosyaları sadece root tarafından değiştirilebilir?

Bu bölüm, sızma testlerinde *dosya izinlerini suistimal etme* veya *yanlış yapılandırılmış sistemleri tespit etme* becerisine temel oluşturur.

---