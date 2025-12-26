# 1. Web Güvenliğine Giriş

---

## 1.1. Web Uygulamalarının Mimarisi

Web güvenliğini anlamak için önce web uygulamasının nasıl çalıştığını doğru kavramak gerekir.

### Temel aktörler

- **İstemci (Client):** Genellikle tarayıcı. Kullanıcıdan aldığı girdiyi HTTP üzerinden sunucuya iletir.
- **Sunucu (Server):** İş mantığını yürütür; veri tabanıyla görüşür, doğrulama yapar, yanıt üretir.
- **Veri Tabanı (Database):** Uygulamanın dinamik içeriklerini ve kullanıcı verilerini saklar.

### Akış

1. Kullanıcı tarayıcıdan bir istek gönderir (HTTP request).
2. Sunucu isteği alır, işler, gerekirse veri tabanı ile iletişim kurar.
3. Üretilen HTML/JSON yanıt tarayıcıya gönderilir (HTTP response).
4. Tarayıcı sonucu işler ve kullanıcıya gösterir.

Bu akışın her aşaması saldırı yüzeyi oluşturur.

---

## 1.2. HTTP Protokolü ve Temel Kavramlar

Web uygulaması güvenliği, HTTP’nin çalışma mantığını bilmeden anlaşılamaz.

### HTTP Request Bileşenleri

- **Method:** GET, POST, PUT, DELETE vb.
- **Headers:** Cookie, User-Agent, Authorization, Content-Type vb.
- **Body:** Form verileri, JSON payload vb.

### HTTP Response Bileşenleri

- Status code (200, 302, 404, 500)
- Headers (Set-Cookie, Content-Type)
- Body (HTML, JSON, dosya içerikleri)

### GET vs POST

- **GET:** Parametreler URL üzerinde görünür. Genellikle sorgulamalar için.
- **POST:** Veri gövde içinde iletilir. Genellikle hassas veri gönderimi için.

### Stateless Mantığı

HTTP *durumsuz* bir protokoldür; her istek bağımsızdır. Oturum yönetimi bu nedenle **cookie**, **session**, **token** gibi mekanizmalarla sağlanır.

---

## 1.3. Cookie, Session, Token Temelleri

### Cookie

Tarayıcıda saklanan küçük veri parçaları.

Genellikle oturum kimliği taşır (session ID).

Önemli flag'ler:

- **HttpOnly:** JavaScript erişemez → XSS riskini azaltır.
- **Secure:** Sadece HTTPS üzerinden gönderilir.
- **SameSite:** CSRF riskini azaltır.

### Session

Sunucu tarafında saklanan kullanıcıya ait durum bilgisidir. Tarayıcıdaki cookie genelde session ID taşır.

### Token

Genellikle API güvenliği ve modern uygulamalarda kullanılır (JWT gibi).

Kullanıcı doğrulandıktan sonra sunucu tarafından üretilen ve kullanıcıyı temsil eden kriptografik veridir.

---

## 1.4. Tarayıcı Güvenlik Mekanizmalarına Giriş

- **Developer Tools:** Request/response inceleme, DOM manipülasyonu, cookie analizi
- **Same-Origin Policy:** Siteler arası erişim kısıtlaması
- **CORS mekanizması:** API erişiminin sınırlandırılması

Tarayıcı mekanizmalarını bilmek, XSS, CSRF ve benzeri saldırıları anlamayı kolaylaştırır.

---

## 2. Web Zafiyetlerinin Sınıflandırılması (OWASP Top 10 Çerçevesi)

---

Bu bölümde zafiyet türlerini kavramsal olarak ele alıyoruz. Uygulamalı örnekler sonraki haftalarda veya laboratuvar çalışmalarında yapılabilir.

---

**OWASP (Open Web Application Security Project)**; küresel, açık kaynaklı ve kar amacı gütmeyen bir organizasyondur. Amacı, yazılım güvenliğini geliştirmek ve web uygulamalarındaki riskleri herkes için anlaşılır ve ölçülebilir hale getirmektir.

OWASP’ın en bilinen çıktısı **OWASP Top 10** listesidir.

## OWASP Top 10’un Tanımı

**OWASP Top 10**, dünya çapında gerçek saldırı verileri, güvenlik araştırmaları ve kurumsal geri bildirimler analiz edilerek hazırlanan, web uygulamalarında en kritik güvenlik risklerinin sıralandığı bir rehberdir.

Bu liste, geliştiriciler, güvenlik uzmanları, pentester’lar ve kurumlar için **de facto standart** haline gelmiştir.

Başlıca amaçları:

