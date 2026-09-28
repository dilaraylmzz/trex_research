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