# .NET Backend Geliştirme – Temel Bilgi ve Kavramlar Araştırma Raporu

Bu repository, modern backend geliştirme süreçleri, .NET ekosistemi, mimari desenler, veritabanı yönetimi, güvenlik pratikleri ve yazılım tasarım prensiplerini kapsayan araştırma ve raporlama çalışmasıdır.

---

## 1. Modern Yazılım Geliştirme Pratikleri

### 1.1 Git ve GitHub Nedir?
- **Git:** Kaynak kodların tarihçesini versiyonlar halinde kaydeden, birden fazla geliştiricinin aynı proje üzerinde değişiklikleri takip ederek birlikte çalışmasını sağlayan dağıtık bir versiyon kontrol sistemidir.
- **GitHub:** Git altyapısını kullanan, projelerin bulutta barındırılmasını, ekip çalışmasını, kod incelemelerini (Pull Request) ve CI/CD süreçlerini yöneten bulut tabanlı bir platformdur.

#### Temel Git Komutları
- `git init`: Bulunulan dizinde yeni bir yerel Git deposu (.git) başlatır.
- `git clone <url>`: Uzak sunucudaki bir depoyu yerel bilgisayara indirir.
- `git add <dosya>`: Değişiklikleri hazırlık alanına (staging area) alır.
- `git commit -m "mesaj"`: Hazırlık alanındaki kodları açıklama ile yerel geçmişe kaydeder.
- `git push origin <dal>`: Yerel commit'leri uzak GitHub deposuna aktarır.
- `git pull origin <dal>`: Uzak depodaki güncellemeleri çekip yerel kodla birleştirir.
- `git branch <ad>`: Yeni bir çalışma dalı oluşturur. `git branch` komutu ise mevcut dalları listeler.
- `git merge <dal>`: Belirtilen daldaki kodları üzerinde çalışılan aktif dala entegre eder.


> 💡 **Kendi yorumum:** Git'in yalnızca kodu saklamak için değil, yapılan değişikliklerin geçmişini takip etmek ve ekip çalışmasını düzenlemek için önemli olduğunu düşünüyorum. Özellikle branch kullanımının farklı özellikleri birbirinden bağımsız geliştirmeyi kolaylaştırması benim için dikkat çekici oldu.

### 1.2 Merge Conflict Nedir ve Nasıl Çözülür?
**Merge Conflict (Birleştirme Çakışması):** İki farklı dalda aynı dosyanın aynı satırlarında çakışan değişiklikler yapıldığında ve Git hangi değişikliğin geçerli olduğunu otomatik olarak belirleyemediğinde ortaya çıkar.

**Çözüm Adımları:**
1. `git status` ile çakışan dosyalar tespit edilir.
2. Çakışan dosya açılır ve Git'in eklediği işaretler (`<<<<<<<`, `=======`, `>>>>>>>`) incelenir.
3. İhtiyaç duyulan kod satırları korunur, çakışma etiketleri silinir.
4. Dosya kaydedildikten sonra `git add .` ve `git commit -m "fix: merge conflict giderildi"` çalıştırılarak birleştirme tamamlanır.


> 💡 **Kendi yorumum:** Merge conflict'in aslında Git'in hatası değil, iki değişiklik arasında otomatik karar veremediği bir durum olduğunu düşünüyorum. Bu nedenle conflict çözmeyi bilmek ekip çalışması açısından önemli bir beceri.

### 1.3 CI/CD Nedir? .NET Projelerinde Nasıl Uygulanır?
- **CI (Continuous Integration):** Geliştiricilerin kodlarını sık aralıklarla ana depoya göndermesi ve her push işleminde uygulamanın otomatik olarak derlenip (build) testlerinin çalıştırılmasıdır.
- **CD (Continuous Delivery/Deployment):** Testleri başarıyla geçen kodun test, staging veya canlı (production) ortamlara otomatik olarak paketlenip yayınlanmasıdır.

#### .NET GitHub Actions Pipeline Örneği:
Proje kök dizininde `.github/workflows/dotnet-ci.yml` dosyası tanımlanarak otomatik test ve derleme süreci kurgulanabilir:

```yaml
name: .NET CI Pipeline

on:
  push:
    branches: [ "main" ]

jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: .NET Kurulumu
      uses: actions/setup-dotnet@v4
      with:
        dotnet-version: '9.0.x'
    - name: Bağımlılıkları Yükle
      run: dotnet restore
    - name: Derle (Build)
      run: dotnet build --no-restore --configuration Release
    - name: Testleri Çalıştır
      run: dotnet test --no-build --verbosity normal
```


> 💡 **Kendi yorumum:** CI/CD'nin en önemli avantajının test ve derleme gibi tekrar eden işlemleri otomatikleştirmek olduğunu düşünüyorum. Böylece geliştiricinin küçük bir değişiklikten sonra sistemi manuel olarak kontrol etme yükü azalıyor.

### 1.4 SDLC (Yazılım Geliştirme Yaşam Döngüsü) ve Metodolojiler
SDLC adımları ve backend geliştiricinin süreçteki rolü:
1. **Planlama & Analiz:** İhtiyaçların belirlenmesi. Backend geliştirici sistem mimarisini ve veri modellerini kurgular.
2. **Tasarım:** Mimari kararlar (Clean Architecture), API şemaları ve veritabanı şeması tasarlanır.
3. **Geliştirme:** İş kuralları kodlanır, veritabanı bağlantıları kurulur ve API uç noktaları yazılır.
4. **Test:** Birim (Unit), entegrasyon ve yük testleri yapılarak sistem doğrulanır.
5. **Dağıtım:** CI/CD pipeline'ları ile canlıya aktarım sağlanır.
6. **Bakım & İzleme:** Hata düzeltmeleri, log takibi ve performans optimizasyonları yürütülür.

**Metodolojiler:**
- **Agile:** Hızlı geri bildirim alan ve değişime hızla uyum sağlayan esnek yazılım geliştirme felsefesidir.
- **Scrum:** Agile prensiplerini 1-4 haftalık "Sprint" adı verilen döngülerle, roller (Scrum Master, Product Owner, Developer) ve toplantılarla yürüten çerçevedir.
- **Kanban:** İş adımlarını panoda görselleştiren, aynı anda devam eden iş sayısını (WIP) sınırlayarak verimliliği artıran sürekli akış modelidir.


