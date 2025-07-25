# Node.js Kursu / freeCodeCamp

## **Node.js Nedir?**

Node.js, JavaScript’i tarayıcı dışında çalıştıran bir ortamdır. 2009’da, Chorme v8 motoru üzerine inşa edildi. Node.js büyük bir topluluğa sahiptir. Node.js sayesinde sadece JS kullanılarak full-stack uygulamalar geliştirilebilir.

---
## Tarayıcı JS’i ile Node.js Arasındaki Farklar Nelerdir?

| DOM | No DOM | DOM ile ilgili JS özellikleri Node.js’de bulunmaz. Çünkü Node.js tarayıcı ortamında çalışmadığından bir DOM’a sahip değildir. |
| --- | --- | --- |
| Window | No window | DOM’a benzer şekilde Node.js Window objesine de sahip değildir. |
|  No filesystem | Filesystem | Tarayıcı JS’inin aksine Node.js dosya sistemine erişebilir ve işletim sistemi ile temas kurabilir. |
| Fragmentation | Versions | Tarayıcı uygulamalarında kullanıcı tarayıcısının uygulamadaki uyumluluğunu test etmek geliştiricinin sorumluluğudur. Fakat Node.js’de böyle bir durum yoktur, çünkü Node.js versiyon sistemi ile çalışır ve bir versiyonda geliştirme tamamlandığında uygulamanın versiyon sebepli bozulma ihtimali yoktur. |
| ES6 modules | Common.js | Tarayıcıda modüller opsiyoneldir. Node.js’de ise modüller yüklü gelir. |

---
## Node.js ile Çalışma Metodları

Node.js ile çalışmanın iki yolu vardır. Biri REPL, diğeri CLI. REPL komut satırında basit bir Node.js ortamı oluşturarak girilen basit Node.js komutlarını çalıştırır, sadece çok temel ve test amaçlı kullanım içindir. Node.js CLI’i ise bir Node komut sistemidir ve terminal üzerinden girilen komutlarla program yönetimi yapar. 

Örneğin terminalde ‘node’ komutu çalıştırılırsa REPL açılmış olur ancak ‘node myScript.js’ komutu çalıştırılırsa CLI ile bir dosya çalıştırılmış olur.

REPL, tarayıcı konsoluna oldukça benzeridir. Tarayıcı konsolunda ciddi bir program geliştirilmez, sadece çok temel ve genellikle test amaçlı işlemler yapılır. 

---
## Globals

Node.js’de window objesi olmamasına rağmen programın her yerinde erişilebilecek globaller vardır. Bunlardan bazıları şu şekildedir:

| __dirname | Mevcut klasörün yolu |
| --- | --- |
| __filename | Mevcut dosya adı |
| require | Modülleri kullanmak için gerekli fonksiyon |
| module | Mevcut modül (dosya) hakkında bilgi |
| process | Programın çalıştırıldığı ortam hakkında bilgil verir. Program gerçek bir uygulamada kullanılmak üzere bir sunucuya deploy edildiğinde o sunucunun veya servis sağlayıcısının programı çalıştırdığı ortam ile iligili bilgi verir. |

---
## Modüller

Node.js CLI kullanırken bir dosya çalıştırılıyor ve bu şekilde program aktif oluyor. Peki bu, bütün yazılımı sadece bir dosya içine mi yazacağımız anlamına geliyor? Tabii ki hayır, Node.js bir dosyayı çalıştırıyor olduğundan bütün yazılım bir dosyada toplanmalı ancak bu bütün yazılımın o dosyaya yazılacağı anlamına da gelmez. 

Modüller sayesinde diğer dosyalara kod yazarak yazılan kodu paylaşabilir ve diğer dosyalarda bu paylaşılan kodları ithal edip kullanabiliriz.

Node.js’de her dosya bir modüldür. Ve her modül ‘encapsulated’dir. Yani veri paylaşımı minimumdur. Bu şekilde geliştirici  export etmedikçe diğer dosyalardan bu modüle erişilemez.

Modüllerin de export ve import yapmanın başlıca yolları şu şekildedir:

```jsx
// dosya 1:
const name = 'Ali'

module.exports = name

// dosya 2: 
const name = require('./dosya1') // Burada dosya yolu yazılırken daima './' syntaxı kullanılır.'
```

Bir başka export yöntemi daha vardır ve bu yöntem  ‘ilerledikçe dışa aktar’ (export as you go) olarak adlandırılır.

```jsx
// dosya 1:
const name = 'Ali'

module.exports.name = name

// dosya 2: 
const name = require('./dosya1') // Burada dosya yolu yazılırken daima './' syntaxı kullanılır.'
```

Bu şekilde birkaç farklı yöntemler olsa da sağlanan fonksiyon aynıdır; bir dosyadaki veriyi başak bir dosyada kullanılabilir yapmak.

### Not

Eğer bir modülde (dosyada) bir fonksiyon tanımlanmış ve dosyada açık şekilde bu fonksiyon çalıştırılmışsa özel bir export olmadan diğer dosyalardan require yapılarak fonksiyon çalıştırılabilir.

Örneğin:

```jsx
// dosya 1: 
function myFunction() {
	console.log(2+2)
}

myFunction()

// dosya 2: 
require('./dosya1')

// Dosya 2'nin çıktısı:
4
```

---
## Built-in Modules

Node.js ile birlikte gelen bazı hazır modüller bulunmaktadır. Bunların önde gelenleri şu şekildedir: OS (Operation system), PATH, FS (Filesystem) ve HTTP. 

Bu modüller import edilirken sonradan oluşturulan modüller gibi ‘./’ syntaxı kullanılmaz. Sadece modül adı yazılır. Ayrıca bu modüller zaten Node.js ile geldiğinden bir şey indirmeye gerek kalmaz.

### OS Modülü

OS modülü işletim sistemi hakkında bilgi sağlar.

Örneğin: 

```jsx
const os = require('os')

// mevcut kullanıcı hakkında bilgi
cosnt user = os.userInfo()
console.log(user)

// sistemin açık kalma süresi hakkında bilgi
const uptime = os.uptime()
console.log(uptime)

// mevcut işletim sistemi hakkında bilgi
const currentOS = {
	name: os.type(),
	release: os.release(),
	totalMem: os.totalmem(),
	freeMem: os.freemem()
}
console.log(currentOS)
```

### PATH Modülü

Path modülü dosya yolları ile ilgili işlemler için geliştirilmiş bir modüldür.

Örneğin:

