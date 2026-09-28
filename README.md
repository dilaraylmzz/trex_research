# .NET Backend Geliştirme - Temel Bilgi ve Kavramlar Araştırma Raporu

Bu repository, modern backend geliştirme süreçleri, .NET ekosistemi, mimari desenler, veritabanı yönetimi, güvenlik pratikleri ve yazılım tasarım prensiplerini kapsayan araştırma ve raporlama çalışmasıdır.

---

## 1. Modern Yazılım Geliştirme Pratikleri

### 1.1 Git ve GitHub Nedir?
- **Git:** Kaynak kodların tarihçesini versiyonlar halinde kaydeden, birden fazla geliştiricinin aynı anda çakışmadan çalışabilmesini sağlayan dağıtık bir versiyon kontrol sistemidir.
- **GitHub:** Git altyapısını kullanan, projelerin bulutta barındırılmasını, ekip çalışmasını, kod incelemelerini (Pull Request) ve CI/CD süreçlerini yöneten bulut tabanlı bir platformdur.

#### Temel Git Komutları
- `git init`: Bulunulan dizinde yeni bir yerel Git deposu (.git) başlatır.
- `git clone <url>`: Uzak sunucudaki bir depoyu yerel bilgisayara indirir.
- `git add <dosya>`: Değişiklikleri hazırlık alanına (staging area) alır.
- `git commit -m "mesaj"`: Hazırlık alanındaki kodları açıklama ile yerel geçmişe kaydeder.
- `git push origin <dal>`: Yerel commit'leri uzak GitHub deposuna aktarır.
- `git pull origin <dal>`: Uzak depodaki güncellemeleri çekip yerel kodla birleştirir.
- `git branch <ad>`: Yeni bir çalışma dalı (branch) açar veya dalları listeler.
- `git merge <dal>`: Belirtilen daldaki kodları üzerinde çalışılan aktif dala entegre eder.

### 1.2 Merge Conflict Nedir ve Nasıl Çözülür?
**Merge Conflict (Birleştirme Çakışması):** İki farklı dalda aynı dosyanın aynı satırlarında çakışan değişiklikler yapıldığında ve Git hangi değişikliğin geçerli olduğunu otomatik olarak belirleyemediğinde ortaya çıkar.

**Çözüm Adımları:**
1. `git status` ile çakışan dosyalar tespit edilir.
2. Çakışan dosya açılır ve Git'in eklediği işaretler (`<<<<<<<`, `=======`, `>>>>>>>`) incelenir.
3. İhtiyaç duyulan kod satırları korunur, çakışma etiketleri silinir.
4. Dosya kaydedildikten sonra `git add .` ve `git commit -m "fix: merge conflict giderildi"` çalıştırılarak birleştirme tamamlanır.

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

---

## 2. .NET Ekosistemi

### 2.1 .NET Tarihçesi ve Platform Karşılaştırması
- **.NET Framework:** 2002 yılında yayımlanan, sadece Windows işletim sisteminde çalışan eski mimaridir.
- **.NET Core:** 2016 yılında sıfırdan yazılan, açık kaynaklı, hafif, modüler ve çapraz platform (Windows, Linux, macOS) destekleyen sürümdür.
- **.NET 7/8/9+:** Framework ve Core ayrımını tamamen sonlandıran, bulut uyumlu, yüksek performanslı ve birleşik modern .NET platformudur.

| Kriter | .NET Framework | .NET Core | Modern .NET (.NET 8/9+) |
|---|---|---|---|
| Platform Desteği | Yalnızca Windows | Windows, Linux, macOS | Windows, Linux, macOS |
| Açık Kaynak | Hayır | Evet | Evet |
| Performans | Standart | Yüksek | Çok Yüksek (AOT desteği) |
| Mimari | Monolitik | Modüler (NuGet tabanlı) | Modüler ve Bulut Uyumlu |

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
- Makinede en güncel **.NET 9 SDK (9.0.300)** yüklüdür; bu sayede C# 13 sözdizimi ile backend geliştirme yapılabilir.
- Sistemde hem **.NET 8 (LTS)** hem de **.NET 9** çalışma zamanları (Runtime) yer almaktadır. Böylece her iki sürümle geliştirilmiş web servisleri sistemde derlenip çalıştırılabilir.
- `RID: win-x64`, uygulamanın 64-bit Windows işletim sistemi üzerinde hedeflendiğini gösterir.