> 💡 **Kendi yorumum:** SDLC'nin yazılım geliştirmenin yalnızca kod yazmaktan ibaret olmadığını göstermesi benim için önemliydi. Planlama, test, dağıtım ve bakım aşamalarının da kaliteli bir ürün için gerekli olduğunu düşünüyorum.

---

## 2. .NET Ekosistemi

### 2.1 .NET Tarihçesi ve Platform Karşılaştırması
- **.NET Framework:** 2002 yılında yayımlanan, sadece Windows işletim sisteminde çalışan eski mimaridir.
- **.NET Core:** 2016 yılında sıfırdan yazılan, açık kaynaklı, hafif, modüler ve çapraz platform (Windows, Linux, macOS) destekleyen sürümdür.
- **.NET 5 ve sonrası:** .NET Framework ile .NET Core ayrımını modern .NET çatısı altında birleştiren, açık kaynaklı ve çapraz platform bir platform ailesidir. .NET 7, .NET 8 ve .NET 9 bu modern sürüm ailesinin örnekleridir.

| Kriter | .NET Framework | .NET Core | Modern .NET (.NET 8/9+) |
|---|---|---|---|
| Platform Desteği | Yalnızca Windows | Windows, Linux, macOS | Windows, Linux, macOS |
| Açık Kaynak | Büyük ölçüde kapalı/legacy yapı | Evet | Evet |
| Performans | Standart | Yüksek | Çok Yüksek (AOT desteği) |
| Mimari | Monolitik | Modüler (NuGet tabanlı) | Modüler ve Bulut Uyumlu |


> 💡 **Kendi yorumum:** .NET'in Windows odaklı eski yapısından çapraz platform modern .NET yapısına geçişi, günümüzde neden Linux ve Docker gibi ortamlarda da sık kullanıldığını açıklıyor. Özellikle tek bir ekosistem içinde farklı platformlara uygulama geliştirebilmek önemli bir avantaj.

### 2.2 `dotnet --info` Terminal Çıktısı ve Değerlendirmesi
Geliştirme ortamında çalıştırılan `dotnet --info` komutunun çıktısı:

```text
.NET SDK:
 Version:           9.0.300
 Commit:            15606fe0a8
 MSBuild version:   17.14.5+edd3bbf37

Çalışma Zamanı Ortamı:
 OS Name:      Windows
 OS Version:   10.0.26200
 OS Platform:  Windows
 RID:          win-x64
 Base Path:    C:\Program Files\dotnet\sdk\9.0.300\

Host:
  Version:      9.0.5
  Architecture: x64

.NET runtimes installed:
  Microsoft.AspNetCore.App 8.0.16
  Microsoft.AspNetCore.App 9.0.5
  Microsoft.NETCore.App 8.0.16
  Microsoft.NETCore.App 9.0.5
  Microsoft.WindowsDesktop.App 9.0.5
```

**Çıktı Analizi ve Yorumu:**
- Bu geliştirme ortamında **.NET 9.0.300 SDK** yüklüdür. .NET 9 ile uyumlu C# dil özellikleri kullanılabilir.
- Sistemde hem **.NET 8 (LTS)** hem de **.NET 9** çalışma zamanları (Runtime) yer almaktadır. Böylece her iki sürümle geliştirilmiş web servisleri sistemde derlenip çalıştırılabilir.
- `RID: win-x64`, uygulamanın 64-bit Windows işletim sistemi üzerinde hedeflendiğini gösterir.


> 💡 **Kendi yorumum:** Kendi bilgisayarımda `dotnet --info` çıktısını incelemek, SDK ile runtime arasındaki farkı daha net anlamamı sağladı. Birden fazla runtime sürümünün aynı bilgisayarda bulunabilmesinin farklı projelerle çalışırken faydalı olduğunu düşünüyorum.

### 2.3 Senkron ve Asenkron Programlama
- **Senkron:** Bir işlem tamamlanmadan bir sonraki işleme geçilmez. I/O (giriş/çıkış) işlemlerinde iş parçacığı (thread) bloke olur ve sistem kaynakları kilitlenir.
- **Asenkron:** Uzun süren veritabanı veya ağ işlemlerinde bekleme sırasında thread bloklanmaz; böylece Thread Pool içindeki kaynaklar başka işlerde kullanılabilir.

#### Anahtar Kavramlar
- `async` / `await`: Asenkron metotları tanımlamak ve arka plandaki işlemi beklerken thread'i bloklamamak için kullanılır.
- `Task` / `Task<T>`: Gelecekte tamamlanacak olan bir asenkron işi temsil eder.
- `ConfigureAwait(false)`: Bir await sonrasında mevcut `SynchronizationContext` bağlamına geri dönme gereksinimini kaldırır. ASP.NET Core uygulamalarında klasik ASP.NET/GUI ortamlarındaki gibi bir request `SynchronizationContext` bulunmadığından çoğu uygulama kodunda buna özel olarak ihtiyaç duyulmaz; kütüphane kodlarında daha anlamlı olabilir.
- `=>` (Expression-Bodied / Lambda): C#'ta tek satırlık metot veya property tanımlarını sadeleştiren ok operatörüdür.

```csharp
public async Task<UserDto> GetUserAsync(int id)
{
    // Veritabanı sorgulanırken thread serbest kalır
    var user = await _context.Users.FirstOrDefaultAsync(u => u.Id == id);
    if (user == null)
        return null;

    return new UserDto
    {
        Name = user.Name,
        Email = user.Email
    };
}
```


> 💡 **Kendi yorumum:** Asenkron programlamanın özellikle veritabanı ve ağ işlemleri gibi bekleme süresi olan işlemlerde önemli olduğunu düşünüyorum. Buradaki temel kazanımın işlemi hızlandırmaktan çok bekleyen thread'i gereksiz yere meşgul etmemek olduğunu anladım.

---

## 3. Backend Geliştirme Temelleri

### 3.1 Backend vs Frontend
- **Frontend (Ön Yüz):** Kullanıcının doğrudan etkileşime girdiği görsel arayüzdür (HTML, CSS, JavaScript, Flutter, React vb.). Kullanıcı deneyimi, veri sunumu ve görsel animasyonlar ile ilgilenir.
- **Backend (Arka Yüz):** Sistemin beyni olarak çalışan sunucu tarafıdır. İş mantığı (business logic), veritabanı işlemleri, güvenlik, yetkilendirme, veri doğrulama ve harici servis entegrasyonlarını yürütür.