```jsx
const path = require('path')
console.log(path.sep) // Çıktı: /

const filePath = path.join('content', 'subfolder', 'test.txt')
console.log(filePath) // Çıktı: 'content/subfolder/test.txt'

const base = path.base(filePath)
console.log(base) // Çıktı: 'test.txt'

const absolutePath = path.resolve(__dirname, 'content', 'subfolder', 'test.txt')
console.log(abosultePath) // Çıktı: 
```

### FS Modülü

FS modülü dosya sistemi ile ilgili işlemleri yapmak için kullanılan bir modüldür. Bu modül sayesinde dosya okuma dosya oluşturma ve dosyaya yazma gibi işlemler yapılabilir. 

Bu modülde iki türlü çalışma metodu vardır; senkronize ve asenkron. 

Senkronize olarak dosya okuma (read):

```jsx
const {readFileSync} = require('fs')

const first = readFileSync('./root/folder/text.txt', 'utf8')
const second = readFileSync('./root/folder/text2.txt', 'utf8')

console.log(first + second) // Çıktı: İlk metin dosyasındaki metin içeriği ile ikincisindeki metin içeriğinin birleşimi.
```

Senkronize olarak dosya yazma (write):

```jsx
const {readFileSync, writeFileSync} = require('fs')

writeFileSync('./root/folder/new-text.txt', 'Hello world!')
// Eğer belirtilen konumda bir dosya bulunuyorsa bu dosyaya 'üzerine yazma' (overwrite) işlemi yapılır.
// Ancak belirtilen koumda bir doay yoksa o konumda bir doay oluşturulur ve parametre olarak belirtilen içerik dosya içerisine yazdırılır.

// Bunun yanında write işlemi yapılırken 'flag' sistemi kullanılarak bazı ayarlar da yapılabilir. 
// Örneğin 'a' flagi belirtilen dosya içeriği dolu olsa bile önceki içeriği değiştirmeden doğrudan yeni içeriği eklemeye yarar:
writeFileSync(
	'./root/folder/new-text.txt', 
	'utf8', 
	'Hello world!',
	{ flag: 'a' }
)
```

Senkronize dosya işlemleri ile asenkron dosya işlemleri arasındaki en büyük fark senkronize işlemlerin ‘blok edici’ bir özellikle çalışması. Yani senkronize işlem tamamlanmadan sonraki kodlara geçilmez. Örneğin vakit alan bir senkronize dosya işlemi varsa Node.js işlem tamamlanıncaya kadar o fonksiyonda bekler ve ilerlemez.

Ancak asenkron işlemler bunun tam aksidir. Yani işlemin ne kadar süre aldığı önemli değildir. Çünkü Node.js bu işlemin tamamlanmasını beklemez. Sadece satır satır ilerlerken o fonksiyonu çalıştırır ve sonucu beklemeden sonraki satırlara devam eder.

Bu sebeple gerçek bir uygulamada senkronize bir dosya işlemi yapılırken bir kullanıcı o işlemi yaptığı sırada o kullanıcının işlemi bitene kadar diğer kullanıcı aynı özelliği kullanamaz. 

Bu yüzden kullanım alanına ve duruma göre hangi yöntemin kullanılacağı çok iyi değerlendirilmelidir.

Asenkron dosya okuma:

```jsx
const { readFile } = require('fs')

readFile('./root/folder/text.txt', 'utf8', (err, result) => {
	if (err) {
		console.log(err)
		return
	}
	// Geri kalan işlemler burada yapılarbilir.
	// Ancak burada bir 'callback hell' oluşma durumu vardır. Yani result'a göre çağrılacak fonksiyonlar bu callback içerisinde çağrılacağından bu bir callback hell'e sebep olabilir.
})
```

### HTTP Modülü (Giriş)

HTTP modülü Node.js’in en genelde en çok kullanılan modüldür. Bu modül server oluşturmak, gelen istekleri kontrol etmek ve bu isteklere göre işlemler yapmak veya sonuçlar döndürmek için kullanılır.

Örneğin basit bir HTTP sunucusu şu şekilde oluşturulabilir:

```jsx
const http = require('http')

const server = http.createServer((req, res) => {
	if (req.url === '/') {
		res.end('Welcome to our home page!')
	} else if (req.url === '/about') {
		res.end('Here is our short history...')
	} else {
		res.end('<h1>There is not a page like that</h1>')
	}
})

server.listen(5000)
```

Bu örnekte olduğu gibi HTTPM modülü ile oluştuurlan server, ‘req’ ve ’res’ olarak iki parametreye sahip. Req parametresi ‘request’ anlamına gelip kullanıcıdan gelen isteğin verisini bulunduran çok büyük bir objedir. Res ise ‘response’ anlamına gelip sunucudan kullnıcıya döndürülen yanıtı ifade eder.

---
## NPM

NPM bir paket/modül mağazasıdır ve Node.js indirildiğinde onunla birlikte gelir. Bu mağza geliştiricilerin kendi yazdıkları kodları/modülleri insanlarla paylaşmasına imkân verir.

NPM üç şeyi yapmamızı sağlar: 

1. Kendi kodumuzu başka projelerimizde kullanmamızı.
2. Başkalarının yazdığı kodu kendi projemizde kullanmamızı.
3. Bizim yazdığımız kodu başkalarıyla paylaşmamızı.

En yaygın NPM komutları şu şekildedir:

```jsx
// npm - global komut, Node.js ile birlikte gelir
// npm --version

// yerel gereklilik - paket, sadece mevcut projede kullanılır
// npm i <paketAdı>

// global gereklilik - paket, büütn projelerde kullanılabilir
// npm install -g <paketAdı>

// Bazı durumlarda güvenlik amaçlı 'yönetici olarar' indirmek için kullanılır. Bilgsayardaki aktif oturumun şifresi verilmelidir.
// sudo npm install -g <paketAdı>
```

  

### package.json

package.json dosyası projedeki NPM modülleri hakkında bilgileri tutan ve bu modülleri listeleyen bir dosyadır. Projeye indirilen bütün paketlerin kaydını tutar ve bu paketleri gereklilik (dependency) olarak kaydeder.

package.json dosyasını oluşturmanın üç farklı yöntemi vardır: 

```jsx
// 1. Manuel olarak oluşturmak (tavsiye edilmez)
// 2. npm init (adım-adım sorulara cevap vererek kurulum)
// 3. npm init -y (otomatik kurulum)
```

### NPM Paketlerinin Kullanımı

Aşağıda bir örnek bir NPM paketi kullanımı bulunmaktadır. Önemli bir nokta ise kullanılacak olan modülün daha önceden mutlaka indirilmiş olmasıdır.

