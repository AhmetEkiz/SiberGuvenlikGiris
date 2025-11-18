# **Bash Scripting**

---

## 1. Nedir?

## $ Bash ve Bash Scripting

**Bash (Bourne Again Shell):** Linux ve Unix sistemlerde en yaygın kullanılan komut yorumlayıcısı (shell)’dir. Yani kullanıcı ile işletim sistemi arasında bir ara katmandır.  En yaygın kullanılan komut yorumlayıcısıdır.

**Bash scripting:** Bash komutlarını **tek tek yazmak yerine bir dosyaya kaydedip otomatik çalıştırmak** anlamına gelir.  Bu dosyalara genellikle `.sh` uzantısı verilir (örneğin `backup.sh`).

Bir Bash script:

- Aslında sırayla çalışan Bash komutlarından oluşur.
- Tek seferde çalışarak **otomasyonu** sağlar.

## 💬 **Shell**

- Kullanıcının komutlarını alıp kernel’e ileten **ara katman**dır.
- Komut yorumlayıcısıdır (command interpreter).
- Çeşitleri:
    - **Bash (Bourne Again Shell)** — en yaygın olanı
    - **sh (Bourne Shell)**
    - **zsh**, **fish**, **ksh, csh, ksh, zsh** — alternatif shell’ler

**Örnek:** Terminalde `ls` yazarsan, shell bu komutu kernel’e gönderir, o da dosya listesini döndürür.

## 🧠 **Kernel (Çekirdek)**

- İşletim sisteminin “**beyni**”dir.
- Donanım (CPU, RAM, disk, ağ kartı) ile yazılımlar arasındaki **köprüyü kurar**.
- Kullanıcı doğrudan kernel ile konuşmaz; komutlar kernel’e **shell** üzerinden gider.

**Kısaca:**

Kullanıcı → Shell → Kernel → Donanım

---

# 2. Başlangıç — script dosyası oluşturma ve çalıştırma

## 2.1. İlk Kod

```bash
# Bash komut yorumlayıcısı konumu neresi?
which bash

# Output
# /bin/bash
```

Bash Scripti oluşturma:

```bash
nano hello_world.sh
```

```bash
#!/usr/bin/bash   
# dosya: hello.sh
echo "Merhaba, $(whoami)!"
```

### 📍 **Shebang (#!) Nedir?**

Script dosyalarının **ilk satırında** bulunan özel bir ifadedir.

Örnek:

```
#!/usr/bin/bash
```

Bu satırda:

- `#!` → **shebang** işaretidir.
- Ardından gelen yol (`/usr/bin/env bash` veya `/bin/bash`) → script’in hangi bash programı veya başka program ile çalıştırılacağını belirtir.

### Çalıştırma:

```bash
chmod +x hello.sh
./hello.sh
```

Alternatif: `bash hello.sh` (izne gerek yok)

Linux’ta her dosyanın **izinleri (permissions)** vardır:

- **r** → read (okuma)
- **w** → write (yazma)
- **x** → execute (çalıştırma)

### **`chmod +x hello.sh` Tam Olarak Ne Anlama Gelir?**

- Komut çıktısı: `files=$(ls -1)` veya `files=`ls -1``
- Default değer: `${var:-default}` — `var` tanımsızsa `default` kullan.
- Atama ile expansion: `: "${VAR:?VAR required}"` — eksikse script durur.

---

## 2.2. Değişkenler, expansion & quoting

- Atama: `name="Ahmet"` (no spaces around `=`)
- Okuma: `echo "$name"` — **her zaman çift tırnak** kullanın (`"$var"`), aksi halde word-splitting ve globbing olur.

### Örnek:

❓Bu örnekten önce öğrendiğiniz dosya oluşturma bash komutuna ile bir dosya oluşturun.

❓Aşağıdaki örnekte, bir dosyanın içeriğinin okunması için gerekli kod verilmiştir. Bir bash script oluşturup bu kodu içine kopyalayın ve gerekli değişikliği yapın.

