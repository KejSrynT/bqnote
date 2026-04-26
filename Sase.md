# SASE (Secure Access Service Edge) Rehberi

## 1. SASE Nedir? (En Basit Tanım)
SASE, **Network (Ağ)** ile **Güvenlik (Security)** servislerinin tek bir bulut platformunda birleşmesidir. 
* **Eski Mimari:** İnternete girmek için ayrı cihaz, güvenlik için ayrı (fiziksel) cihazlar.
* **SASE Mimarisi:** Tüm trafik bulut tabanlı tek bir "akıllı filtre" üzerinden geçer.

---

## 2. Olayı Nedir ve Nasıl Çalışır?
Geleneksel yapılardaki (VPN gibi) yavaşlığı ve güvenlik açıklarını çözmeyi hedefler.

* **Kimliğe Dayalıdır:** "Nereden" bağlandığın değil, "kim" olduğun ve "hangi cihazla" (şirket bilgisayarı mı, kişisel mi?) bağlandığın önemlidir.
* **Bulut Tabanlıdır:** Güvenlik kontrolleri (Antivirüs, Firewall, URL Filtreleme), kullanıcının bilgisayarı ile gideceği site arasındaki en yakın **bulut noktasında (Edge)** yapılır.
* **Sürekli Denetim (Zero Trust):** Bir kez giriş yapmak yetmez. Sistem her adımda "Gerçekten sen misin?" ve "Cihazın hala güvenli mi?" diye kontrol eder.

---

## 3. Geleneksel Yapı (VPN/Firewall) vs. SASE

| Özellik | Geleneksel Yapı (VPN/MPLS) | SASE |
| :--- | :--- | :--- |
| **Merkez** | Şirket binasındaki fiziksel cihaz. | Bulut (Her yer merkez). |
| **Hız** | Trafik merkeze gidip geldiği için yavaş (Hairpinning). | En yakın bulut noktasından çıktığı için hızlı. |
| **Güvenlik** | "İçerideysen güvenlisin" (Kale mantığı). | "Hiç kimseye güvenme" (Zero Trust). |
| **Yönetim** | Her şubeye/ofise ayrı donanım gerekir. | Tek bir panelden tüm dünyadaki kullanıcılar yönetilir. |

---

## 4. Gerçek Hayat Senaryosu: "Kale" vs. "Dijital Pasaport"

### 🏰 Eski Yöntem: Kale Modeli
Şirket merkezi dev bir kaledir. Sen dışarıdayken bir dosyaya bakman gerekirse, kilometrelerce yol gidip kalenin kapısındaki muhafıza (VPN) kendini tanıtman gerekir. İçeri girdiğinde tüm odalarda (serverlar) gezebilirsin. Anahtarın çalınırsa hırsız her yere girebilir.

### 🛂 Yeni Yöntem: SASE (Dijital Pasaport Modeli)
Şirket verileri dünyanın her yerindeki güvenli kasalara (Bulut/SaaS) dağıtılmıştır. Elinde gelişmiş bir **Dijital Pasaport** vardır. 
* Nerede olursan ol, pasaportun senin kimliğini ve cihaz güvenliğini saniyeler içinde doğrular. 
* Seni kaleye kadar yormaz; olduğun yerin en yakınındaki güvenli noktadan veriye ulaştırır. 
* Sadece yetkin olan odayı açar, başka odaya geçmek istersen tekrar kontrol yapar.

---

## 5. Berqnet Mülakatı İçin "Altın Cümle"
> "Günümüzde uygulamalar buluta (SaaS) taşındı ve çalışanlar ofis dışına çıktı. Artık trafiği zorla merkezdeki bir donanıma sokmak verimsizleşti. SASE, güvenliği kullanıcının olduğu yere götürerek hem hızı artırıyor hem de **Zero Trust (Sıfır Güven)** prensibiyle güvenliği modernize ediyor."

---

## 6. SASE'nin 4 Temel Bileşeni
1.  **SD-WAN:** Trafiği akıllı ve verimli yönlendirme.
2.  **SWG (Secure Web Gateway):** Güvenli internet çıkışı ve filtreleme.
3.  **CASB (Cloud Access Security Broker):** Bulut uygulamaları (Drive, Office 365) için güvenlik.
4.  **ZTNA (Zero Trust Network Access):** Uygulama bazlı, "hiç kimseye güvenme" temelli erişim.