```jsx
const _ = request('lodash')

const items = [1, [2, [3, [4]]]]
const newItems = _.flattenDeep(items)

console.log(newItems) // Çıktı: [1, 2, 3, 4]
```

### Kodun Paylaşımı ve package.json’un Önemi

NPM ile indirilen bütün modüllerin dosyaları ‘node_modules’ klasörüne kaydedilir. Bu klasörün ise boyutu genellikle büyük olur ve bu da versiyon kontrol sistemine yükleme esnasında sorun oluşturur. 

Bu sorunun çözümü ise ‘node_modules’ klasörünü ‘.gitignore’ dosyasına ekleyerek GitHub’a yüklememektir. Böylece bu dosya sadece yerel ortamda kalacağından dosyanın büyüklüğü önemli olmayacaktır.

Ancak denebilir ki; ‘Eğer node_modules GitHub’a kaydedilmeyecekse bu repo başkası tarafından klonlandığında kodda kullanılan modüller nasıl çalışacak?’. 

Bu sorunun çözümü package.json dosyasıdır. Repo klonlanıp proje kurulurken package.json dosyasındaki tarife göre gerekli dosyalar indirilerek klonlanan bilgisayarda yeni bir ‘node_modules’ oluşturulacak ve kod çalışır hâle gelecektir. Böylece her committe ciddi bir boyuta sahip olan node_modules’un GitHub’a yüklenme maliyeti olmayacaktır.

Kısacası package.json dosyası projenin çalışır hâle gelmesi için bir ‘kurulum kılavuzudur.

### Dev Dependency (Geliştirme Gerekliliği)

Bazı paketleri indirilirken ‘dev dependency’ olarak indirilebilir. Bunun anlamı, bu paketlerin sadece geliştirme ortamı için olduğu ve uygulamanın çalışması için doğrudan gerekli olmadığıdır.

Örneğin ‘Nodemon’ modülü NOde.js uygulamasında yapılan her değişikliği otomatik olarak algılayarak bizim her defasında ‘node app.js’ benzeri bir komut çalıştırmamıza gerek kalmadan kendisi uygulamayı yeniden çalıştırır.

Ancak bu modül projenin olmazsa olmazı değildir ve yokluğu durumunda projenin çalışmaması söz konusu değildir. Nodemon genelde sadece geliştirme ortamında geliştiricinin işini kolaylaştırmak için kullanıldığından geliştirici gerekliliği ‘dev dependency’ olarak package.json’a kaydedilebilir.

### Paket Silme

Paketleri silmek için ‘npm uninstall <paketAdı>’ komutu kullanılır.

### Global Paket İndirme

Modülleri global olarak indirmek, onları sadece bir projede değil bütün projelerde yerel indirme yapmadan kullanabileceğimiz anlamına gelir. Eğer bir paket indiriyorsak ve o paket sadece bir projede kullanılacaksa hâlen yerel indirme kullanmak daha mantıklıdır.

Bu konuda bir diğer önemli araç da ‘npx’dir. npx sayesinde global olarak bile indirmeden modülleri çağırabiliriz.

### package-lock.json

package-lock.json dosyası pacakge.json dosyasındaki gerekliliklerin versiyon numaralarının ve bu gerekliliklerin de kullandığı diğer paketlerin versiyon numaralarını içeren detaylı bir kayıt dosyasıdır. Bu dosya projenin daha kararlı ve verimli şekilde kurulmasını sağlar.

---
## Olay Döngüsü (Event Loop)

Event loop mantığını anlamak için öncelikle senkronize ve asenkronize çalışma mantığını anlamak gerekir. JavaScript dili ‘tek çekirdekli’ (‘single-threded’) bir dilidir. Yani aynı anda sadece bir işlemi yapabilir. Örneğin zaman alan iki fonksiyon olduğunda ikisini de kendi başına aynı anda işleyemez.

Senkronize çalışan kodlar ‘bloking’ özelliğine sahiptir. Bu kodlar çalıştırılırken zaman alsa dahi sonraki satıra geçmeden önce o kodun tamamlanması gerekir. 

Asenkronize olarak çalıştırılan kodda ise durum tam tersidir. Kod ne kadar zamana alırsa alsın event loop ile arkaplana atılır ve arka planda Libuv API ile çalıştırılırken JS sonraki kodlara devam eder. Ve bu kodu yalnızca işlenemeye hazır hâle geldiğinde çalıştırır.

Örneğin: 

```jsx
console.log(1)

setTimeout(() => {
	console.log(2)
}, 2000)

console.log(3)

// Çıktı: 1, 3, 2
```

Burada çıktının bu şekilde olmasının sebebi setTimeout’un asenkron bir fonksiyon olmasıdır. JS asenkron fonksiyonların tamamlanmasını beklemez ve onları tamamlamak üzere İS’e (işletim sistemi / OS) veya Libuv API’a teslim ederek yalnızca cevap geldiğinde işlerler. İS veya Libuv ise ‘single-threaded’ın aksine çok işleme yeteneği olan çalıştırıcılardır (executors). JS’in async kodları diğer çalıştırıcılara aktarması işlemine ‘offload’ (boşaltma) denir.

Böylece JS, uzun zaman alan kodları çalıştırırken diğer görevlerin ihmâl edilmesi ve uygulamadaki diğer işlevlerin donması durumu yaşanmaz.

![image.png](image.png)

### Async Pattern

Server oluşturulduktan sonra bloklayıcı kod kullanılırsa (blocking code) bu bütün sunucuda, ‘bütün kullanıcılar için’ bir donma oluşturacaktır.

Örneğin:

```jsx
const hhtp = require('http')

const server = http.createServer((req, res) => {
	if (req.url === '/')} {
		res.end('Welcome!')
	}
	if (req.url === '/about')} {
		// ! BLOKLAYICI KOD !
		for (let i = 0; i < 1000; i++) {
			for (let j = 0; j < 1000; j++) {
				console.log(j + i)
			}
		}
		res.end('Welcome!')
	}
})
```

Bu örnekteki gibi senkronize (bloklayıcı) kod kullanıldığında bir kullanıcı ‘hakkında’ sayfasına request attığında diğer kullanıcıların requestlerinde de donma meydana gelir. Yani bir kullanıcını requesti gerçek zamanlı olarak bütün uygulamayı etkileyebilir. 

Bunu engellemek için uzun zaman alan işlemler daima asenkron (async) olarak yapılmalıdır.

### Callback Sorunu

