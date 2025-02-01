<h1>CSRF (Cross-Site Request Forgery) Zafiyeti Nedir?</h1>

<p>CSRF (Cross-Site Request Forgery), bir saldırganın kurbanı, istemeden bir işlemi gerçekleştirmeye zorladığı bir web güvenlik açığıdır. Bu zafiyet, kullanıcının tarayıcısında oturum açtığı ve kimlik doğrulamasının geçerli olduğu durumlarda, kötü niyetli bir isteğin kurban adına yapılmasına olanak tanır. Örneğin, bir bankacılık uygulamasında, bir saldırgan kurbanın bilgisi dışında bir para transferi gerçekleştirebilir.</p>

<h3>CSRF Zafiyeti Nasıl Çalışır?</h3>
CSRF, genelikle şu adımları içerir:
```php
1.Kurbanın oturumunun aktif olması: Kurban, bir web uygulamasında (örneğin bir bankacılık sitesinde) oturum açmış olmalıdır.

2.Kötü niyetli bir isteğin hazırlanması: Saldırgan, kurban adına istek gönderen bir form veya bağlantı oluşturur.

3.Kurbanın tuzağa düşürülmesi: Kurban, saldırgan tarafından hazırlanmış bir bağlantıya tıkladığında veya bir formu doldurduğunda, istek hedef web uygulamasına gönderilir.

4.Yetkilendirilmiş işlemin gerçekleştirilmesi: Hedef uygulama, isteğin kurbandan geldiğini düşünerek işlemi gerçekleştirir.
```
<h2>CSRF Saldırı Türleri</h2>
<h4>1. GET Tabanlı CSRF</h4>
GET tabanlı CSRF saldırıları, bir URL’nin kötüye kullanılması yoluyla gerçekleştirilir. Örneğin:

Zararlı URL:
```php
https://bank.example.com/transfer?account=12345&amount=1000
```
Kurban, bu URL’ye tıkladığında, oturum çerezleri otomatik olarak eklenir ve transfer işlemi gerçekleşir.

Saldırı Senaryosu:

Saldırgan, bir sosyal medya platformunda, “Hediye Çeki Kazanın!” başlıklı sahte bir bağlantı paylaşır. Bu bağlantıya tıklayan kurban, farkında olmadan saldırganın bankacılık uygulamasındaki hesabına para transfer eder. Kurban, oturumunun açık olması nedeniyle işlem otomatik gerçekleşir.


<h2>POST Tabanlı CSRF</h2>
<p>POST tabanlı CSRF saldırıları, bir formun kurban adına gönderilmesiyle gerçekleştirilir.</p>
```php
<html>
  <body>
    <form action="https://bank.example.com/transfer" method="POST">
      <input type="hidden" name="account" value="12345">
      <input type="hidden" name="amount" value="1000">
    </form>
    <script>
      document.forms[0].submit();
    </script>
  </body>
</html>
```
<p>Saldırı Senaryosu:

Saldırgan, bir e-posta göndererek kurbanı sahte bir “Hesap Güvenliği Güncellemesi” sayfasına yönlendirir. Bu sayfa, arka planda yukarıdaki zararlı formu çalıştırır ve kurban adına saldırganın hesabına para transferi yapar.

bu kodun yanı sıra kendi sayfanıza img vb tag kodlarını kullanarak ta aynı işlemi yapabilirsiniz yeter ki kullanıcı o siteye gitsin :)

aşağıda ki kod da img tag i kullanılarak source kısmına yönlendirme verilmiş kurbanımız bu kodun olduğu sayfaya girdiğinde herhangi bir resim göremeyecek ama arkaplanda kod çalıştığı için e-mail değişecektir</p>

```php
<img src="https://csrfinj.com/email/change?email=pwned@evil-user.net">
```

<h2>CORS İle Kombinasyon</h2>
<p>Eğer bir web uygulaması CORS (Cross-Origin Resource Sharing) yapılandırmasını yanlış yapılandırdıysa, CSRF saldırıları daha tehlikeli hale gelebilir. Bu durumda, saldırgan farklı bir etki alanından istek göndererek işlem gerçekleştirebilir.

Saldırı Senaryosu:

Saldırgan, “trustedbank.example.com” yerine “malicious.example.com” alan adını kullanır. CORS yapılandırmasındaki eksiklikler nedeniyle, saldırganın alanından gönderilen istekler güvenilir kabul edilir ve kurbanın hesabında değişiklik yapılabilir.</p>