# Siber Güvenlik ve Berqnet Teknik Kavramlar Rehberi

## 1. SASE'nin 4 Temel Direği
SASE bir paket program gibidir, içinde şu dört teknoloji barındırır:

* **SD-WAN (Software Defined WAN):** İnternet trafiğini akıllıca yönetir. Eğer şirkette iki internet hattı varsa, kritik trafiği (Zoom, ERP) hızlı hattan, önemsiz trafiği yavaş hattan gönderir.
* **ZTNA (Zero Trust Network Access):** "Asla güvenme, her zaman doğrula." Kullanıcı ağın içinde olsa bile her uygulama için tekrar kimlik doğrulaması yapar.
* **SWG (Secure Web Gateway):** Şirket çalışanlarının internette gezerken zararlı sitelere girmesini engelleyen bulut tabanlı bir filtredir.
* **CASB (Cloud Access Security Broker):** Şirket verilerinin bulut uygulamalarında (Google Drive, Dropbox vb.) nasıl paylaşıldığını denetler. Veri sızıntısını önler.

---




## 2. Hotspot (Güvenli Ortak İnternet)
**Olayı Nedir?** Misafirlerin veya müşterilerin (kafe, otel, hastane) kendi şifreleriyle veya SMS/T.C. Kimlik doğrulamasıyla internete bağlanmasını sağlayan sistemdir.

* **Neden Önemli?** Kayıtsız internet dağıtmak suçtur.
* **Berqnet Notu:** Berqnet, Hotspot tarafında Türkiye'deki mevzuata en uyumlu cihazlardan biridir. SMS doğrulaması ve otel yazılımlarıyla (PMS) tam entegre çalışır.

---

## 3. VPN (Sanal Özel Ağ)
**Olayı Nedir?** İnternet üzerinde "şifreli bir tünel" açarak dışarıdaki bir kullanıcının şirket ağına sanki oradaymış gibi güvenli bağlanmasını sağlar.

* **SSL VPN:** Sadece bir tarayıcı veya küçük bir yazılımla bağlanılır. Kullanıcı dostudur, evden çalışanlar için idealdir.
* **IPsec VPN:** Genelde iki ofisi (Merkez-Şube) birbirine bağlamak için kullanılır. Kalıcı ve donanım tabanlı bir tüneldir.

---

## 4. 5651 Sayılı Kanun ve Loglama
**Olayı Nedir?** Türkiye'de internet toplu kullanım sağlayıcılarının, kimin hangi siteye ne zaman girdiğini kayıt altına almasını zorunlu kılan kanundur.

* **Zaman Damgası:** Tutulan logların sonradan değiştirilmediğini kanıtlamak için log dosyasının "mühürlenmesidir".
* **Berqnet Notu:** Berqnet cihazları bu logları otomatik tutar ve yasal olarak geçerli şekilde imzalar (TÜBİTAK Zaman Damgası vb.).

---

## 5. UTM vs. Firewall
**Olayı Nedir?** * **Firewall:** Sadece gelen giden trafiği (IP/Port bazlı) durduran bir trafik polisidir.
* **UTM (Unified Threat Management):** İçinde Antivirüs, IPS/IDS, Web Filtreleme ve VPN gibi birçok güvenliği barındıran "hepsi bir arada" güvenlik kutusudur.

---

## 6. IPS ve IDS (Saldırı Tespit ve Engelleme)
* **IDS (Detection):** Sadece "Hey, biri kapıyı zorluyor!" diye alarm verir. Müdahale etmez.
* **IPS (Prevention):** Kapıyı zorlayanı görür görmez hem alarm verir hem de kapıyı yüzüne kapatır (Trafiği bloklar).

---

## 7. NAT (Network Address Translation)
**Olayı Nedir?** İç ağdaki özel (Private) IP adreslerini, internete çıkarken tek bir genel (Public) IP adresine dönüştürme işlemidir.
* **Port Yönlendirme (Destination NAT):** Dışarıdan birinin içerideki bir kameraya veya sunucuya erişmesi için "Şu porttan geleni şu iç IP'ye gönder" demektir.