Uzun zaman alan fonksiyonların async olarak yazılması gerektiğinden bahsettik ama bu işlemlerden gelecek sonuca göre yapılacak başka işlemler için iki yol bulunmaktadır; callbackler ve async/await syntaxı.

Callbacklerle çalışmak okunabilirlik ve kodu yazma açısından daha zordur. Örneğin:

```jsx
const { readFile, writeFile }= require('fs')

readFile('path/to/file', 'utf8', (data1, err) => {
	if (err) {
		console.error(err)
	} else {
		const firstData = data1
		readFile('apth/to/second/file', 'utf8', (data2, err) => {
			if (err) {
				console.error(err)
			} else {
			const secondData = data2
				writeFile('path/to/write/file', (data1 + data2), () => {
					if (err) {
						console.log(err)
					} else {
						console.log('Program finished!')	
					}
				})
			}
		})
	}
})
```

Callback yönteminin hem yazması daha zor hem de okunabilirliği az. Bunun için genelde, aynı işi gören ama hem yazması daha kolay hem de okunabilirliği çok daha yüksek olan ‘async-await’ tercih edilir.

### Async/await ve Promises

Async/await kullanımının amacı callbacklerin bir biri ardına bağlandıklarında neden oldukları ciddi okunabilirlik problemini çözmektir. 

```jsx
console.log(1)

function myFunction() {
	console.log(2)
	
	await longWork()
	
	console.log(3)
}

myFunciton()
console.log(4)
```

```jsx
// Çıktı:

1
2
4
// Uzun iş (await) tamamlandıktan sonra
3
```

Kısacası async/await kullanıldığında fonksiyon genel olarak async çalışır ve diğer işlemleri bloke etmez ancak await kullanılan yerde ise fonksiyon kendi içerisinde sync gibi çalışır ve await tamamlanmadan kendi içinde sonraki işleme devam etmez. Ancak yine de bu sadece fonksiyon içi için geçerlidir yani fonksiyon dışındaki işlemler bloke edilmez.

Önemli bir diğer nokta ise await ile kullanılan her işlemin bir ‘promise’ döndürmesi gerektiğidir. Yani promise döndürmeyen işlemlerin await ile kullanılması hükümsüzdür. 

Bu yüzden aşağıdaki örnekte olduğu gibi await daima promise döndüren işlemlerle kullanılmalıdır. Bir önceki callback kullanımının en temiz şekilde yeniden yapılandırılmış (refactor edilmiş) hâli şu şekildedir:

```jsx
const {readFile, writeFile} = require('fs/promises')

async function readAndWriteFiles() {
	try {
		const firstData = await readFile('path/to/file', 'utf8')
		const secondData = await readFile('path/to/second/file', 'utf8')
		
		await writeFile('path/to/write', firstData + secondData);
    console.log('Dosya başarıyla yazıldı.');
	} catch (err) {
		console.error(err)
	}
}
```

Görüldüğü üzere ikinci kullanım birinci callback kullanımından çok daha temiz ve okunabilir.

---
## Olaylar (Events)

Node.js olay tabanlı (event-driven) çalışır. Yani genelde bir olay senaryosu belirlenir, olay gerçekleştiğinde çalıştırılacak kod hazırlanır ve olay dinlemeye alınarak tetiklenme durumunda daha önce belirlenen kod çalıştırılır.

```jsx
const EventEmitter = require('events')

const customEmitter = new EventEmitter()

customEmitter.on('request', () => {
	console.log('Olay tetiklendi!')
})

customEmitter.emit('request')

// Parametreki kullanım:

customEmitter.on('request', (name, age) => {
	console.log('Olay şu kişi tarafından tetiklendi:', name, age)
})

customEmitter.emit('request', 'Ahmet', 30)
```

Daha gerçeğe uygun bir kullanım senaryosu:

```jsx
const http = require('http')

const server = http.createServer()

server.on('request', (req, res) => {
	res.end('Hoşgeldiniz!')
})

server.listen(5000)
```

---
## Streams (Akışlar)

Node.js’de akışlar (streams) veri işlemleri açıısndan son derece önemli bir yere sahiptir. Streamslerin amacı veri okuma, yazma ve değiştirme işlemlerini ‘optimize ve verimli’ şekilde gerçekleştirmektir. 

Küçük veri işlemlerinde streamslere pek ihtiyaç duyulmaz. Streamslerin asıl kullanım yeri büyük verilerdir. Çünkü küçük birkaç bytelık küçük veriler performans sorunlarına yol açmazlar.

Ancak büyük verilerde durum farklıdır. Verilerin okunması veya yazılması işlemler yapılacakken RAM kullanılır ve GBlar boyutundaki verileri bir bütün hâlinde RAM’e göndermek ciddi performans kayıplarına, hatta Node.js programının çökmesine bile sebep olabilir. 

Örneğin 5GBlık bir veriyi ‘readFile’ ile okumaya çalışırsak bu veri ‘tek parça’ hâlinde RAM’e gönderilecektir ve bu da büyük performans kayıplarına sebep olabilir.

Ancak streams ile veri okunduğunda bu 5GBlıık dosya ‘chunk’ denilen varsayılan olarak 64 bytelık veriler hâlinde RAM’e gönderilecek ve ‘parça parça’ işlenerek RAM’ın şişmesinin önüne geçilecektir. 

Örnek:

```jsx
const {createReadStream} = require('fs')

const stream = createReadStream('dosya/yolu/büyük_metin.txt')

stream.on('data', (chunk) => {
	console.log(chunk)
})
```

Yukarıdaki örnekte önce ‘creatReadStream’ ile bir akış oluşturulur ve asenkron olarak başlatılan bu işlemle belirtilen konumdaki dosyanın içeriği chunklar hâlinde RAM’e gönderilir. Buradan her chunk’ın RAM’e aktarımının ardından belirtilen olay dinleyici (event listener) sayesinde chunk için belirtilen işlem yapılır. Bu örnekte console.log işlemi yapılır.

Yani stream dosyayı parça parça RAM’e yollar ve event listener da gelen parçaları teker teker alarak yapılacak işlemi yapar.

Eğer bir işlem belirtilmemişse chunklar RAM’de yine de birikmez ve birkaç chunk sonra silinir. Bu şekilde performans sorunlarının önü kapatılır ve veri işlemleri verimli bir şekilde gerçekleştirilir.

Dört stream tipi vardır:

| Stream Tipi | Açıklama | Örnek Kullanım |
| --- | --- | --- |
| **Readable** | Sadece veri okunabilen stream | `fs.createReadStream()` (dosya okuma) |
| **Writable** | Sadece veri yazılabilen stream | `fs.createWriteStream()` (dosya yazma) |
| **Duplex** | Hem okuyabilen hem yazabilen stream | TCP soketleri, WebSocket bağlantısı |
| **Transform** | Gelen veriyi dönüştüren özel Duplex stream | `zlib.createGzip()` (sıkıştırma), şifreleme |

İşte birkaç streams örneği:

```jsx
const fs = require('fs');

const stream = fs.createReadStream('dosya.txt');

stream.on('data', (chunk) => {
  console.log('Chunk geldi:', chunk.length);
});

stream.on('end', () => {
  console.log('Dosya okuma bitti');
});
```

```jsx
// Büyük bir JSON dosyasını MongoDB'ye parça parça aktarma
const fs = require('fs');
const { MongoClient } = require('mongodb');

const client = new MongoClient('mongodb://localhost:27017');
client.connect().then(() => {
  const db = client.db('test');
  const collection = db.collection('veriler');

  const stream = fs.createReadStream('büyük.json', { encoding: 'utf8' });
  let buffer = '';

  stream.on('data', (chunk) => {
    buffer += chunk;
    let sınır;
    while ((sınır = buffer.indexOf('\n')) >= 0) {
      const satır = buffer.slice(0, sınır);
      buffer = buffer.slice(sınır + 1);
      const json = JSON.parse(satır);
      collection.insertOne(json);
    }
  });
});
```

```jsx
// Log dosyasını okurken, sıkıştırıp yeni bir .gz dosyasına yazma.
const fs = require('fs');
const zlib = require('zlib');

const readStream = fs.createReadStream('log.txt');
const writeStream = fs.createWriteStream('log.txt.gz');
const gzip = zlib.createGzip();

readStream.pipe(gzip).pipe(writeStream);
```

```jsx
// Sunucu, gelen isteğe göre dosyayı veya başka bir kaynağı chunk chunk ağ üzerinden gönderir.
const http = require('http');
const fs = require('fs');

http.createServer((req, res) => {
  const stream = fs.createReadStream('film.mp4');
  res.writeHead(200, { 'Content-Type': 'video/mp4' });
  stream.pipe(res);
}).listen(3000);
```

---
## HTTP Mesajları

Kullanıcı cihazı (client) ile sunucu (server) arasındaki iletişim HTTP mesajları ile sağlanır. Bu mesajlar çeşittir; client tarafından gönderilen ‘talep (request)’ mesajları ve server tarafından gönderilen ‘yanıt (response)’ mesajları. 

### Talep (Request) Mesajları

Request mesajları client’in, talebini servera iletme yoludur. HTTP protokolü üzerinden hedef internet adresine (url)’ye gönderilen bu mesaj server tarafından alınır ve mesajın içeriğine göre gerekli işlemler yapılır. Bu mesajın birkaç bölümden oluşan bir yapısı vardır. Bu bölümler şu şekildedir:

1. **İstek satırı (Request line):**
    
    Request line, mesajın tipini, nereye yönlendirildiğini ve kullanılan HTTP versiyonu olmak üzere temel bilgileri içerir.
    
2. **Başlıklar (Headers):**
    
    Headers mesajın üst verisini (metadata) taşır ve veri türü, yetkilendirme, içerik uzunluğu gibi bilgileri içerir. Genelde otomatik olarak oluşturulur ancak bazı durumlarda manuel olarak da belirlenebilir.
    
3. **İçerik (Body) - Opsiyonel:**
    
    Eğer mesaj, server’a bir veri taşıyacaksa bu veri body kısmında yer alır. Bu kısım opsiyonel olsa da mesaj tipine göre kullanım sıklığı değişebilir. Örneğin GET mesajlarında pek fazla kullanılmazken POST mesajlarında daha sık kullanılır. Bodyde bulunan veriler farklı tiplerde olabilir ancak genelde JSON tipinde veri aktarılır.
    

Request mesajlarında zorunlu olan ‘request line’da mutlaka bulunması gereken ve mesajın talep tipini belirten 4 yaygın metod bulunur; GET, POST, PUT, DELETE.

| GET | Veri almak için kullanılır. |
| --- | --- |
| POST | Veri göndermek için kullanılır. |
| PUT | Mevcut veriyi güncellemek için kullanılır. |
| DELETE | Mevcut veriyi silmek için kullanılır. |

Örnek bir HTTP Request mesajı:

```jsx
// Request line
GET /www.news.com HTTP/1.1

// Headers
Host: example.com
Content-Type: application/json
Authorization: Bearer eyJhbGciOi...

// Body
{
  "username": "user312",
  "age": 41
}
```

### Yanıt (Response) Mesajları

Response mesajları, client tarafından gönderilen request mesajlarına yanıt olarak gönderilen mesajlardır. Örneğin client, tarayıcıda site linkine tıkladığında servera sitenin HTML kodunu almak için bir request gönderilir, server da bu kodu hazırlar ve response mesajı olarak gönderir.

Response mesajları da aynı request mesajlarındaki gibi 3 ana bölümden oluşur:

1. **Durum satırı (Status line):**
    
    Statsus line, HTTP versiyonu, durum kodu ve durum mesajı bilgilerini içerir. Durum kodu talep edilen işlemin tamamlanıp tamamlanmadığı gibi durum bilgisini ifade eden bir koddur ve durum metni de bunun metin açıklamasıdır.
    
2. **Headers:**
    
    Request mesajlardaki gibi metadatayı içerir.
    
3. **Body:**
    
    Request mesajlardaki gibi mesaj içeriğini ifade eder, gönderilecek verinin bulunduğu bölümdür yapılan işleme göre mevcut veya nâmevcut olabilir.
    

Örnek bir response mesajı: 

```jsx
// Status line
HTTP/1.1 200 OK

// Headers
Content-Type: application/json
Content-Length: 54
Connection: keep-alive

// Body
{
  "message": "Merhaba, POST isteğin başarıyla alındı.",
  "success": true
}
```

---
## Basit HTTP Sunucusu Örneği

Aşağıda request ve response objeleri ve çeşitli metodları kullanılarak basit bir HTTP sunucusu oluşturulmuştur.