1. Web uygulamalarındaki en yaygın ve en tehlikeli zafiyetlere dikkat çekmek
2. Geliştiricilere güvenli kod geliştirme alışkanlıkları kazandırmak
3. Kurumlara temel bir güvenlik kontrol listesi sunmak
4. Güvenlik testlerinin (pentest, code review) önceliklendirilmesini sağlamak

OWASP Top 10 bir standart değildir; ancak güvenlik denetimleri, geliştirme süreçleri ve pentest raporları çoğunlukla bu listeyi referans alır.

## Neden Önemlidir?

- Tüm sektör tarafından kabul görmüş evrensel bir çerçeve sunar.
- Eğitimlerde ve sertifika programlarında temel referans kaynağıdır.
- Uygulamalı güvenlik testlerinde odak noktalarını belirler.
- Geliştiricilerin hangi güvenlik risklerine öncelik vereceğini açık biçimde gösterir.
- Web güvenlik projelerinde minimum gereklilik seti gibi kullanılır.

Bu nedenle web güvenliği öğrenirken OWASP Top 10’u anlamak, güvenlik yaklaşımının temelini sağlar.

Ardından gelecek “Web Zafiyetlerinin Sınıflandırılması” bölümü tam olarak bu listenin güncel sürümünü esas alır.

## 2.1. Broken Access Control (Erişim Kontrolünün Kırılması)

**Tanım:** Kullanıcıların yetkisi olmayan işlemleri yapabilmesi.

**Örnekler:**

- Yetkisiz bir kullanıcının admin paneline erişebilmesi
- IDOR (Insecure Direct Object Reference):

    `/user?id=1001` parametresini değiştirip başka kullanıcıya erişmek

**Etki:** Yetkisiz veri erişimi, veri sızıntısı, hesap devralma.

---

## 2.2. Cryptographic Failures (Kriptografik Hatalar)

**Tanım:** Hatalı şifreleme uygulamaları, hassas verilerin korunmaması.

**Örnekler:**

- Verilerin düz metin olarak tutulması
- Zayıf şifreleme algoritmaları (MD5, SHA1)
- Hatalı TLS yapılandırması

**Etki:** Şifre sızıntısı, MITM saldırıları, veri bütünlüğünün bozulması.

---

## 2.3. Injection (Enjeksiyon Saldırıları)

**Tanım:** Kullanıcı girdisinin güvenli işlenmemesi sonucu sorgulara kötü amaçlı kod sokulması.

### Yaygın türler

- **SQL Injection**
- **Command Injection**
- **LDAP Injection**
- **NoSQL Injection**

**Etki:** Veri ihlali, sistem kontrolünün ele geçirilmesi.

---

## 2.4. Insecure Design (Güvensiz Tasarım)

**Tanım:** Daha kod yazılmadan tasarım seviyesinde yapılan hata veya eksiklikler.

**Örnekler:**

- Rate-limiting olmaması
- Yetersiz doğrulama akışları
- Güvenlik kontrollerinin mimaride öngörülmemesi

---

## 2.5. Security Misconfiguration (Güvenlik Yanlış Yapılandırmaları)

**Tanım:**Uygulama, sunucu veya bileşenlerin yanlış, eksik, default ayarlarla çalışması.

**Örnekler:**

- Varsayılan şifrelerin değiştirilmemesi
- Gereksiz servislerin açık olması
- Hatalı erişim izinleri

---

## 2.6. Vulnerable and Outdated Components (Zafiyetli/Eski Bileşenler)

**Tanım:**Güncellenmemiş kütüphane ve sistemlerin kullanılması.

**Örnekler:**

- Eski jQuery sürümü
- Zafiyetli framework versiyonları
- Patch uygulanmamış sunucular

---

## 2.7. Identification & Authentication Failures (Kimlik Doğrulama Hataları)

**Örnekler:**

- Zayıf parola politikaları
- MFA kullanılmaması
- Parolaların tahmin edilebilir olması
- Oturum süresinin sınırsız olması

---

## 2.8. Software & Data Integrity Failures (Yazılım/Veri Bütünlüğü Hataları)

**Örnekler:**

- İmzalanmamış güncellemeler
- Manipüle edilebilen CI/CD pipeline
- Güvenilmeyen kaynaklardan paket indirme

---

## 2.9. Security Logging & Monitoring Failures (Kayıt ve İzleme Hataları)

**Örnekler:**

- Güvenlik olaylarının kaydedilmemesi
- Logların merkezi izlenmemesi
- Uyarı mekanizmalarının olmaması

**Etki:** Saldırının tespiti gecikir; hasar büyür.

---

## 2.10. Server-Side Request Forgery (SSRF)

**Tanım:**Sunucunun saldırgan tarafından yönlendirilen bir isteği iç ağdaki veya dıştaki başka bir kaynağa göndermesi.

**Örnek:**

Bir URL parametresi üzerinden sunucunun iç IP adreslerini taraması.