### 2.3 Senkron ve Asenkron Programlama
- **Senkron:** Bir işlem tamamlanmadan bir sonraki işleme geçilmez. I/O (giriş/çıkış) işlemlerinde iş parçacığı (thread) bloke olur ve sistem kaynakları kilitlenir.
- **Asenkron:** Uzun süren veritabanı veya ağ işlemlerinde thread bloklanmaz; istek tamamlanana kadar thread başka talepleri işlemek üzere havuza (Thread Pool) serbest bırakılır.

#### Anahtar Kavramlar
- `async` / `await`: Asenkron metotları tanımlamak ve arka plandaki işlemi beklerken thread'i bloklamamak için kullanılır.
- `Task` / `Task<T>`: Gelecekte tamamlanacak olan bir asenkron işi temsil eder.
- `ConfigureAwait(false)`: Backend servislerinde iş parçacığının aynı SynchronizationContext üzerinde dönme zorunluluğunu kaldırarak performans sağlar ve deadlock riskini önler.
- `=>` (Expression-Bodied / Lambda): C#'ta tek satırlık metot veya property tanımlarını sadeleştiren ok operatörüdür.

```csharp
public async Task<UserDto> GetUserAsync(int id)
{
    // Veritabanı sorgulanırken thread serbest kalır
    var user = await _context.Users.FirstOrDefaultAsync(u => u.Id == id);
    return new UserDto => (user.Name, user.Email);
}
```

---

## 3. Backend Geliştirme Temelleri

### 3.1 Backend vs Frontend
- **Frontend (Ön Yüz):** Kullanıcının doğrudan etkileşime girdiği görsel arayüzdür (HTML, CSS, JavaScript, Flutter, React vb.). Kullanıcı deneyimi, veri sunumu ve görsel animasyonlar ile ilgilenir.
- **Backend (Arka Yüz):** Sistemin beyni olarak çalışan sunucu tarafıdır. İş mantığı (business logic), veritabanı işlemleri, güvenlik, yetkilendirme, veri doğrulama ve harici servis entegrasyonlarını yürütür.

### 3.2 Web Sunucusu ve API Türleri
- **Web Sunucusu:** İstemcilerden gelen HTTP isteklerini dinleyen, bunları işleyen ve ilgili yanıtı geri dönen yazılımdır (Örn: Kestrel, IIS, Nginx). .NET Core uygulamaları yerleşik olarak hafif ve yüksek performanslı **Kestrel** web sunucusunu kullanır.
- **API (Application Programming Interface):** Farklı yazılımların veya sistemlerin birbirleriyle standart kurallar çerçevesinde iletişim kurmasını sağlayan arayüzdür.
  - **REST API:** Web standartlarına (HTTP metodları ve durum kodları) dayalı, durumsuz (stateless) servisler.
  - **SOAP:** XML tabanlı, katı kontratlara (WSDL) sahip kurumsal servis protokolü.
  - **GraphQL:** İstemcinin yalnızca ihtiyaç duyduğu alanları sorgulayabildiği tek endpoint'li veri sorgulama dili.
  - **gRPC:** HTTP/2 ve Protocol Buffers (Protobuf) kullanan, mikroservisler arası ultra hızlı ikili (binary) iletişim protokolü.

### 3.3 HTTP Metodları ve Durum Kodları

| Metod | CRUD Karşılığı | Açıklama | Idempotent mı? |
|---|---|---|---|
| `GET` | Read | Kaynakları sorgulamak/okumak için kullanılır. Sunucuda durum değiştirmez. | Evet |
| `POST` | Create | Sunucuda yeni bir kaynak oluşturmak için kullanılır. | Hayır |
| `PUT` | Update | Belirtilen kaynağın tamamını güncellemek veya yoksa oluşturmak için kullanılır. | Evet |
| `DELETE` | Delete | Belirtilen kaynağı silmek için kullanılır. | Evet |