```jsx
const http = require('http')

const server = http.createServer((req, res) => {
	const url = req.url
	
	// anasayfa
	if (url === '/') {
		res.writeHeader(200, {'content-type' : 'text/html'})
		res.write('<h1>Sitemize hoşgeldiniz!</h1>')
		res.end()
	}
	
	// hakkında sayfaıs
	if (url === '/hakkinda') {
		res.writeHeader(200, {'content-type' : 'text/html'})
		res.write('<h1>Bu bir hakkında sayfasıdır</h1>')
		res.end()
	}
	
	// iletişim sayfası
	if (url === '/iletisim') {
		res.writeHeader(200, {'content-type' : 'text/html'})
		res.write('<h1>Bu bir iletişim sayfasıdır</h1>')
		res.end()
	}
	
	// 404 bildirimi
	else (url === '/') {
		res.writeHeader(404, {'content-type' : 'text/html'})
		res.write('<h1>Aradığınız sayfa bulunamadı</h1>')
		res.end()
	}
})

server.listen('5000', () => {
	console.log('Kullanıcı sunucuya giirş yaptı.')
})
```

Bu örnekte kullanılan başlıca metodlar; writeHeader, write ve end metodlarıdır. HTTP modülü kullanılırken respons’a header ekleme, body ekleme gibi işlemler bu metodlar aracılığıyla yapılırken Express’de çalışırken farklı aynı işlevler farklı şekillerde gerçekleştirilmektedir.

**writeHeader()** metodu client tarafından gelen request mersajına yeni bir response mesajı oluştururken bu mesaja header eklemek için kullanılır. Böylece gönderilen mesajın durmu ve içerik tipi gibi bilgilere kolaylıkla ulaşılabilir.

**write()** metodu ise response mesajına body eklemek için kullanılır. writeHeader ile oluşturulan header’daki içerik tipi (content-type) bu body bölümünün tarayıcıda nasıl okunacağını doğruda etkiler. Örneğin write metodu ile ‘<h1>Merhaba dünya!</h1>’ olarak eklenen içeirk content-type ‘text/plain’ olursa tarayıcı ekranında doğrudan ‘<h1>Merhaba dünya!</h1>’ olarak görünür. Ancak head’daki content type ‘text/html’ olursa tarayıcı ekranında başlık stilinde ‘Merhaba dünya!’ yazısı görünür çünkü tarayıcı içeriğin tipini header’a göre algılar ve buna göre hareket eder. 

**end()** metodu ise daha önceki metodlar ile oluşturulan response objesinin tamamlanması ve tarayıcıya gönderilmesini sağlar.

### Response Olarak Dosya Göndermek

Gerçek uygulamalarda neredeyse her zaman write() metodu ile body alanına bir dosya eklenir. Bu dosya bir .html, .css, .js, .ts, .xml gibi farklı tiplerde olabilir. Bu işlem şu şekilde yapılır:

```jsx
const http = require('http')
const {readFileSync} = require('fs')

const anasayfa = readFileSync('./index.html')

const server = http.createServer((req, res) => {
	const url = req.url
	
	if (url === '/') {
		res.writeHeader(200, {'content-type': 'text/html'})
		res.write(anasayfa)
		res.end()
	}
})
```

Böylece index.html dosyasındaki  bütün içerik header’da belirtilen içerik tipine göre tarayıcı tarafından okunacak ve kullanıcıya ulaştırılacak.

Ancak burada önemli bir nokta mevcut. O da bu index.html dosyasının kendi içinde ihtiyaç duyduğu CSS veya JS dosyaları. Bu dosyalar da, index.html okunurken sunucuya bir GET tipinde request mesajı olarak geri döner. Bu requestler için klasik HTTP modülü kullunılan sunucularda manuel olarak bütün dosyaları okumak ve request url’sine göre bu dosyaların gönderilme işlemini ayarlamak gerekmektedir.

Örneğin:

```jsx
const http = require('http')
const {readFileSync} = require('fs')

const anasayfa = readFileSync('./index.html')
const style = readFileSync('.style.css')
const javascript = readFileSync('index.js')

const server = http.createServer((req, res) => {
	const url = req.url
	
	if (url === '/') {
		res.writeHeader(200, {'content-type': 'text/html'})
		res.write(anasayfa)
		res.end()
	}
	
	if (req === '/style.css') {
		res.writeHeader(200, {'content-type': 'text/css'})
		res.write(style)
		res.end()
	}
	
	if (req === '/index.js') {
		res.writeHeader(200, {'content-type': 'text/javascript'})
		res.write(style)
		res.end()
	}
})
```

Ancak bu işlem içerisinde onlarca, yüzlerce dosya barındıran projeler için verimsiz ve sürdürülemez. Bu sorunun çözümü ise Express.js. 

---
## Express.js

Express.js bir Node.js kütüphanesidir (framework). Bu framework sayesinde orijinal (vanilla) Node.js’de yapılması çetrefilli olan dosya gönderimi gibi birçok işlem çok daha kolay bir şekilde yapılabilmekte. 

Express.js, bir framework olduğundan dolayı Node.js kurulumunda otomatik olarak kurulmamaktadır. Bu sebeple harici bir modül olan Express.js, ancak ‘npm’ ile indirildikten sonra kulllanılabilir.

Aşağıda basit bir Express.js sunucusu oluşturulmuştur:

```jsx
const {express} = require('express')
const app = express()

app.get('/', (req, res) => {
	console.log('Kullanıcı giriş yaptı.')
	res.status(200).send('Ana Sayfa')
})

app.get('/about', (req, res) => {
	res.status(200).send('Hakkımızda Sayfası')
})

app.all('*', (req, res) => {
	res.status(404).send('<h1>Aradğınız sayfa bulunamadı!</h1>')
})

app.listen('5000', () => {
	console.log('Port 5000 dinleniyor.')
})
```

Aşağıda, Express ile oluşturulan sunucularla en çok kullanılan metodlar bulunmaktadır: 

```jsx
// app.get: Client tarafından gelen get isteklerini alır.
// app.post: Client tarafından gelen post isteklerini alır.
// app.delete: Client tarafından gelen delete isteklerini alır.
// app.all: Client tarafından gelen bütün isteklerini alır.
// app.use: Ara katman (Middleware) oluşturmak için kullanılır.
// app.listen: Belirtilen alan adını (domain) veya portu sürekli olarak dinler.
```

### Express.js ile Static Dosya Sunma

Daha önce vanilla Node.js’de belirttiğimiz çoklu dosya sunma işleminin manuel yapılma mecburiyetinin getirdiği zorunluluk Express.js ile kolayca çözülebiliyor.  

express.static metodu ile belirlenen bir konumdaki büütn dosyalar sabit (static) olarak algılanıyor ve sunucuya bu dosyalar için request geldiğinde otomatik olarak istenilen dosya client’e gönderiliyor. Böylece geliştiricinin teker teker her dosyası import edip bütün bu dosyalar için ayrı ‘if’ blokları yazmasına da gerek kalmıyor.