```bash
#!/usr/bin/bash
USERNAME="ahmet"
path="Documents/klasor1/dosya.txt"

file="/home/${USERNAME}/${path}"

echo "File : $file"
echo "Dosya İçeriği:\n"
cat file
```

---

❓ Kullanıcı adını Linux komutları ile bulunabiliyor, burada bir değişkene bu komutu kullanarak atama yapalım.

## 2.3. Yorum Satırı:

Yorum satırı `#` sembolünden sonra yazılanlar yorum olur ve bash tarafından işlenmez.

```bash
#!/usr/bin/bash

# kullanıcı adını tanımladık
USERNAME=$(whoami)! 

# dosya yolunu gerekliyse değiştir
path="Documents/klasor1/dosya.txt"

file="/home/${USERNAME}/${path}"  # dosya adının birleştirilmesi

echo "File : $file"
echo "Dosya İçeriği:\n"
cat file
```

## 2.4. Koşullar (if / else)

```bash
if [[ -f "$file" && -r "$file" ]]; then
  echo "$file var ve okunabilir"
else
  echo "dosya yok veya okunamaz"
fi
```

`[[ ... ]]` kullanımı: daha güvenli, regex ve `&&`/`||` içinde daha iyi.

Dosya testleri:

- `f` dosya
- `d` dizin
- `r` okunabilir
- `w` yazılabilir
- `s` boş değil

String karşılaştırma:

- `[[ "$a" == "$b" ]]`, `[[ -z "$a" ]]` (boş mu)

Sayısal karşılaştırma: `-eq`, `-ne`, `-lt`, `-gt` veya `(( a < b ))` ile aritmetik.

### if-else Sorusu

❓ Örnek soru Kullanıcıdan 2 sayı girmesini isteyin ve değişkenlere kaydedin ve 2 değerin hangisinin daha büyük olduğunu karşılaştırın.

✅ Çözüm:

```bash
echo "ilk değeri gir:"
read deger1
echo "ikinci degeri gir:"
read deger2

if [ "$deger1" -gt "$deger2" ]; then
 echo "deger1 deger2'den buyuktur."
elif [ "$deger1" -eq "$deger2" ]; then
 echo "deger1 ve deger2 esittir."
else
 echo "deger2 deger1'den buyuktür."
fi
```

---

### D. Döngüler

`for` örneği:

```bash
#!/bin/bash
for i in {1..5}
do
 echo $i
done
```

```bash
#!/bin/bash
for i in {1..5}; do
 echo $i
done
```

`while` örneği:

```bash
count=0
while [ count < 5 ]; do
  echo "Sayaç: $count"
  ((count++))
done
```

### Döngü Sorusu

❓1 saniye aralıklarla sayan bir döngü yazalım.

✅ Çözüm:

```bash
#!/bin/bash
for i in {1..5}; do
 echo $i
 sleep 1
done
```

---

# Bash Scripting ve Linux Komutları için Ek Kaynaklar

## Türkçe

- https://www.yusufsezer.com.tr/linux-bash/

## İngilizce

- https://www.w3schools.com/bash/index.php
- https://linuxconfig.org/bash-scripting-tutorial
- https://www.freecodecamp.org/news/bash-scripting-tutorial-linux-shell-script-and-command-line-for-beginners/

# Kaynaklar

- https://www.geeksforgeeks.org/linux-unix/different-shells-in-linux/
- https://www.yusufsezer.com.tr/linux-bash/
- https://www.digitalocean.com/community/tutorials/different-types-of-shells-in-linux
- https://tr.eitca.org/cybersecurity/eitc-is-lsa-linux-system-administration/bash-scripting/how-bash-scripts-work/examination-review-how-bash-scripts-work/what-is-the-shebang-line-in-a-bash-script-and-why-is-it-important/