> 💡 **Kendi yorumum:** Frontend ve backend ayrımını birlikte düşündüğümüzde bir uygulamanın görünen kısmı ile arka plandaki iş mantığının farklı sorumluluklara sahip olduğu daha net görülüyor. Backend tarafının güvenlik ve veri yönetimi açısından kritik olduğunu düşünüyorum.

### 3.2 Web Sunucusu ve API Türleri
- **Web Sunucusu:** İstemcilerden gelen HTTP isteklerini kabul eden ve yanıtların iletilmesini sağlayan yazılımdır (Örn: Kestrel, IIS, Nginx). ASP.NET Core uygulamalarında **Kestrel** yaygın olarak kullanılan yerleşik web sunucusudur; IIS veya Nginx gibi sunucular reverse proxy olarak da konumlandırılabilir.
- **API (Application Programming Interface):** Farklı yazılımların veya sistemlerin birbirleriyle standart kurallar çerçevesinde iletişim kurmasını sağlayan arayüzdür.
  - **REST API:** HTTP kaynakları, metodları ve durum kodlarından yararlanan; stateless tasarımın yaygın olduğu bir API yaklaşımıdır.
  - **SOAP:** XML tabanlı, katı kontratlara (WSDL) sahip kurumsal servis protokolü.
  - **GraphQL:** İstemcinin ihtiyaç duyduğu alanları şema üzerinden sorgulamasını sağlayan bir API sorgulama dilidir; uygulamalarda çoğunlukla tek endpoint yaklaşımı kullanılır.
  - **gRPC:** HTTP/2 ve Protocol Buffers (Protobuf) kullanan, mikroservisler arası ultra hızlı ikili (binary) iletişim protokolü.


> 💡 **Kendi yorumum:** API'lerin farklı uygulamaların birbirleriyle iletişim kurmasını sağlayan bir sözleşme gibi çalıştığını düşünüyorum. REST, SOAP, GraphQL ve gRPC'nin aynı ihtiyaca farklı teknik yaklaşımlar sunması, proje gereksinimine göre teknoloji seçmenin önemli olduğunu gösteriyor.

### 3.3 HTTP Metodları ve Durum Kodları

| Metod | CRUD Karşılığı | Açıklama | Idempotent mı? |
|---|---|---|---|
| `GET` | Read | Kaynakları sorgulamak/okumak için kullanılır. Sunucuda durum değiştirmez. | Evet |
| `POST` | Create | Sunucuda yeni bir kaynak oluşturmak için kullanılır. | Hayır |
| `PUT` | Update | Bir kaynağın temsiliyle değiştirilmesi/güncellenmesi için kullanılır; bazı API tasarımları upsert davranışı da tanımlayabilir. | Evet |
| `DELETE` | Delete | Belirtilen kaynağı silmek için kullanılır. | Evet |

#### Yaygın HTTP Durum Kodları:
- `200 OK`: İstek başarılı.
- `201 Created`: Yeni kayıt başarıyla oluşturuldu.
- `400 Bad Request`: İstemci geçersiz veya eksik veri gönderdi.
- `401 Unauthorized`: Kimlik doğrulanmadı (Token eksik veya geçersiz).
- `403 Forbidden`: Kimlik doğrulandı ancak kaynağa erişim yetkisi yetersiz.
- `404 Not Found`: İstenen kaynak sunucuda bulunamadı.
- `500 Internal Server Error`: Sunucu tarafında beklenmeyen bir hata oluştu.


> 💡 **Kendi yorumum:** HTTP metodlarını ve durum kodlarını doğru kullanmanın API'nin anlaşılabilirliğini artırdığını düşünüyorum. Özellikle 401 ile 403 arasındaki farkın kimlik doğrulama ve yetkilendirme ayrımını anlamak açısından önemli olduğunu gördüm.

### 3.4 REST vs SOAP vs GraphQL Karşılaştırması

| Kriter | REST | SOAP | GraphQL |
|---|---|---|---|
| **Veri Formatı** | Çoğunlukla JSON; XML de kullanılabilir | XML | JSON gibi farklı veri temsilleri kullanılabilir |
| **İletişim Kuralları** | HTTP fiilleri ve URL kaynakları | Katı kurallar (WSDL, SOAP Envelope) | Tip şeması (Schema Definition) |
| **Esneklik** | Orta (Sabit endpoint yanıtları) | Düşük (Sıkı kontrat bağlılığı) | Çok Yüksek (İstemci alanı seçer) |
| **Over-fetching / Under-fetching** | Yaşanabilir | Yaşanabilir | Çözülmüştür (Yalnızca istenen alan döner) |


> 💡 **Kendi yorumum:** Bu üç yaklaşımın birbirinin doğrudan alternatifi gibi düşünülmemesi gerektiğini düşünüyorum. Veri ihtiyacı, mevcut sistemler ve entegrasyon gereksinimleri hangi yaklaşımın uygun olacağını belirleyebilir.

### 3.5 JSON Veri Formatı
JSON (JavaScript Object Notation), platformlar arası veri alışverişinde yaygın kullanılan hafif bir veri serileştirme formatıdır. Nesne, dizi, metin, sayı, boolean ve null gibi veri türlerini destekler.

```json
{
  "id": 101,
  "title": "Clean Architecture ile .NET",
  "author": {
    "name": "Dilara Yılmaz",
    "role": "Backend Developer"
  },
  "tags": ["dotnet", "csharp", "architecture"],
  "isActive": true
}
```


> 💡 **Kendi yorumum:** JSON'un sade ve okunabilir olması nedeniyle web API'lerinde yaygın kullanılmasını anlaşılır buluyorum. Nesne ve dizi gibi yapıların desteklenmesi, karmaşık verilerin de düzenli şekilde taşınmasını kolaylaştırıyor.

---

## 4. ASP.NET ve Yazılım Mimarileri

### 4.1 ASP.NET vs ASP.NET Core
- **ASP.NET (Legacy):** Yalnızca Windows ve IIS üzerinde çalışan, .NET Framework'e bağımlı eski monolitik web platformudur.
- **ASP.NET Core:** Açık kaynaklı, modüler, bağımsız platformlarda (Windows, Linux, Docker, macOS) çalışan, çok daha yüksek performans sunan modern web çerçevesidir.