Örnek:

  

```jsx
const {express} = require('express')
const {path} = require('path')
const app = express()

// Middleware oluşturularak belirtilen klasöründeki bütün dosyaların konumları belirlendi
// Bu dosyalar talep edildiğinde gönderilmek üzere kaydedildi.
app.use(express.static('./public'))

app.get('/', (req, res) => {
	res.sendFile(path.resolve(__dirname, './örnek/dosya.html'))
})

app.all('*', (req, res) => {
	res.statu(404).send('Aradığınız sayfa bulunamadı.')
})

app.listen('5000', () => {
	console.log('5000. port dinleniyor.')
})
```

---
## Sayfa Parametreleri (Route Params)

Route Params, request edilem URL ile sunucuya veri aktararak bu veriye göre response alınmasını sağlayan bir yapıdır.

Örneğin bir e-ticaret sitesinde ürünlerin gösterildiği bir route olduğunu düşünelim. Bu route ile backend’e request gönderildiğinge bütün ürünler response olarak alınacaktır. Ancak kullanıcı bir ürüne tıklayıp o ürünün sayfasına gitmek istediğinde sunucuya gönderilen requestte özel olarak bu ürünü belirtilmesi zorunludur. Bunu yağmak için ‘route params’ kullanılabilir.

Örnek:

```jsx
// Vitrin sayfasının URL: app/products/

app.get('app/products/', (req, res) => {
	res.status(200).send(products)
})

// Özel olarak bir ürünün URL: app/products/:productId
// Değişken olarak kullanılacak olan query string alanının başına ':' konulur.

app.get('app/products/:productId', (req, res) => {
	const {productId} = req.params
	
	const requestedProduct = products.find(
    (product) => product.id === Number(productID)
  )
  if (!requestedProduct) {
    return res.status(404).send('Product Does Not Exist')
  }

  return res.json(requestedProduct)
	
	res.status(200).send(responseProducts)
})
```

### If Bloklarında Response Göndermek için Return Kullanımı

If blokları içerisinde response gönderileceği durumlarda eğer blok dışında da bir response gönderimi var ise blok içerisindeki response öncesinde return anahtar kelimesini kullanmak mecburîdir.

Bunun sebebi ise return kullanılmadığında blok içerisindeki response gönderildikten sonra Node.js’in kodu okumaya devam etmesi ve blok dışındaki response kodunu da çalıştırmasıdır. Ve bir requeste aynı app metodu içerisinde (örn. app.get, app.post) yanlızca bir response gönderilebilir olmasıdır. Res.send ise app metodunu sonlandırmadığından çoklu response dönme ihtimali olan durumlarda hata almamak için mutlaka return kullanılmalıdır.

Örnek:

```jsx
const express = require('express')
const app = express()

app.get('app/products/:productId', (req, res) => {
	const {productId} = req.params
	
	const requestedProduct = products.find(
    (product) => product.id === Number(productID)
  )
  if (!requestedProduct) {
	  // Burada return kullanılmazsa sonraki response da gönderilecek ve bu da bir hataya sebep olacaktır.
    // return anahtarı aktif olan app.get dönügüsünden çıkılmasını sağlar. Ancak res bunu yapmaz.
    return res.status(404).send('Product Does Not Exist')
  }

  return res.json(requestedProduct)
	
	res.status(200).send(responseProducts)
})

app.listen('5000')
```

---
## Sorgu Metni (Query String)

Route params ile basit bir sorgu olarak spesifik bir sayfayı sorgulama işlemi yapılabileceği gibi ‘query stringler’ ile de daha gelişmiş sorgulamalar yapılabilir. Yine aynı mantıkla URL ile bazı sorgu parametreleri gönderilir ve sunucu bu parametrelerle veriyi filtreleyip istenilen cevabı gönderir.

Query stringleri URL’ye eklemek için ‘?’ işareti kullanılır. ‘?’ işaretinden sonrası query string sayılır. Aralara ‘&’ eklenerek birden fazla query string belirtilebilir. Query string olarak kullanılacak değişkenlerin backend’de kayıtlı olması gerekmektedir.

Örneğin:

```jsx
app/products/?search=telefon&limit=20
```

Aksi hâlde bunlar bir hüküm ifade etmezler.

Genel query string kullanımı örneği:

```jsx
const express = require('express')
const app = express()
const { products } = require('./data')

app.get('/', (req, res) => {
  res.send('<h1> Home Page</h1><a href="/api/products">products</a>')
})

app.get('/api/v1/query', (req, res) => {
  // console.log(req.query)
  const { search, limit } = req.query
  let sortedProducts = [...products]

  if (search) {
    sortedProducts = sortedProducts.filter((product) => {
      return product.name.startsWith(search)
    })
  }
  if (limit) {
    sortedProducts = sortedProducts.slice(0, Number(limit))
  }
  if (sortedProducts.length < 1) {
    // res.status(200).send('no products matched your search');
    return res.status(200).json({ sucess: true, data: [] })
  }
  res.status(200).json(sortedProducts)
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

---
## Orta Katman (Middleware)

Middleware, request mesajları ile response mesajları arasında bulunan işlem katmanına verilen isimdir. Server’a bir request geldiğinde response mesajı göndermeden önce bu katmanda belli işlemler yapılır ve bu aşamanın ardından response mesajı gönderilir.

Middleware raporlama (loglama), güvenlik kontrolü (authentication) gibi birçok kritik işlem için kullanılabilir.

Ayrıca middleware aşamasındaki işlemin sonucu response aşamasına da yansıtılabilir. Örneğin kullanıcı doğrulaması yani authtentication aşaması başarısız olursa response bir hata mesajı döndürülebilir.

Basit bir middleware yapısı şu şekildedir:

```jsx
const express = require('express')
const app = express()

//  req => middleware => res

const logger = (req, res, next) => {
  const method = req.method
  const url = req.url
  const time = new Date().getFullYear()
  console.log(method, url, time)
  
  next() // next() sonraki aşamaya geçilmesini sağlar ve zorunludur. Eğer kullanılmazsa client yüklenme döngüsüne düşer.
}

app.get('/', logger, (req, res) => {
  res.send('Home')
})

