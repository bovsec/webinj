IDOR (Insecure Direct Object References) Zafiyeti Nedir?

DOR (Insecure Direct Object References), bir yetkilendirme kontrolü eksikliği nedeniyle bir kullanıcının başka bir kullanıcının verilerine veya kaynaklarına erişmesine olanak tanıyan bir güvenlik zafiyetidir. Bu açık, genellikle uygulamanın doğrudan bir nesneye referans veren zayıf bir şekilde yapılandırılmış URL’ler veya parametreler kullanması nedeniyle ortaya çıkar.


IDOR Zafiyeti Nasıl Çalışır?
IDOR zafiyeti, bir kullanıcının kendi erişim hakkına sahip olmadığı verileri, yalnızca bir parametreyi değiştirerek görebilmesine dayanır. Bu genellikle şu yollarla gerçekleşir:

URL parametrelerinde doğrudan nesne referansları (ör. kullanıcı ID’si veya dosya adı)
Gizli verilerin doğrudan açıklanması
Sunucuda eksik veya yanlış uygulanmış yetkilendirme kontrolleri
Basit Bir Örnek:
Bir web uygulaması, kullanıcının kendi profil bilgilerine erişmesini sağlayan bir URL’ye sahip olabilir:

https://example.com/profile?id=123
Eğer bir saldırgan bu URL’deki id parametresini 124 gibi başka bir değere değiştirerek başka bir kullanıcının profil bilgilerine erişebiliyorsa, bu bir IDOR zafiyetidir.

IDOR Zafiyeti Türleri
1. URL Tabanlı IDOR
Bu, en yaygın IDOR türüdür ve doğrudan URL’de yer alan parametrelerin manipülasyonu ile gerçekleşir.

Örnek:

https://example.com/orders?order_id=1001
Saldırgan, order_id değerini 1002 yaparak başka bir kullanıcının sipariş bilgilerini görebilir:

https://example.com/orders?order_id=1002
2. POST Verileri Üzerinden IDOR
POST istekleri ile gönderilen veriler manipüle edilerek başka nesnelere erişim sağlanabilir.

Örnek: Bir dosya indirme API’si şu POST verilerini kullanabilir:

{
  "file_id": "example_file.pdf"
}
Saldırgan, file_id değerini değiştirerek farklı bir dosyayı indirebilir:

{
  "file_id": "confidential_file.pdf"
}
3. JSON Web Token (JWT) veya Cookie Manipülasyonu
JWT veya çerezlerde kullanıcıya ait kimlik bilgilerinin değiştirilmesi yoluyla yetkisiz erişim sağlanabilir.

Örnek:

{
  "user_id": "123"
}
Saldırgan bu değeri 124 olarak değiştirerek başka bir kullanıcının verilerine erişebilir.

IDOR Saldırısı Senaryosu
Senaryo 1: Kullanıcı Profili Erişimi
Bir sosyal medya platformunda, kullanıcıların kendi profillerine erişmek için şu URL’yi kullandığını varsayalım:

https://socialmedia.com/profile?user_id=101
Saldırgan, bu parametreyi değiştirdiğinde başka bir kullanıcının profil bilgilerine erişebilir:

https://socialmedia.com/profile?user_id=102
Zararlı Kod Örneği:

# Burp Suite Repeater ile test
GET /profile?user_id=102 HTTP/1.1
Host: socialmedia.com
Cookie: session=abc123
Eğer uygulama yeterli yetkilendirme kontrolü uygulamıyorsa, saldırgan bu istekle başka bir kullanıcının bilgilerini görebilir.

Senaryo 2: Dosya İndirme
Bir belge yönetim sistemi, dosya indirme istekleri için şu URL’yi kullanıyor olabilir:

https://docs.example.com/download?file_id=report2025.pdf
Eğer saldırgan, file_id değerini değiştirirse, yetkisiz dosyalara erişebilir:

https://docs.example.com/download?file_id=confidential2025.pdf
Zararlı Kod Örneği:

GET /download?file_id=confidential2025.pdf HTTP/1.1
Host: docs.example.com
Cookie: session=xyz456
Eğer sunucu bu isteği doğrulamadan dosyayı dönerse, hassas bilgiler sızdırılabilir.

SENARYO 3:
bu bir tryhackme ctfidir ve tamamen YASALDIR


hesabımıza giriş yaptık ve id değerimizin 11 olduğunu gördük şimdi bunu değiştiriyoruz .

id değerimizi 1 yaptık ve president yolunde hesaba giriş yaptık idor açığı çalışmış oldu bir daha değiştiriyoruz değerimizi

database admini rolüne girdik ve belli başlı şeyleri silme düzenleme yetkimiz olduğunu görüyoruz
Korunma Yöntemleri
IDOR zafiyetlerini önlemek için aşağıdaki güvenlik önlemleri alınabilir:

1. Yetkilendirme Kontrolleri
Tüm erişimlerin, kullanıcı yetkilendirmesi doğrulanarak yapılmasını sağlayın.
Kullanıcıların yalnızca kendi verilerine eriştiğinden emin olun.
2. Dolaylı Referanslar Kullanın
Doğrudan ID yerine, benzersiz ve rastgele oluşturulmuş tokenlar kullanın.
Örnek:

Doğrudan: https://example.com/orders?order_id=1001
Güvenli: https://example.com/orders?token=abc123xyz
3. Parametre Doğrulama
Sunucu tarafında gelen parametrelerin doğruluğunu kontrol edin.
Örnek: Kullanıcının bir order_id'ye erişim hakkı olup olmadığını kontrol edin.
4. Gelişmiş Güvenlik Testleri
IDOR zafiyetlerini tespit etmek için düzenli olarak güvenlik testleri (ör. penetration test) gerçekleştirin.
Burp Suite, OWASP ZAP gibi araçları kullanarak manuel testler yapın.