> 💡 **Kendi yorumum:** ASP.NET Core'un çapraz platform ve modüler yapısının modern backend geliştirmeye daha uygun olduğunu düşünüyorum. Özellikle Docker ve Linux ortamlarıyla birlikte kullanılabilmesi benim açımdan önemli.

### 4.2 MVC (Model-View-Controller) Deseni
- **Model:** Uygulamanın verisini, durumunu ve iş kurallarını temsil eder.
- **View:** Kullanıcıya sunulan görsel arayüz katmanıdır (Razor sayfaları, HTML).
- **Controller:** Kullanıcı isteklerini karşılayan, Model ile etkileşime geçen ve uygun View ya da JSON yanıtını dönen kontrol merkezidir.


> 💡 **Kendi yorumum:** MVC'nin uygulamadaki sorumlulukları ayırarak kodun daha düzenli hale gelmesine yardımcı olduğunu düşünüyorum. Backend API geliştirirken Controller'ın istekleri karşılayan katman olarak konumlanması bu ayrımı anlamayı kolaylaştırıyor.

### 4.3 Middleware Nedir ve Çalışma Mantığı?
Middleware, HTTP istek ve yanıt hattına (pipeline) eklenen ara yazılımlardır. Her middleware gelen isteği işleyebilir, bir sonraki adıma iletebilir (`next()`) ya da isteği sonlandırabilir (short-circuit).

#### `Program.cs` İçerisindeki Sıralamanın Önemi:
Sıralama kritiktir; yetkilendirme yapılmadan önce kimlik doğrulanmalıdır:
```csharp
var app = builder.Build();

app.UseExceptionHandler("/error"); // 1. Hataları en dışta yakalar
app.UseHttpsRedirection();         // 2. HTTPS yönlendirmesi
app.UseRouting();                  // 3. Rota eşleştirme
app.UseAuthentication();           // 4. "Sen kimsin?" (Kimlik Doğrulama)
app.UseAuthorization();            // 5. "Yetkin var mı?" (Yetkilendirme)

app.MapControllers();              // 6. Endpoint çalıştırma
app.Run();
```


> 💡 **Kendi yorumum:** Middleware sırasının sonucu doğrudan etkileyebilmesi dikkatimi çekti. Özellikle authentication ve authorization işlemlerinin doğru sırada çalışması, sadece kodu yazmanın değil pipeline'ı doğru tasarlamanın da önemli olduğunu gösteriyor.

### 4.4 Dependency Injection (DI) ve Servis Yaşam Döngüleri
Dependency Injection, sınıfların bağımlı olduğu nesneleri kendileri üretmek yerine dışarıdan (Inversion of Control - IoC konteynerinden) almasını sağlayan tekniktir. Bu sayede kod loosely coupled (gevşek bağlı) ve test edilebilir hale gelir.

#### Servis Yaşam Döngüleri (Lifetimes):
1. **Transient (`AddTransient`):** Her talep edildiğinde yeni bir örnek (instance) üretilir.
2. **Scoped (`AddScoped`):** Tek bir HTTP isteği boyunca aynı örnek kullanılır, istek tamamlandığında bellekten temizlenir (Örn: `DbContext`).
3. **Singleton (`AddSingleton`):** Uygulama çalıştığı sürece yalnızca tek bir örnek oluşturulur ve tüm istekler aynı nesneyi paylaşır (Örn: Caching servisleri).

```csharp
// Program.cs içerisinden kayıt:
builder.Services.AddScoped<IProductService, ProductService>();
builder.Services.AddSingleton<ICacheService, MemoryCacheService>();
```


> 💡 **Kendi yorumum:** Dependency Injection'ın sınıfları doğrudan birbirine bağlamak yerine bağımlılıkları dışarıdan vermesi kodun test edilmesini kolaylaştırıyor. Scoped, Singleton ve Transient seçimlerinin de uygulamanın davranışını doğrudan etkilediğini düşünüyorum.

### 4.5 Katmanlı Mimari (N-Tier Architecture)
Uygulama sorumluluklara göre yatay katmanlara ayrılır:
- **Presentation (Sunum):** Controller, API endpoint'leri ve arayüz katmanı.
- **Business Logic (İş Katmanı):** İş kuralları, validasyonlar ve servisler (`ProductService`).
- **Data Access Layer (DAL):** Veritabanı işlemleri, DbContext ve Repository sınıfları.


> 💡 **Kendi yorumum:** Katmanlı mimarinin özellikle öğrenme aşamasında sorumlulukları görselleştirmek için anlaşılır bir yaklaşım olduğunu düşünüyorum. Her katmanın görevini ayırmak, projenin büyümesiyle oluşabilecek karmaşıklığı azaltabilir.

### 4.6 Clean Architecture (Temiz Mimari)
Uncle Bob tarafından ortaya konan bu mimaride temel kural **Bağımlılıkların Dışa Değil, İçe Doğru Akması İlkesidir (Dependency Inversion)**. Çekirdek iş kuralları veritabanından veya arayüzden tamamen bağımsızdır.

```text
[ API / Presentation Layer ]
           │
           ▼
[ Application Layer ] ───────▶ [ Domain Layer ]
           ▲                           ▲
           │                           │
[ Infrastructure Layer ] ─────────────┘
```

- **Domain:** Varlıklar (Entities), Value Objects, Domain Events. Hiçbir dış kütüphaneye bağımlı değildir.
- **Application:** İş senaryoları, Use-Case'ler, DTO'lar, CQRS Handler'ları ve Interface tanımları.
- **Infrastructure:** Veritabanı erişimi (EF Core), e-posta gönderimi, harici servis adaptörleri.
- **API / Web:** HTTP isteklerini karşılayan ve sonuçları dönen dış kabuk.

## 5. Veritabanı ve ORM (Object-Relational Mapping)


> 💡 **Kendi yorumum:** Clean Architecture'ın benim için en önemli noktası iş kurallarının veritabanı veya framework gibi dış teknolojilere bağımlı olmamasını hedeflemesi. Bu yaklaşımın uzun ömürlü projelerde değişiklik yapmayı kolaylaştırabileceğini düşünüyorum.