app.get('/about', logger, (req, res) => {
  res.send('About')
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

Ancak bu yapının zayıf tarafı middlware fonksiyonlarının doğrudan ‘her istek için manuel olarak ayrı ayrı’ çağrılması gerektiğidir. Örneğin onlarca farklı istek için her metoda manuel olarak middleware eklemek zordur. Bunun için belirtilen routelara otomatik olarak belirtilen middlewareları ekleyen ‘**app.use**’ metodu mevcuttur.

İyileştirilmiş kullanım şu şekildedir:

```jsx
const express = require('express')
const app = express()
const logger = require('./logger')
const authorize = require('./authorize')

//  req => middleware => res
app.use('/', [logger, authorize]) // Route belirtilmediğinde bütün routelara middleware uygulanır.

app.get('/', (req, res) => {
  res.send('Home')
})

app.get('/about', (req, res) => {
  res.send('About')
})

app.get('/api/products', (req, res) => {
  res.send('Products')
})

app.get('/api/items', (req, res) => {
  console.log(req.user)
  res.send('Items')
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

Uygulamaların kapsamı genişledikçe ihtiyaç duyulan middleware türü de artmaktadır. Bu ihtiyacı karşılamanın bir yolu sıfırdan middleware fonksiyonları yazmak olabileceği gibi Express.js’in kendi sapladığı middlewareları kullanmak ve NPM üzerindeki middlewareları indirmek gibi yöntemler de bulunmaktadır.

Örneğin:

```jsx
app.use([logger, authorize]) // Manuel olarak oluşturulabilirler.
app.use(express.static('./public')) // Dosya yollarını belirlemek için Express tarafından sağlanır.
app.use(morgan('tiny') // Loglama için NPM'den ile indirilmiştir.
```

---
## Örnek Metod Kullanımları

Aşağıda farklı request metodlarının basit bir örneği bulunmaktadır:

```jsx
const express = require('express')
const app = express()
let { people } = require('./data')

// static assets
app.use(express.static('./methods-public'))
// parse form data
app.use(express.urlencoded({ extended: false }))
// parse json
app.use(express.json())

app.get('/api/people', (req, res) => {
  res.status(200).json({ success: true, data: people })
})

app.post('/api/people', (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).json({ success: true, person: name })
})

app.post('/api/postman/people', (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).json({ success: true, data: [...people, name] })
})

app.post('/login', (req, res) => {
  const { name } = req.body
  if (name) {
    return res.status(200).send(`Welcome ${name}`)
  }

  res.status(401).send('Please Provide Credentials')
})

app.put('/api/people/:id', (req, res) => {
  const { id } = req.params
  const { name } = req.body

  const person = people.find((person) => person.id === Number(id))

  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${id}` })
  }
  const newPeople = people.map((person) => {
    if (person.id === Number(id)) {
      person.name = name
    }
    return person
  })
  res.status(200).json({ success: true, data: newPeople })
})

app.delete('/api/people/:id', (req, res) => {
  const person = people.find((person) => person.id === Number(req.params.id))
  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${req.params.id}` })
  }
  const newPeople = people.filter(
    (person) => person.id !== Number(req.params.id)
  )
  return res.status(200).json({ success: true, data: newPeople })
})

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})
```

---
## Yönlendirici (Router) Yapısı

Klasik Express.js uygulamalarında olduğu gibi bütün istekleri doğrudan app objesi üzerinde gerçekleştirmek kod karmaşasına yol açmakta ve okunabilirliği düşürmektedir. Bu sorunu çözmek için her farklı route için bir dosya oluşturarak ve özel bir Router yapısı kullanabiliriz.

Örneğin:

```jsx
// Ana dosya: 
const express = require('express')
const app = express()

const people = require('./routes/people')
const auth = require('./routes/auth')

// static assets
app.use(express.static('./methods-public'))
// parse form data
app.use(express.urlencoded({ extended: false }))
// parse json
app.use(express.json())

app.use('/api/people', people)
app.use('/login', auth)

app.listen(5000, () => {
  console.log('Server is listening on port 5000....')
})

// people.js:
const express = require('express')
const router = express.Router()

const {
  getPeople,
  createPerson,
  createPersonPostman,
  updatePerson,
  deletePerson,
} = require('../controllers/people')

// Birinci kullanım tarzı:
// router.get('/', getPeople)
// router.post('/', createPerson)
// router.post('/postman', createPersonPostman)
// router.put('/:id', updatePerson)
// router.delete('/:id', deletePerson)

// İkinci kullanım tarzı:
router.route('/').get(getPeople).post(createPerson)
router.route('/postman').post(createPersonPostman)
router.route('/:id').put(updatePerson).delete(deletePerson)

module.exports = router

// auth.js:
const express = require('express')
const router = express.Router()

router.post('/', (req, res) => {
  const { name } = req.body
  if (name) {
    return res.status(200).send(`Welcome ${name}`)
  }

  res.status(401).send('Please Provide Credentials')
})

module.exports = router
```

Bu kullanım sayesinde ana dosya çok daha temiz ve okunabilir hâle getirilmiş oldu.

### Router yapısında kontrolcüler (Controllers)

Okunabilirliği daha da artırmak için controllers kullanımı da önemlidir. Controllers kullanımının mantığı router.get(’/’) metodundaki ikinci parametre olan callback fonksiyonunu başka bir dosyada fonksiyon olarak oluşturup router dosyasına sadece isim olarak aktararak kullanmaktır.

Örneğin:

```jsx
// Controllers/people.js:
let { people } = require('../data')

const getPeople = (req, res) => {
  res.status(200).json({ success: true, data: people })
}

const createPerson = (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).send({ success: true, person: name })
}

const createPersonPostman = (req, res) => {
  const { name } = req.body
  if (!name) {
    return res
      .status(400)
      .json({ success: false, msg: 'please provide name value' })
  }
  res.status(201).send({ success: true, data: [...people, name] })
}

const updatePerson = (req, res) => {
  const { id } = req.params
  const { name } = req.body

  const person = people.find((person) => person.id === Number(id))

  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${id}` })
  }
  const newPeople = people.map((person) => {
    if (person.id === Number(id)) {
      person.name = name
    }
    return person
  })
  res.status(200).json({ success: true, data: newPeople })
}

const deletePerson = (req, res) => {
  const person = people.find((person) => person.id === Number(req.params.id))
  if (!person) {
    return res
      .status(404)
      .json({ success: false, msg: `no person with id ${req.params.id}` })
  }
  const newPeople = people.filter(
    (person) => person.id !== Number(req.params.id)
  )
  return res.status(200).json({ success: true, data: newPeople })
}

module.exports = {
  getPeople,
  createPerson,
  createPersonPostman,
  updatePerson,
  deletePerson,
}
```
