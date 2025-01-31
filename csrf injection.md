CSRF (Cross-Site Request Forgery) Zafiyeti Nedir?

CSRF (Cross-Site Request Forgery), bir saldırganın kurbanı, istemeden bir işlemi gerçekleştirmeye zorladığı bir web güvenlik açığıdır. Bu zafiyet, kullanıcının tarayıcısında oturum açtığı ve kimlik doğrulamasının geçerli olduğu durumlarda, kötü niyetli bir isteğin kurban adına yapılmasına olanak tanır. Örneğin, bir bankacılık uygulamasında, bir saldırgan kurbanın bilgisi dışında bir para transferi gerçekleştirebilir.

CSRF Zafiyeti Nasıl Çalışır?
CSRF, genelikle şu adımları içerir:

Kurbanın oturumunun aktif olması: Kurban, bir web uygulamasında (örneğin bir bankacılık sitesinde) oturum açmış olmalıdır.
Kötü niyetli bir isteğin hazırlanması: Saldırgan, kurban adına istek gönderen bir form veya bağlantı oluşturur.
Kurbanın tuzağa düşürülmesi: Kurban, saldırgan tarafından hazırlanmış bir bağlantıya tıkladığında veya bir formu doldurduğunda, istek hedef web uygulamasına gönderilir.
Yetkilendirilmiş işlemin gerçekleştirilmesi: Hedef uygulama, isteğin kurbandan geldiğini düşünerek işlemi gerçekleştirir.

CSRF Saldırı Türleri
1. GET Tabanlı CSRF
GET tabanlı CSRF saldırıları, bir URL’nin kötüye kullanılması yoluyla gerçekleştirilir. Örneğin:

Zararlı URL:

https://bank.example.com/transfer?account=12345&amount=1000
Kurban, bu URL’ye tıkladığında, oturum çerezleri otomatik olarak eklenir ve transfer işlemi gerçekleşir.

Saldırı Senaryosu:

Saldırgan, bir sosyal medya platformunda, “Hediye Çeki Kazanın!” başlıklı sahte bir bağlantı paylaşır. Bu bağlantıya tıklayan kurban, farkında olmadan saldırganın bankacılık uygulamasındaki hesabına para transfer eder. Kurban, oturumunun açık olması nedeniyle işlem otomatik gerçekleşir.

SENARYO 2 ;

bir şifre değiştirme form u görmektesiniz burada yeni şifremizi yazıyor ve isteği burp ile yakalıyoruz

istekte get metodundan sonraki kısmı kopyalayıp düzenliyoruz


düzenledikten sonra isteğimizi kurbana mail yada wp gibi platformdan url yi atıyoruz isterseniz burada url yi urlshorter vb platformlardan kısaltabilirsiniz attığımız kişi tıkladığı anda şifresi bizim düzenlemiş olduğumuz bilgiyle aynı oluyor
2. POST Tabanlı CSRF
POST tabanlı CSRF saldırıları, bir formun kurban adına gönderilmesiyle gerçekleştirilir.

Zararlı Kod Örneği:

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
Saldırı Senaryosu:

Saldırgan, bir e-posta göndererek kurbanı sahte bir “Hesap Güvenliği Güncellemesi” sayfasına yönlendirir. Bu sayfa, arka planda yukarıdaki zararlı formu çalıştırır ve kurban adına saldırganın hesabına para transferi yapar.

bu kodun yanı sıra kendi sayfanıza img vb tag kodlarını kullanarak ta aynı işlemi yapabilirsiniz yeter ki kullanıcı o siteye gitsin :)

aşağıda ki kod da img tag i kullanılarak source kısmına yönlendirme verilmiş kurbanımız bu kodun olduğu sayfaya girdiğinde herhangi bir resim göremeyecek ama arkaplanda kod çalıştığı için e-mail değişecektir

<img src="https://csrfinj.com/email/change?email=pwned@evil-user.net">
3. CORS İle Kombinasyon
Eğer bir web uygulaması CORS (Cross-Origin Resource Sharing) yapılandırmasını yanlış yapılandırdıysa, CSRF saldırıları daha tehlikeli hale gelebilir. Bu durumda, saldırgan farklı bir etki alanından istek göndererek işlem gerçekleştirebilir.

Saldırı Senaryosu:

Saldırgan, “trustedbank.example.com” yerine “malicious.example.com” alan adını kullanır. CORS yapılandırmasındaki eksiklikler nedeniyle, saldırganın alanından gönderilen istekler güvenilir kabul edilir ve kurbanın hesabında değişiklik yapılabilir.

Korunma Yöntemleri
1. CSRF Token Kullanımı
CSRF token, her işlem için sunucu tarafından oluşturulan ve doğrulanan benzersiz bir değerdir. Token, form verilerine veya URL’ye eklenir.

Örnek CSRF Token:

<input type="hidden" name="csrf_token" value="a1b2c3d4">
Sunucu Tarafında Doğrulama:

if request.form['csrf_token'] != session['csrf_token']:
    abort(403)  # Yetkisiz işlem
2. Kullanıcı Doğrulaması
Kritik işlemler öncesinde kullanıcıdan şifre veya başka bir doğrulama isteyerek CSRF saldırılarının etkisi azaltılabilir.

Örnek:

Para transferi öncesinde iki faktörlü kimlik doğrulama (OTP) kullanımı.
3. HTTP-Only ve Secure Çerezler
Çerezlerin JavaScript tarafından okunabilirliğini ve yalnızca HTTPS bağlantıları üzerinden gönderilebilir olmasını sağlayarak, oturum çerezleri daha güvenli hale getirilebilir.

Örnek:

Set-Cookie: session=abc123; HttpOnly; Secure
4. Referer Header Kontrolü
İsteklerin kaynağını kontrol ederek, yetkisiz alanlardan gelen istekleri engelleyebilirsiniz.

Örnek:

if 'Referer' not in request.headers or not request.headers['Referer'].startswith('https://trusted.example.com'):
    abort(403)