### 5.1 SQL Nedir? İlişkisel (RDBMS) vs İlişkisel Olmayan (NoSQL) Veritabanları
- **SQL (Structured Query Language):** İlişkisel veritabanlarını sorgulamak, güncellemek ve yönetmek için kullanılan standart bildirimsel dildir.
- **RDBMS (İlişkisel):** Verileri katı şemalara sahip tablolarda, satır ve sütunlar halinde saklar. Tablolar arasında birincil (Primary Key) ve yabancı (Foreign Key) anahtarlarla ilişkiler kurulur. ACID (Atomicity, Consistency, Isolation, Durability) prensiplerine sıkı sıkıya bağlıdır.
  - *Örnekler:* PostgreSQL, MSSQL, MySQL.
- **NoSQL (İlişkisel Olmayan):** Esnek şemalı, büyük veri hacimlerini yatayda ölçekleyebilen (horizontal scaling) sistemlerdir. Belge (Document), anahtar-değer (Key-Value), kolon veya grafik tabanlı modeller kullanır.
  - *Örnekler:* MongoDB, Redis, Cassandra.


> 💡 **Kendi yorumum:** SQL ve NoSQL'un birbirinden tamamen biri iyi biri kötü şeklinde ayrılmaması gerektiğini düşünüyorum. Veri yapısı, tutarlılık ihtiyacı ve ölçekleme gereksinimleri hangi veritabanının kullanılacağını belirlemeli.

