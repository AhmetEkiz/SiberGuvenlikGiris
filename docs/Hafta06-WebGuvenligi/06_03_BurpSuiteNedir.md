# 06.03. Burp Suite Nedir?

---

# 1. Burp Suite Nedir?

**Burp Suite**, web uygulamalarının güvenlik testlerini (Web Pentest) yapmak için kullanılan, sektör standardı bir **entegre web güvenliği test platformudur**. [PortSwigger](https://portswigger.net/) tarafından geliştirilmiştir ve hem **manuel** hem de **yarı otomatik** testler için kullanılır.

Temel olarak Burp Suite:

- Tarayıcı ile web uygulaması arasına **proxy** olarak girer.
- HTTP/HTTPS trafiğini **yakalar, analiz eder ve değiştirmenize** imkân tanır.
- Zafiyetlerin (OWASP Top 10 gibi) tespitinde aktif rol oynar.

Etik hacker’lar, sızma testi uzmanları ve güvenlik ekipleri tarafından yaygın şekilde kullanılır.

# 2. Burp Suite Hangi Amaçlarla Kullanılır?

- Kimlik doğrulama (authentication) ve yetkilendirme (authorization) testleri
- SQL Injection, XSS, CSRF gibi web zafiyetlerinin analizi
- Form ve parametre manipülasyonu
- Oturum (session) yönetimi testleri
- API güvenlik testleri

## 2.1. Burp Suite ile Tespit Edilebilecek Yaygın Zafiyetler

- **SQL Injection:**
    - Uygulamanın kullanıcı girdilerini filtrelemeden SQL sorgularına eklemesi sonucu, saldırganın veritabanı sorgularını manipüle edebilmesidir. Yetkisiz veri okuma, veri silme veya admin erişimi ile sonuçlanabilir.
- **Cross-Site Scripting (XSS):**
    - Kullanıcıdan alınan zararlı JavaScript kodlarının tarayıcıda çalıştırılmasına imkân veren zafiyettir. Oturum çalma, keylogging ve kullanıcı yönlendirme gibi saldırılara yol açar.
- **IDOR:**
    - Yetkilendirme kontrolü yapılmadan nesnelere (user id, dosya id vb.) doğrudan erişim sağlanmasıdır. Saldırgan başka kullanıcılara ait verilere erişebilir.
- **CSRF:**
    - Kullanıcının oturumu açıkken, onun adına istek gönderilmesini sağlayan zafiyettir. Parola değiştirme veya para transferi gibi kritik işlemler tetiklenebilir.
- **Broken Authentication:**
    - Kimlik doğrulama ve oturum yönetiminin hatalı uygulanmasıdır. Zayıf şifreler, session fixation veya token tahmin edilebilirliği gibi problemleri kapsar.
- **Security Misconfiguration:**
    - Varsayılan ayarlar, gereksiz servisler, açık debug modları veya hatalı HTTP header’ları nedeniyle oluşan güvenlik açıklarıdır. En yaygın ama en kolay önlenebilir zafiyetlerdendir.

---

# 3. Burp Suite Sürümleri

| Sürüm | Özellik |
| --- | --- |
| **Community** | Ücretsiz, manuel testler için yeterli |
| **Professional** | Ücretli, otomatik tarayıcı (Scanner), gelişmiş Intruder |
| **Enterprise** | Kurumsal, sürekli güvenlik taramaları |

Bu tutorial **Burp Suite Community** üzerinden ilerlemektedir.

# 4. Burp Suite Temel Bileşenleri

## 4.1. Proxy

Burp Suite’in çekirdeğidir. Tarayıcı ile web uygulaması(web sitesi) arasına girerek tüm HTTP/HTTPS trafiğini yakalar, durdurur(intercept) ve değiştirmenizi sağlar.

![./imgs/06_03/image.png](./imgs/06_03/image.png)

https://www.geeksforgeeks.org/computer-networks/what-is-proxy-server/

**Proxy kavramı nedir:** [06.04.01. Proxy Nedir?](06_04_01_ProxyNedir.md) 

**Teknik İşlevi:**

- Client ↔ Server arasına **Man-in-the-Middle** olarak girer
- HTTP request ve response’ları **ham (raw)** haliyle gösterir
- Trafiği durdurabilir (Intercept), değiştirebilir veya olduğu gibi iletebilir

**Ne işe yarar:**

- Request/Response manipülasyonu
- Parametre değiştirme
- Header ve cookie analizi
- Yetkilendirme ve input validation testleri

Intercept açıkken istek akışı manuel kontrol altındadır.

## 4.2. Target

Hedef uygulamanın tüm yüzeyini analiz etmek için kullanılır. Proxy üzerinden geçen tüm endpoint’ler otomatik olarak haritalanır.

**Teknik İşlevi:**

- Proxy’den geçen tüm istekleri otomatik olarak sınıflandırır
- URL, parametre, HTTP metod ve response’ları gruplayarak gösterir

**Ne işe yarar:**

- Site Map çıkarma
- Endpoint, parametre ve metod analizi
- Test kapsamının (scope) belirlenmesi

## 4.3. Repeater

- Tek bir HTTP isteğini defalarca gönderip, küçük değişikliklerle response farklarını gözlemlemenizi sağlar.

**Teknik İşlevi:**

- Request’i birebir kopyalar
- Parametre, header veya body üzerinde değişiklik yapmanıza izin verir
- Response’ları anlık olarak gösterir

**Ne işe yarar:**

- SQL Injection doğrulama
- IDOR testleri
- Input validation kontrolü
- Hata mesajı ve status code analizi

Manuel zafiyet doğrulamanın en güçlü aracıdır.

## 4.4. Intruder

- Parametre bazlı otomatik saldırılar için kullanılır. Fuzzing ve brute force senaryolarının merkezidir.

**Teknik İşlevi:**

- Request içindeki parametreleri işaretlersiniz
- Payload listesi tanımlarsınız
- Burp bu payload’ları sırayla dener

**Ne işe yarar:**

- Login brute force
- ID tahmini
- Parametre fuzzing
- Rate limit testleri

Community sürümünde hız kısıtlıdır, ancak mantık aynıdır.

## 4.5. Decoder

- Veri dönüştürme ve çözümleme aracıdır.

**Teknik İşlevi:**

- URL encoding
- Base64 decode
- Hex → ASCII dönüşümü

**Ne işe yarar:**

- Base64, URL encode/decode
- Hash formatlarını analiz etme
- Gizlenmiş parametreleri anlama

Özellikle token ve cookie analizlerinde kullanılır.

## 4.6. Comparer

İki veya daha fazla request/response arasındaki farkları byte seviyesinde karşılaştırır.

**Teknik İşlevi:**

- Response body farklarını gösterir
- Header veya status code değişimlerini tespit eder

**Ne işe yarar:**

- Yetkili / yetkisiz response farklarını görmek
- IDOR ve authorization testleri
- Intruder sonuçlarını analiz etmek

Küçük farkların büyük zafiyetleri ortaya çıkardığı yer burasıdır.

# 5. Kısa Özet

Burp Suite → **gör, değiştir, tekrar gönder, karşılaştır** prensibiyle çalışır.

Proxy + Repeater → manuel pentest’in omurgasıdır.

1. **Proxy** → Trafiği yakala
2. **Target** → Endpoint’i analiz et
3. **Repeater** → Manuel test yap
4. **Intruder** → Otomatik deneme
5. **Comparer** → Sonuçları kıyasla
6. **Decoder** → Token / veri çöz


# 6. Önemli Kaynaklar

- https://www.beyaz.net/tr/guvenlik/makaleler/burp_suite_nedir.html
- https://tryhackme.com/room/burpsuitebasics
    - **TryHackMe:** Ücretli ve ücretsiz rehberlerin olduğu interaktif platform TryHackMe’nin ücretli Burp Suite Başlangıç Eğitimi
- https://www.kalilinuxlogs.com/2025/07/burp-suite-web-proxy.html
- https://www.geeksforgeeks.org/ethical-hacking/what-is-burp-suite/
- https://www.bugcrowd.com/blog/the-ultimate-beginners-guide-to-burp-suite/