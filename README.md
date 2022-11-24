<h1>Tüm cihazlarda YouTube videoları otomatik başlatma</h1>

YouTube videolarını sayfaya gömerek paylaşma sırasında, videonun otomatik olarak oynamaya başlaması işlemi:

Videolar şüphesiz ki kullanıcılara vermek istediğimiz bilgileri aktarmak için kullandığımız en çarpıcı, etkileyici araçlardır. Zaman zaman isteğimiz veya gelen talepler üzerine sayfaya eklediğimiz bu videoları sayfa yüklendiği anda otomatik başlatabiliriz.

YoutTube videolarında otomatik başlatma için sitenin bize verdiği autoplay=1 parametresini iframe src adresinin arkasına ekleyebiliyoruz. Örnek olarak: 
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/Mq0yEI_xpb8?autoplay=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```

Peki bu parametre sorunumuzu tam olarak çözmek için yeterli mi?

Yanıt: Hayır.

Masaüstü ve laptop bilgisayarlarda video otomatik olarak oynamaya başlıyor fakat mobil cihazlarda başlamadığını görüyoruz. Bunun iki nedeni var:

1 - Mobil cihazların en kişisel eşyalarımızdan biri olması.
2 - Kullanıcı deneyimi.

Bu iki nedeni birbirinden ayrı değil aynı anda çalışan mekanizmalar olarak düşünebiliriz. Şöyle ki:

<h2>1 - Mobil cihazların en kişisel eşyalarımızdan biri olması:</h2>
Toplu taşımalarda, toplantılarda, kısacası yalnız olmadığımız, topluluk içinde bulunduğumuz anlarda içinde otomatik oynatma komutuyla video bulunan bir sayfayı açtığımızda telefonumuzdan gelecek olan video sesi bütün dikkat üzerimize çekeceği gibi çevrede rahatsızlık oluşturabilir. Ben şahsen telefonumdan benim bilgim ve iradem dışında bir ses çıkmasını tercih etmem.

<h2>2 - Kullanıcı deneyimi:</h2> Kullanıcı telefonuyla otomatik oynayan video içeren bir sayfa açtığında, kısıtlı olan bağlantı video dosyasını indirmeye çalıştığı için hız sorunu yaşacaktır. Video, karşılaşılan ilk ekranda görünmüyor olabilir yani fold dışında kalmıştır. Bu durumda kullanıcı sayfayı açtığı anda nereden geldiğini anlamadığı bir ses, müzik, gürültü vb duyacaktır. Bir de kulaklık takılıysa, sesin yüksekliğine göre ani bir irkilme yaşaması da söz konusu.

Bu ani ses olayını engellemek, video sesini başlangıçta tamamen kapatmakla mümkün. Bunu sağlamak için iframe src adresine mute=1 parametresini ekliyoruz. Örnek:
```
<iframe width="560" height="315" src="https://www.youtube.com/embed/Mq0yEI_xpb8?mute=1" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
```
Şimdi videoyu hem otomatik başlatan, hem de sesini kapatarak kimseyi rahatsız etmeyen bir sayfamız oldu.

Peki sorunumuz tamamen çözüldü mü? Malesef yine hayır.

Mobil cihazların son sürümleri, src içine eklediğimiz bu parametreleri hala engeller durumda. Artık bunu aşmamızın tek yolu kalıyor: YouTube API kullanmak.

YouTube API kullanarak video üzerinde kendi istediğimiz özel kontrol tuşlarını oluşturabildiğimiz gibi, herhangi bir cihazda sayfa yüklendiği anda videonun otomatik başlamasını da sağlayabiliyoruz.

<h2>Sadede gelirsek:</h2>

Öncelikle sayfaya eklediğimiz video için YouTube API kullanacağımızı belirtmeliyiz: enablejsapi=1

Daha sonrasında sesi yine güvenlik altına almanızı öneririm. Kimse yaptığınız sayfadan rahatsız olmasın: mute=1

İsteğe bağlı olarak video üzerinde HTML5 özelliklerini kullanmak için bir parametremiz var: html5=1

Bunları kullandığımızda örneğimiz şöyle oluyor:
```
<iframe id="ytVideo" width="560" height="315"
      src="https://www.youtube.com/embed/Mq0yEI_xpb8?mute=1&enablejsapi=1&html5=1" title="YouTube video player"
      frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
      allowfullscreen></iframe>
```
Burada iframe için bir id="ytVideo" ekledim çünkü şimdi YouTube API kullanarak videonun sayfa yüklendiğinde otomatik başlaması için bir script yazacağız:

Önce video alanı çin bir global oluşturalım:
```
var player;
```
Bundan sonra YT API yüklendiği anda çalışacak fonksiyonumuzu ekliyoruz:
```
/* API yüklendiği anda çalışacak fonksiyon */
function onYouTubePlayerAPIReady() {
	/* Video iframe (id="ytVideo") kullanarak global player oluşturuyoruz  */
      player = new YT.Player('ytVideo', {
        events: {
			/* player hazır olduğunda aşağıdaki fonksiyonu çağırıyoruz */
          'onReady': onPlayerReady
        }
      });
    }
    
    /* videoyu başlatma fonksiyonu */
    function onPlayerReady(event) {
      player.playVideo();
    }
    
    /* YouTube API'yi sayfaya eklememiz de gerekiyor */
    var tag = document.createElement('script');
    tag.src = "https://www.youtube.com/player_api";
    var firstScriptTag = document.getElementsByTagName('script')[0];
    firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
```
    
Bu işlemlerden sonra sayfayı açtığımızda, video hem mobil cihazlarda hem de masaüstü ve laptop bilgisayarlarda sesi kısık olarak otomatik oynamaya başlayacak.

🌧