### 5.2 ORM ve Entity Framework Core Nedir?
- **ORM (Object-Relational Mapping):** Nesne yönelimli programlama dillerindeki nesneler (C# sınıfları) ile ilişkisel veritabanı tabloları arasında köprü kuran bir tekniktir. Geliştiriciyi ham SQL sorguları yazmaktan kurtarır.
- **Entity Framework Core (EF Core):** .NET ekosisteminin modern, açık kaynaklı, çapraz platform ve hafif ORM aracıdır.


> 💡 **Kendi yorumum:** EF Core'un C# nesneleri üzerinden veritabanıyla çalışmayı kolaylaştırması geliştirici açısından büyük avantaj. Ancak ORM kullanırken SQL'in temel mantığını bilmenin performans sorunlarını anlamak için gerekli olduğunu düşünüyorum.

### 5.3 `DbContext` Nedir ve Nasıl Çalışır?
`DbContext`, EF Core'un kalbidir. Veritabanı ile uygulama arasındaki oturumu temsil eder. EF Core açısından `DbContext`, **Unit of Work** yaklaşımını doğrudan destekler; `DbSet` ise repository benzeri veri erişim işlevleri sağlar. Bu nedenle ayrıca Repository katmanı eklemek her projede zorunlu değildir. Değişiklikleri izler (Change Tracking), sorguları SQL'e dönüştürür ve `SaveChangesAsync()` çağrıldığında tek bir transaction içinde veritabanına yansıtır.


> 💡 **Kendi yorumum:** DbContext'i öğrendikten sonra EF Core'un yalnızca SQL sorgusu çalıştıran bir araç olmadığını, değişiklik takibi ve transaction yönetimi gibi süreçlerde de rol aldığını daha iyi anladım.

### 5.4 Code-First vs Database-First Yaklaşımı

| Kriter | Code-First | Database-First |
|---|---|---|
| **Çıkış Noktası** | C# Entity sınıfları | Mevcut veritabanı şeması |
| **Yönetim** | EF Core Migrations (`dotnet ef migrations add`) | Scaffold komutları ile sınıfların tersine mühendislikle üretilmesi |
| **Avantajı** | Veritabanı bağımsızlığı, kod üzerinde tam sürüm kontrolü | Halihazırda var olan karmaşık ve eski (legacy) veritabanlarına kolay entegrasyon |
| **Kullanım Alanı** | Sıfırdan başlanan modern mikroservis / web projeleri | Kurumsal ve önceden tasarlanmış veritabanı projeleri |


> 💡 **Kendi yorumum:** Code-First yaklaşımının sıfırdan geliştirilen projelerde kod ve veritabanı şemasını birlikte ilerletmeyi kolaylaştırdığını düşünüyorum. Database-First ise hazır ve kurumsal veritabanlarına bağlanırken daha anlamlı bir seçenek olabilir.

### 5.5 Temel SQL Sorguları ve Karşılık Gelen LINQ İfadeleri

1. **Seçme / Filtreleme (SELECT & WHERE):**
   - *SQL:* `SELECT * FROM Products WHERE Price > 100 AND IsActive = 1;`
   - *LINQ:*
     ```csharp
     var products = await _context.Products
         .Where(p => p.Price > 100 && p.IsActive)
         .ToListAsync();
     ```

2. **Ekleme (INSERT):**
   - *SQL:* `INSERT INTO Products (Title, Price, IsActive) VALUES ('Laptop', 25000, 1);`
   - *LINQ / EF Core:*
     ```csharp
     var product = new Product { Title = "Laptop", Price = 25000, IsActive = true };
     await _context.Products.AddAsync(product);
     await _context.SaveChangesAsync();
     ```

3. **Güncelleme (UPDATE):**
   - *SQL:* `UPDATE Products SET Price = 27000 WHERE Id = 1;`
   - *LINQ / EF Core:*
     ```csharp
     var product = await _context.Products.FindAsync(1);
     if (product != null)
     {
         product.Price = 27000;
         await _context.SaveChangesAsync();
     }
     ```

4. **Silme (DELETE):**
   - *SQL:* `DELETE FROM Products WHERE Id = 1;`
   - *LINQ / EF Core:*
     ```csharp
     var product = await _context.Products.FindAsync(1);
     if (product != null)
     {
         _context.Products.Remove(product);
         await _context.SaveChangesAsync();
     }
     ```


> 💡 **Kendi yorumum:** Aynı işlemin SQL ve LINQ ile nasıl ifade edildiğini görmek ORM'un çalışma mantığını anlamama yardımcı oldu. LINQ kodu daha doğal görünse de arka planda hangi SQL'in oluştuğunu bilmenin önemli olduğunu düşünüyorum.

---

## 6. Güvenlik ve Performans

### 6.1 Authentication vs Authorization
- **Authentication (Kimlik Doğrulama):** "Kullanıcı kim?" sorusunun yanıtıdır. Kullanıcının kimliğini doğrulamak için parola, iki adımlı doğrulama (2FA) veya token kontrolü yapılır.
- **Authorization (Yetkilendirme):** "Doğrulanan kullanıcının bu kaynağa erişim izni var mı?" sorusunun yanıtıdır. Rol ve yetki (Role-based, Policy-based, Claim-based) denetimlerini kapsar.


> 💡 **Kendi yorumum:** Authentication ve authorization kavramlarının birbirinden ayrılması güvenlik açısından temel bir konu. Bir kullanıcının kimliğinin doğrulanmasının tek başına her kaynağa erişim hakkı vermediğini özellikle önemli buluyorum.

### 6.2 JWT (JSON Web Token) Mimarisi
JWT, taraflar arasında güvenli ve doğrulanabilir JSON nesneleri aktaran durumsuz (stateless) bir standarttır (RFC 7519). Üç bileşenden oluşur:
1. **Header:** Kullanılan algoritma (`HS256`, `RS256`) ve token tipini içerir.
2. **Payload:** Kullanıcı kimliği (sub), roller ve son geçerlilik tarihi (exp) gibi hak iddialarını (claims) taşır.
3. **Signature:** Header ve Payload üzerinde seçilen imzalama algoritmasına göre oluşturulur. HMAC tabanlı algoritmalarda gizli anahtar; RSA/ECDSA gibi algoritmalarda ise özel anahtar kullanılır. İmza, token'ın bütünlüğünün ve kaynağının doğrulanmasına yardımcı olur.


> 💡 **Kendi yorumum:** JWT'nin yapısını Header, Payload ve Signature olarak incelemek token'ın neden sadece kodlanmış bir kullanıcı bilgisi olmadığını anlamamı sağladı. Token'ın güvenli saklanması ve süresinin doğru yönetilmesi de en az token üretmek kadar önemli.

### 6.3 OAuth 2.0, OpenID Connect ve OpenIddict İlişkisi
- **OAuth 2.0:** Bir yetkilendirme (authorization) protokolüdür. Kullanıcının şifresini paylaşmadan üçüncü taraf bir uygulamanın kaynaklara erişmesine izin verir (Örn: "Google hesabınla Spotify'a erişim ver").
- **OpenID Connect (OIDC):** OAuth 2.0 üzerine inşa edilmiş bir kimlik doğrulama (authentication) katmanıdır. `id_token` üreterek kullanıcının kim olduğunu doğrular.
- **OpenIddict:** .NET uygulamalarında OAuth 2.0 ve OpenID Connect tabanlı yetkilendirme/kimlik sunucusu işlevleri geliştirmeye yardımcı olan bir kütüphanedir.


> 💡 **Kendi yorumum:** OAuth 2.0 ile OpenID Connect arasındaki farkı anlamanın önemli olduğunu düşünüyorum. OAuth daha çok yetkilendirme amacı taşırken OIDC kimlik doğrulama bilgisini bunun üzerine ekliyor.

### 6.4 Backend Performans Optimizasyon Teknikleri
1. **`AsNoTracking()` Kullanımı:**
   - EF Core, okuduğu her nesneyi bellekte izler (change tracking). Sadece listeleme yapılan `GET` isteklerinde `.AsNoTracking()` kullanıldığında bellek tüketimi düşer ve sorgu çalışma hızı ciddi oranda artar.
   - *Örnek:* `_context.Products.AsNoTracking().ToListAsync();`
2. **Önbellekleme (Caching - In-Memory ve Dağıtık Redis):**
   - Sık erişilen ve az değişen veriler doğrudan veritabanından çekilmek yerine belleğe yazılır. Çok sunuculu (load-balanced) ortamlarda **Redis** dağıtık cache çözümü olarak sunucular arası veri tutarlılığı sağlar.
3. **`IAsyncEnumerable` ile Asenkron İterasyon:**
   - Büyük veri setlerinde sonuçları parça parça asenkron olarak tüketmeye yardımcı olabilir. Gerçek anlamda istemciye streaming yapılabilmesi ise endpoint ve serializer yapılandırmasına da bağlıdır.


> 💡 **Kendi yorumum:** Performansın yalnızca donanım gücüyle ilgili olmadığını gördüm. Gereksiz veriyi çekmemek, cache kullanmak ve uygun asenkron veri erişimi gibi yazılım kararlarının da performans üzerinde doğrudan etkisi olduğunu düşünüyorum.

### 6.5 OWASP Top 10 (2021) Güvenlik Açıkları ve ASP.NET Core Önlemleri

| Açık Adı | Tanım | ASP.NET Core Savunması |
|---|---|---|
| **A01: Broken Access Control** | Kullanıcıların yetkisi dışındaki kaynaklara erişebilmesi. | Endpoint'lerde `[Authorize(Roles = "Admin")]` ve policy bazlı denetimler kullanmak. |
| **A02: Cryptographic Failures** | Hassas verilerin (şifreler, kartlar) güvensiz saklanması/iletilmesi. | Zorunlu HTTPS (`app.UseHttpsRedirection()`) ve BCrypt/Argon2 ile parola hash'leme. |
| **A03: Injection (SQLi, Command)** | Zararlı sorguların veya komutların girdi alanlarından veritabanına sızması. | EF Core parametreli sorguları varsayılan uygular; asla string concatenation ile SQL yazılmamalıdır. |
| **A04: Insecure Design** | Yazılım mimarisinin en baştan tehdit modellemesi yapılmadan kurgulanması. | Güvenli kodlama standartları ve rate-limiting ile brute-force saldırılarını engellemek. |
| **A05: Security Misconfiguration** | Varsayılan şifreler, açık bırakılan debug portları veya detaylı hata mesajları. | Production ortamında `UseDeveloperExceptionPage` kapatılmalı, hassas header'lar temizlenmelidir. |
| **A06: Vulnerable Components** | Güvenlik açığı bulunan güncel olmayan üçüncü taraf NuGet paketleri. | `dotnet list package --vulnerable` komutu ile paket güvenlik taraması yapmak. |
| **A07: Identification and Auth Failures** | Zayıf parola politikası, oturum sabitleme veya brute-force açıkları. | ASP.NET Core Identity ile güçlü parola kuralları, hesap kilitleme ve 2FA zorunluluğu. |
| **A08: Software and Data Integrity Failures** | Doğrulanmamış yazılım/veri değişiklikleri ve güvenilir olmayan kaynaklardan gelen bileşenler. | Bağımlılık ve build zincirini doğrulamak, güvenilir paket kaynakları kullanmak ve bütünlük kontrolleri uygulamak. |
| **A09: Security Logging & Monitoring Failures** | Yetkisiz erişimlerin loglanmaması veya saldırı anında alarm üretilmemesi. | Serilog/ELK gibi merkezi loglama araçlarıyla denetim (audit) logları tutmak. |
| **A10: Server-Side Request Forgery (SSRF)** | Sunucunun saldırgan tarafından hedeflenen uzak bir kaynağa istek yapmaya zorlanması. | Dışa giden isteklerde IP/Domain beyaz listelemesi (whitelisting) yapmak. |

**CSRF için kısa not:** CSRF, OWASP Top 10 (2021) içinde ayrı bir kategori olarak yer almaz; ancak özellikle cookie tabanlı kimlik doğrulamada dikkate alınması gereken bir saldırıdır. Anti-forgery token, SameSite cookie ayarları ve uygun Origin/Referer kontrolleri gibi önlemler kullanılabilir.


> 💡 **Kendi yorumum:** OWASP Top 10 listesinin backend geliştiriciler için bir kontrol listesi gibi kullanılabileceğini düşünüyorum. Güvenlik açıklarının önemli bir kısmı uygulama tasarlanırken alınabilecek önlemlerle azaltılabilir.

---

## 7. Logging ve Hata Yönetimi

### 7.1 Loglama Neden Önemlidir? Log Seviyeleri Nelerdir?
Loglama, uygulamanın arka plandaki davranışlarını, performans darboğazlarını ve çalışma zamanı hatalarını analiz edebilmek için hayati bir gözlemlenebilirlik (observability) bileşenidir.

#### Standart Log Seviyeleri (`LogLevel`):
- **Trace:** En ayrıntılı teşhis bilgileridir; sadece derin hata ayıklama süreçlerinde açılır.
- **Debug:** Geliştirme aşamasında akışı takip etmek için kullanılan bilgilerdir.
- **Information:** Sistemin normal çalışma akışındaki kritik olaylar (Örn: "Kullanıcı oturum açtı").
- **Warning:** Hata olmayan ancak potansiyel sorun teşkil edebilecek durumlar (Örn: "Disk doluluk oranı %85").
- **Error:** Mevcut işlemin başarısız olmasına yol açan ancak uygulamanın çalışmasını durdurmayan hatalar.
- **Critical:** Uygulamanın çökmesine yol açabilecek kritik sistem arızaları (Örn: "Veritabanına ulaşılamıyor").


> 💡 **Kendi yorumum:** Logların yalnızca hata olduğunda değil, sistemin normal davranışını ve performansını takip etmek için de önemli olduğunu düşünüyorum. Doğru log seviyesi seçimi gereksiz log kalabalığını da azaltabilir.

### 7.2 ASP.NET Core'da `ILogger` Kullanımı
ASP.NET Core yerleşik olarak bağımlılık enjeksiyonuna (DI) uygun `ILogger<T>` arayüzü sunar:

```csharp
public class OrderService
{
    private readonly ILogger<OrderService> _logger;

    public OrderService(ILogger<OrderService> logger)
    {
        _logger = logger;
    }

    public void ProcessOrder(int orderId)
    {
        _logger.LogInformation("Sipariş işleme alındı: {OrderId}", orderId);
        try
        {
            // Sipariş mantığı...
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Sipariş işlenirken beklenmeyen hata oluştu! Id: {OrderId}", orderId);
            throw;
        }
    }
}
```


> 💡 **Kendi yorumum:** `ILogger` kullanımında yapılandırılmış loglamanın, mesaj içine değerleri doğrudan birleştirmek yerine alanları ayrı tutması açısından faydalı olduğunu düşünüyorum. Bu yapı daha sonra logları filtrelemek ve analiz etmek için avantaj sağlayabilir.

### 7.3 Global Exception Handling (Merkezi Hata Yönetimi)
Hataların `try-catch` bloklarıyla her yere saçılması yerine, merkezi bir middleware ile yakalanması Clean Code açısından esastır. Bu yaklaşım hassas sunucu hatalarının istemciye sızmasını engeller ve standart `ProblemDetails` (RFC 7807) formatında yanıt döner:

```csharp
public class ExceptionMiddleware
{
    private readonly RequestDelegate _next;
    private readonly ILogger<ExceptionMiddleware> _logger;

    public ExceptionMiddleware(RequestDelegate next, ILogger<ExceptionMiddleware> logger)
    {
        _next = next;
        _logger = logger;
    }

    public async Task InvokeAsync(HttpContext httpContext)
    {
        try
        {
            await _next(httpContext);
        }
        catch (Exception ex)
        {
            _logger.LogError(ex, "Sunucu genelinde yakalanan hata: {Message}", ex.Message);
            await HandleExceptionAsync(httpContext, ex);
        }
    }

    private static Task HandleExceptionAsync(HttpContext context, Exception exception)
    {
        context.Response.ContentType = "application/json";
        context.Response.StatusCode = (int)HttpStatusCode.InternalServerError;

        var response = new
        {
            StatusCode = context.Response.StatusCode,
            Message = "Sunucu tarafında beklenmeyen bir hata meydana geldi.",
            // Teknik hata ayrıntıları istemciye gönderilmez; yalnızca sunucu loglarında tutulur.
        };

        return context.Response.WriteAsync(JsonSerializer.Serialize(response));
    }
}
```


> 💡 **Kendi yorumum:** Hataları merkezi olarak yönetmenin kod tekrarını azaltmasının yanında kullanıcıya teknik detayları göstermemek açısından da önemli olduğunu düşünüyorum. Detayların loglarda tutulup istemciye güvenli bir mesaj dönülmesi daha doğru bir yaklaşım.

---

## 8. Yazılım Geliştirme Prensipleri ve Tasarım Desenleri

### 8.1 SOLID Prensipleri
Yazılımın esnek, anlaşılır ve bakımı kolay olmasını sağlayan 5 temel nesne yönelimli tasarım ilkesidir:

1. **S - Single Responsibility Principle (SRP):** Bir sınıfın değişmesi için yalnızca tek bir nedeni olmalıdır. Tek bir işten sorumlu olmalıdır.
   - *Örnek:* Bir sınıf hem sipariş hesaplayıp hem de e-posta göndermemelidir. E-posta gönderimi `IEmailService` sınıfına devredilmelidir.
2. **O - Open/Closed Principle (OCP):** Sınıflar gelişime açık, ancak değişime kapalı olmalıdır. Yeni bir özellik eklemek mevcut çalışan kodu değiştirmemelidir.
   - *Örnek:* Ödeme yöntemleri için `if-else` yazmak yerine bir `IPaymentMethod` interface'i tanımlanıp yeni yöntemler (Kredi Kartı, Havale) bu interface'ten türetilmelidir.
3. **L - Liskov Substitution Principle (LSP):** Alt sınıflar, türedikleri üst sınıfların yerine kullanılabilmeli ve onların davranışını bozmamalıdır.
   - *Örnek:* `Kare`, `Dikdörtgen` sınıfından türetildiğinde en-boy bağımsız değiştirilemiyorsa LSP ihlal edilmiş olur.
4. **I - Interface Segregation Principle (ISP):** İstemciler kullanmadıkları metotları içeren geniş arayüzleri uygulamaya zorlanmamalıdır. Büyük arayüzler yerine amaca özel küçük arayüzler tercih edilmelidir.
   - *Örnek:* `IWorker` yerine `IWorkable` ve `IFeedable` arayüzlerinin ayrı tutulması (Robot çalışan yemek yemez).
5. **D - Dependency Inversion Principle (DIP):** Yüksek seviyeli modüller, düşük seviyeli modüllere doğrudan bağımlı olmamalıdır; her ikisi de soyutlamalara (interface/abstract class) bağımlı olmalıdır.
   - *Örnek:* `OrderService`, doğrudan `SqlDatabase` sınıfına değil, `IRepository` arayüzüne bağımlı olmalıdır.


> 💡 **Kendi yorumum:** SOLID prensiplerini katı kurallar olarak değil, kodun değiştirilebilirliğini ve bakımını kolaylaştıran rehberler olarak görüyorum. Özellikle Single Responsibility ve Dependency Inversion prensiplerinin backend projelerinde sık karşıma çıkacağını düşünüyorum.

### 8.2 Tasarım Desenleri (Design Patterns)
- **Singleton Pattern:** Bir sınıftan uygulama yaşam döngüsü boyunca yalnızca tek bir örneğin oluşturulmasını garanti eder.
  - *Kullanım Senaryosu:* Konfigürasyon yöneticisi, bellek içi önbellek (MemoryCache) yönetimi.
- **Repository Pattern:** Veritabanı erişim mantığını iş mantığından soyutlar; veritabanı türü değiştiğinde iş katmanının etkilenmemesini sağlar.
- **Factory Pattern:** Nesne üretim sürecini istemciden gizleyerek bir arayüz veya üst sınıf üzerinden dinamik nesne üretilmesini sağlar.


> 💡 **Kendi yorumum:** Design Pattern'ların hazır kod parçaları olmadığını, tekrar eden tasarım problemlerine yönelik çözüm yaklaşımları olduğunu anladım. Bu nedenle bir pattern'ı sadece kullanmış olmak için değil, gerçek bir ihtiyaç olduğunda tercih etmek gerektiğini düşünüyorum.

### 8.3 Clean Code (Temiz Kod) Nedir?
Clean Code; okunması, anlaşılması ve üzerinde geliştirme yapılması kolay, gereksiz karmaşıklıktan arındırılmış koddur.
- **İsimlendirme:** Kısaltmalardan kaçınılmalı, değişken ve metot adları işlevini doğrudan açıklamalıdır (`d` yerine `daysSinceLastLogin`).
- **Fonksiyon Boyutu:** Fonksiyonlar tek bir sorumluluğa odaklanmalı ve gereksiz yere büyümemelidir. 20 satır gibi kesin bir sınır evrensel bir kural değildir.
- **Yan Etkilerden Kaçınma:** Bir metot hem sorgulama yapıp hem de arkada veri tabanını sessizce değiştirmemelidir (CQS - Command Query Separation).


> 💡 **Kendi yorumum:** Clean Code açısından benim için en önemli nokta kodun sadece bilgisayar tarafından değil, başka bir geliştirici tarafından da kolay anlaşılabilmesi. Anlamlı isimler ve küçük, tek sorumluluklu metotların bakım sürecini kolaylaştıracağını düşünüyorum.

### 8.4 Yazılım Mimari Desenlerinin Karşılaştırılması

| Mimari Türü | Temel Yaklaşım | Avantajı | Hangi Senaryoda Tercih Edilir? |
|---|---|---|---|
| **Layered (Katmanlı)** | Sunum, İş, Veri katmanları yatay dizilir. | Kurulumu ve öğrenmesi çok kolaydır. | Küçük ve orta ölçekli projeler, MVP çalışmaları. |
| **Clean Architecture** | Bağımlılıklar içe (Domain çekirdeğine) doğrudur. | Test edilebilirlik en üst düzeydedir; framework/DB bağımsızdır. | Uzun ömürlü, karmaşık iş kuralları olan kurumsal projeler. |
| **Microservices** | Sistem bağımsız çalışan servis sınırlarına bölünür. | Bağımsız deploy ve ölçekleme yapılabilir. | Güçlü domain sınırları, bağımsız ölçekleme/deploy ihtiyacı ve operasyonel altyapısı uygun büyük sistemler. |
| **Event-Driven** | Servisler mesaj kuyrukları (RabbitMQ/Kafka) ile haberleşir. | Servisler arası asenkron çalışma ve tam gevşek bağlılık (loose coupling). | Anlık yoğun trafik alan sipariş, bildirim ve finans sistemleri. |
| **Hexagonal (Ports & Adapters)** | Çekirdek uygulama giriş/çıkış portları ile dış dünyadan yalıtılır. | Dış bağımlılıkların kolayca mock'lanabilmesi ve değiştirilebilirliği. | Sık sık dış API ve entegrasyon değiştiren sistemler. |

> 💡 **Kendi yorumum:** Her proje için tek bir mimarinin doğru olmadığını düşünüyorum. Küçük bir projede gereksiz karmaşıklık oluşturmak yerine ihtiyaca uygun sade bir yapı tercih edilirken, büyük ve değişken sistemlerde daha gelişmiş mimariler gerekli olabilir.