#### Yaygın HTTP Durum Kodları:
- `200 OK`: İstek başarılı.
- `201 Created`: Yeni kayıt başarıyla oluşturuldu.
- `400 Bad Request`: İstemci geçersiz veya eksik veri gönderdi.
- `401 Unauthorized`: Kimlik doğrulanmadı (Token eksik veya geçersiz).
- `403 Forbidden`: Kimlik doğrulandı ancak kaynağa erişim yetkisi yetersiz.
- `404 Not Found`: İstenen kaynak sunucuda bulunamadı.
- `500 Internal Server Error`: Sunucu tarafında beklenmeyen bir hata oluştu.

### 3.4 REST vs SOAP vs GraphQL Karşılaştırması

| Kriter | REST | SOAP | GraphQL |
|---|---|---|---|
| **Veri Formatı** | Çoğunlukla JSON (XML de destekler) | Yalnızca XML | JSON |
| **İletişim Kuralları** | HTTP fiilleri ve URL kaynakları | Katı kurallar (WSDL, SOAP Envelope) | Tip şeması (Schema Definition) |
| **Esneklik** | Orta (Sabit endpoint yanıtları) | Düşük (Sıkı kontrat bağlılığı) | Çok Yüksek (İstemci alanı seçer) |
| **Over-fetching / Under-fetching** | Yaşanabilir | Yaşanabilir | Çözülmüştür (Yalnızca istenen alan döner) |

### 3.5 JSON Veri Formatı
JSON (JavaScript Object Notation), platformlar arası veri transferinde standart haline gelmiş anahtar-değer (key-value) yapısıdır.

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

---

## 4. ASP.NET ve Yazılım Mimarileri

### 4.1 ASP.NET vs ASP.NET Core
- **ASP.NET (Legacy):** Yalnızca Windows ve IIS üzerinde çalışan, .NET Framework'e bağımlı eski monolitik web platformudur.
- **ASP.NET Core:** Açık kaynaklı, modüler, bağımsız platformlarda (Windows, Linux, Docker, macOS) çalışan, çok daha yüksek performans sunan modern web çerçevesidir.

### 4.2 MVC (Model-View-Controller) Deseni
- **Model:** Uygulamanın verisini, durumunu ve iş kurallarını temsil eder.
- **View:** Kullanıcıya sunulan görsel arayüz katmanıdır (Razor sayfaları, HTML).
- **Controller:** Kullanıcı isteklerini karşılayan, Model ile etkileşime geçen ve uygun View ya da JSON yanıtını dönen kontrol merkezidir.

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

### 4.5 Katmanlı Mimari (N-Tier Architecture)
Uygulama sorumluluklara göre yatay katmanlara ayrılır:
- **Presentation (Sunum):** Controller, API endpoint'leri ve arayüz katmanı.
- **Business Logic (İş Katmanı):** İş kuralları, validasyonlar ve servisler (`ProductService`).
- **Data Access Layer (DAL):** Veritabanı işlemleri, DbContext ve Repository sınıfları.

### 4.6 Clean Architecture (Temiz Mimari)
Uncle Bob tarafından ortaya konan bu mimaride temel kural **Bağımlılıkların Dışa Değil, İçe Doğru Akması İlkesidir (Dependency Inversion)**. Çekirdek iş kuralları veritabanından veya arayüzden tamamen bağımsızdır.

```text
[ API / Presentation Layer ]
           │
           ▼
[ Infrastructure Layer ] ──▶ [ Application Layer ]
                                     │
                                     ▼
                              [ Domain Layer ] (Çekirdek - Bağımsız)
```

- **Domain:** Varlıklar (Entities), Value Objects, Domain Events. Hiçbir dış kütüphaneye bağımlı değildir.
- **Application:** İş senaryoları, Use-Case'ler, DTO'lar, CQRS Handler'ları ve Interface tanımları.
- **Infrastructure:** Veritabanı erişimi (EF Core), e-posta gönderimi, harici servis adaptörleri.
- **API / Web:** HTTP isteklerini karşılayan ve sonuçları dönen dış kabuk.