# Berqnet Teknik Mülakat Hazırlık Notları

## 1. SASE Nedir?
**En Kısa Tanım:** SASE (Secure Access Service Edge), ağ yönetimini (SD-WAN) ve güvenliği (Firewall, Zero Trust vb.) tek bir bulut platformunda birleştiren modern bir güvenlik mimarisidir. 
* Güvenliği bir cihaz olmaktan çıkarıp, kullanıcıya her yerde eşlik eden bir **bulut servisi** haline getirir.

---

## 2. SASE Teknolojisi Nasıl Çalışır?
SASE, kullanıcıyı merkeze alır. Çalışma mantığı şu üç ana prensibe dayanır:
* **Kimlik ve Cihaz Kontrolü:** "Nereden" bağlanıldığına değil, "kimin" (Kullanıcı adı/şifre/MFA) ve "hangi cihazın" (Şirket bilgisayarı mı?) bağlandığına bakar.
* **Bulut Dağıtımı:** Trafik, şirketin ana merkezindeki Firewall'a gitmek yerine, kullanıcıya en yakın bulut noktasına (Edge) gider ve orada taranır.
* **Sürekli Denetim (Zero Trust):** Bağlantı bir kez kurulunca bitmez; kullanıcının her hareketi güvenlik politikalarına göre sürekli denetlenir.

---

## 3. Berqnet SASE’yi Geleneksel VPN’den Farklı Kılan Nedir?

> Tek bir platformda ağ ve güvenliği birleştirir, VPN’lerin karmaşıklığını ortadan kaldırır.


| Özellik | Geleneksel VPN | Berqnet SASE |
| :--- | :--- | :--- |
| **Bağlantı Noktası** | Trafik önce şirket merkezindeki cihaza gider, oradan internete çıkar. | Kullanıcı direkt bulut üzerinden güvenli internete çıkar (Hızlıdır). |
| **Güvenlik Yaklaşımı** | "Kapıdan girene güvenilir." İçeri giren ağda her yere erişebilir. | "Hiç kimseye güvenilmez." Sadece yetkili olduğu uygulamayı görür. |
| **Yönetim** | VPN kurmak ve yönetmek karmaşıktır, donanıma bağlıdır. | Tamamen yazılım tabanlıdır, merkezi panelden kolayca yönetilir. |
| **Performans** | Uzak mesafelerde hız düşer (Gecikme/Latency yaşanır). | Bulut altyapısı sayesinde mesafe fark etmeksizin yüksek performans sağlar. |

---

## 💡 Gerçek Hayat Örneği: "Şirket Ofisi" vs. "Havalimanı VIP Kartı"

### Geleneksel VPN (Şirket Ofisi Modeli)
Eskiden şirketin dosyalarına bakmak için fiziksel ofis binasına (Firewall) gitmek zorundaydın. Evdeysen, VPN ile "sanal bir tünel" üzerinden ofise bağlanırdın. Ancak ofise bir kez girdiğinde, tüm odaların kapısı sana açıktı. Eğer VPN şifreni biri çalarsa, şirketin tüm odalarına (verilerine) girebilirdi.

### Berqnet SASE (Havalimanı VIP Kartı Modeli)
Şirketin verilerinin artık ofis binasında değil, dünyanın her yerindeki güvenli kasalarda olduğunu düşün. Senin elinde ise akıllı bir **VIP Kartı (SASE)** var. 
* Bu kartla dünyanın neresinde olursan ol, sana en yakın güvenlik noktasına gidersin. 
* Kartın senin kimliğini, parmak izini ve üzerindeki kıyafetlerin temizliğini (cihaz güvenliği) anında kontrol eder. 
* Sana tüm binanın anahtarını vermez; kartın sadece **"C odasındaki X dosyasını"** görmene izin verir. Başka bir dosyaya bakmak istersen, kartın yetkini anında tekrar sorgular.




#### Firewall as a Service (FWaaS) Nedir?

Firewall as a Service (FWaaS), bulut üzerinden sunulan güvenlik duvarı hizmetidir.