## 5. Veritabanı ve ORM (Object-Relational Mapping)

### 5.1 SQL Nedir? İlişkisel (RDBMS) vs İlişkisel Olmayan (NoSQL) Veritabanları
- **SQL (Structured Query Language):** İlişkisel veritabanlarını sorgulamak, güncellemek ve yönetmek için kullanılan standart bildirimsel dildir.
- **RDBMS (İlişkisel):** Verileri katı şemalara sahip tablolarda, satır ve sütunlar halinde saklar. Tablolar arasında birincil (Primary Key) ve yabancı (Foreign Key) anahtarlarla ilişkiler kurulur. ACID (Atomicity, Consistency, Isolation, Durability) prensiplerine sıkı sıkıya bağlıdır.
  - *Örnekler:* PostgreSQL, MSSQL, MySQL.
- **NoSQL (İlişkisel Olmayan):** Esnek şemalı, büyük veri hacimlerini yatayda ölçekleyebilen (horizontal scaling) sistemlerdir. Belge (Document), anahtar-değer (Key-Value), kolon veya grafik tabanlı modeller kullanır.
  - *Örnekler:* MongoDB, Redis, Cassandra.

### 5.2 ORM ve Entity Framework Core Nedir?
- **ORM (Object-Relational Mapping):** Nesne yönelimli programlama dillerindeki nesneler (C# sınıfları) ile ilişkisel veritabanı tabloları arasında köprü kuran bir tekniktir. Geliştiriciyi ham SQL sorguları yazmaktan kurtarır.
- **Entity Framework Core (EF Core):** .NET ekosisteminin modern, açık kaynaklı, çapraz platform ve hafif ORM aracıdır.

### 5.3 `DbContext` Nedir ve Nasıl Çalışır?
`DbContext`, EF Core'un kalbidir. Veritabanı ile uygulama arasındaki oturumu temsil eder. Hem **Repository** hem de **Unit of Work** tasarım kalıplarının birleşik uygulamasıdır. Değişiklikleri izler (Change Tracking), sorguları SQL'e dönüştürür ve `SaveChangesAsync()` çağrıldığında tek bir transaction içinde veritabanına yansıtır.

### 5.4 Code-First vs Database-First Yaklaşımı

| Kriter | Code-First | Database-First |
|---|---|---|
| **Çıkış Noktası** | C# Entity sınıfları | Mevcut veritabanı şeması |
| **Yönetim** | EF Core Migrations (`dotnet ef migrations add`) | Scaffold komutları ile sınıfların tersine mühendislikle üretilmesi |
| **Avantajı** | Veritabanı bağımsızlığı, kod üzerinde tam sürüm kontrolü | Halihazırda var olan karmaşık ve eski (legacy) veritabanlarına kolay entegrasyon |
| **Kullanım Alanı** | Sıfırdan başlanan modern mikroservis / web projeleri | Kurumsal ve önceden tasarlanmış veritabanı projeleri |

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

---

## 6. Güvenlik ve Performans

### 6.1 Authentication vs Authorization
- **Authentication (Kimlik Doğrulama):** "Kullanıcı kim?" sorusunun yanıtıdır. Kullanıcının kimliğini doğrulamak için parola, iki adımlı doğrulama (2FA) veya token kontrolü yapılır.
- **Authorization (Yetkilendirme):** "Doğrulanan kullanıcının bu kaynağa erişim izni var mı?" sorusunun yanıtıdır. Rol ve yetki (Role-based, Policy-based, Claim-based) denetimlerini kapsar.

### 6.2 JWT (JSON Web Token) Mimarisi
JWT, taraflar arasında güvenli ve doğrulanabilir JSON nesneleri aktaran durumsuz (stateless) bir standarttır (RFC 7519). Üç bileşenden oluşur:
1. **Header:** Kullanılan algoritma (`HS256`, `RS256`) ve token tipini içerir.
2. **Payload:** Kullanıcı kimliği (sub), roller ve son geçerlilik tarihi (exp) gibi hak iddialarını (claims) taşır.
3. **Signature:** Header ve Payload'un sunucuya ait gizli bir anahtar (secret key) ile hash'lenmesiyle üretilir. Verinin yolda tahrif edilip edilmediğini doğrular.

### 6.3 OAuth 2.0, OpenID Connect ve OpenIddict İlişkisi
- **OAuth 2.0:** Bir yetkilendirme (authorization) protokolüdür. Kullanıcının şifresini paylaşmadan üçüncü taraf bir uygulamanın kaynaklara erişmesine izin verir (Örn: "Google hesabınla Spotify'a erişim ver").
- **OpenID Connect (OIDC):** OAuth 2.0 üzerine inşa edilmiş bir kimlik doğrulama (authentication) katmanıdır. `id_token` üreterek kullanıcının kim olduğunu doğrular.
- **OpenIddict:** .NET uygulamalarında bağımsız bir OAuth 2.0 ve OpenID Connect kimlik sunucusu (Identity Provider) kurmayı sağlayan popüler ve esnek bir kütüphanedir.

### 6.4 Backend Performans Optimizasyon Teknikleri
1. **`AsNoTracking()` Kullanımı:**
   - EF Core, okuduğu her nesneyi bellekte izler (change tracking). Sadece listeleme yapılan `GET` isteklerinde `.AsNoTracking()` kullanıldığında bellek tüketimi düşer ve sorgu çalışma hızı ciddi oranda artar.
   - *Örnek:* `_context.Products.AsNoTracking().ToListAsync();`
2. **Önbellekleme (Caching - In-Memory ve Dağıtık Redis):**
   - Sık erişilen ve az değişen veriler doğrudan veritabanından çekilmek yerine belleğe yazılır. Çok sunuculu (load-balanced) ortamlarda **Redis** dağıtık cache çözümü olarak sunucular arası veri tutarlılığı sağlar.
3. **`IAsyncEnumerable` ile Akış (Streaming):**
   - Büyük veri setleri çekilirken tüm listenin belleğe (RAM) dolması yerine, veritabanından gelen veriler geldikçe anlık olarak istemciye akıtılır (stream edilir). Bu sayede sunucu bellek tüketimi minimuma indirilir.

### 6.5 OWASP Top 10 Güvenlik Açıkları ve ASP.NET Core Önlemleri

| Açık Adı | Tanım | ASP.NET Core Savunması |
|---|---|---|
| **A01: Broken Access Control** | Kullanıcıların yetkisi dışındaki kaynaklara erişebilmesi. | Endpoint'lerde `[Authorize(Roles = "Admin")]` ve policy bazlı denetimler kullanmak. |
| **A02: Cryptographic Failures** | Hassas verilerin (şifreler, kartlar) güvensiz saklanması/iletilmesi. | Zorunlu HTTPS (`app.UseHttpsRedirection()`) ve BCrypt/Argon2 ile parola hash'leme. |
| **A03: Injection (SQLi, Command)** | Zararlı sorguların veya komutların girdi alanlarından veritabanına sızması. | EF Core parametreli sorguları varsayılan uygular; asla string concatenation ile SQL yazılmamalıdır. |
| **A04: Insecure Design** | Yazılım mimarisinin en baştan tehdit modellemesi yapılmadan kurgulanması. | Güvenli kodlama standartları ve rate-limiting ile brute-force saldırılarını engellemek. |
| **A05: Security Misconfiguration** | Varsayılan şifreler, açık bırakılan debug portları veya detaylı hata mesajları. | Production ortamında `UseDeveloperExceptionPage` kapatılmalı, hassas header'lar temizlenmelidir. |
| **A06: Vulnerable Components** | Güvenlik açığı bulunan güncel olmayan üçüncü taraf NuGet paketleri. | `dotnet list package --vulnerable` komutu ile paket güvenlik taraması yapmak. |
| **A07: Identification and Auth Failures** | Zayıf parola politikası, oturum sabitleme veya brute-force açıkları. | ASP.NET Core Identity ile güçlü parola kuralları, hesap kilitleme ve 2FA zorunluluğu. |
| **A08: Software and Data Integrity Failures** | Doğrulanmamış kaynaklardan gelen güncellemeler ve güvensiz deserialization. | JSON serileştirmesinde System.Text.Json kullanmak, CI/CD paket imzalamalarını doğrulamak. |
| **A09: Security Logging & Monitoring Failures** | Yetkisiz erişimlerin loglanmaması veya saldırı anında alarm üretilmemesi. | Serilog/ELK gibi merkezi loglama araçlarıyla denetim (audit) logları tutmak. |
| **A10: Server-Side Request Forgery (SSRF)** | Sunucunun saldırgan tarafından hedeflenen uzak bir kaynağa istek yapmaya zorlanması. | Dışa giden isteklerde IP/Domain beyaz listelemesi (whitelisting) yapmak. |

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
            Detailed = exception.Message // Production ortamında gizlenmelidir
        };

        return context.Response.WriteAsync(JsonSerializer.Serialize(response));
    }
}
```

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

### 8.2 Tasarım Desenleri (Design Patterns)
- **Singleton Pattern:** Bir sınıftan uygulama yaşam döngüsü boyunca yalnızca tek bir örneğin oluşturulmasını garanti eder.
  - *Kullanım Senaryosu:* Konfigürasyon yöneticisi, bellek içi önbellek (MemoryCache) yönetimi.
- **Repository Pattern:** Veritabanı erişim mantığını iş mantığından soyutlar; veritabanı türü değiştiğinde iş katmanının etkilenmemesini sağlar.
- **Factory Pattern:** Nesne üretim sürecini istemciden gizleyerek bir arayüz veya üst sınıf üzerinden dinamik nesne üretilmesini sağlar.

### 8.3 Clean Code (Temiz Kod) Nedir?
Clean Code; okunması, anlaşılması ve üzerinde geliştirme yapılması kolay, gereksiz karmaşıklıktan arındırılmış koddur.
- **İsimlendirme:** Kısaltmalardan kaçınılmalı, değişken ve metot adları işlevini doğrudan açıklamalıdır (`d` yerine `daysSinceLastLogin`).
- **Fonksiyon Boyutu:** Fonksiyonlar tek bir iş yapmalı ve ideal olarak 20 satırı geçmemelidir.
- **Yan Etkilerden Kaçınma:** Bir metot hem sorgulama yapıp hem de arkada veri tabanını sessizce değiştirmemelidir (CQS - Command Query Separation).

### 8.4 Yazılım Mimari Desenlerinin Karşılaştırılması

| Mimari Türü | Temel Yaklaşım | Avantajı | Hangi Senaryoda Tercih Edilir? |
|---|---|---|---|
| **Layered (Katmanlı)** | Sunum, İş, Veri katmanları yatay dizilir. | Kurulumu ve öğrenmesi çok kolaydır. | Küçük ve orta ölçekli projeler, MVP çalışmaları. |
| **Clean Architecture** | Bağımlılıklar içe (Domain çekirdeğine) doğrudur. | Test edilebilirlik en üst düzeydedir; framework/DB bağımsızdır. | Uzun ömürlü, karmaşık iş kuralları olan kurumsal projeler. |
| **Microservices** | Sistem bağımsız çalışan küçük servislere bölünür. | Bağımsız deploy edilebilir, farklı teknolojiler kullanılabilir, yatay ölçeklenir. | Çok büyük ekipler, yüksek trafikli ve modüler büyük sistemler. |
| **Event-Driven** | Servisler mesaj kuyrukları (RabbitMQ/Kafka) ile haberleşir. | Servisler arası asenkron çalışma ve tam gevşek bağlılık (loose coupling). | Anlık yoğun trafik alan sipariş, bildirim ve finans sistemleri. |
| **Hexagonal (Ports & Adapters)** | Çekirdek uygulama giriş/çıkış portları ile dış dünyadan yalıtılır. | Dış bağımlılıkların kolayca mock'lanabilmesi ve değiştirilebilirliği. | Sık sık dış API ve entegrasyon değiştiren sistemler. |