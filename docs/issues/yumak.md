<!-- game: Yumak -->
<!-- repo: MustyYumak -->
# EPIC: Yumak — birleştir, olgunlaştır, ör: sepet taşsa da örgü kalır
labels: epic, enhancement

## Neden
Sahibi (2026-10-14): Suika tarzı bırak-birleştir mekaniğinin izlemesi keyifli ama mobil klonlarında "neden bir tur daha?" sorusunun cevabı yok; her tur sıfırdan başlar, kaybedilen her şey kaybolur. Yumak bu mekaniğin üstüne tek bir kural ekler: yeterince büyüyen (olgun) yumak sepetten çıkıp örgü panosuna gider ve bir giysiye satır ekler. Sepet taşınca tur biter ama örülen satırlar kaybolmaz; oyuncu "atkının 6/10 satırı bitti" diye geri gelir. Birleştirme üretime dönüşür, üretim koleksiyona (dolap), koleksiyon karaktere (Teyze'yi giydirme). Bu, Screwdom'un kanıtladığı "kalıcı ilerleme + kıtlık" tutma kancasının cozy bir sürümüdür.

Araştırma raporu ve tasarım taslakları: https://claude.ai/code/artifact/8ab0af47-8a63-4fac-8fd7-aad383f2fe81 (bölüm 5, diğer fikirler: Yumak) ve https://claude.ai/artifact/3VMA4d5qTn8GF89PDf5Uj9 (Yumak · Oyun: sepete düşen iç halka gölgeli yumaklar, "olgun · örgüye gidiyor" etiketi, üstte 10 sıralık atkı ilerlemesi 6 örülü, Dolap · 3, Sıradaki, "Sepeti genişlet · reklam").

Portföydeki rolü: Daily Drop'un fizik çekirdeğini ortak `MustyMergePhysics` paketine çıkarıp ikinci kez kullanan ilk oyun (portföyün "aynı motor, farklı kanca" tezinin kanıtı); 35+ kadın ve senior cozy kitlesine açılan ilk ürün; IAP ağırlıklı ilk gelir modeli. Solo MVP 4–5 hafta.

## Hedef kitle ve konumlandırma
- 35+ kadın ve senior (Fluffy Drop'un 60+ kitlesiyle kesişir), küresel EN + TR; "cozy", "no timer", "relaxing" arayan oyuncu. İkincil: örgü/el işi hobisi olan herkes (Ravelry/Pinterest kitlesi).
- Mağaza adı: **Yumak: Cozy Yarn Merge** (EN) / **Yumak: Örgü ve Yün Bulmacası** (TR). Anahtar kelimeler: cozy knitting game, yarn merge, relaxing, knit, no timer, cozy puzzle.
- Konumlandırma cümlesi: "Yumakları birleştir, olgunlaşanlar örgüye gitsin. Sepet taşsa da ördüğün satırlar kalır."
- Rakiplerden farkı: birleştirmenin kalıcı bir çıktısı var (giysi), tur kaybı ilerlemeyi silmez, prosedürel örgü deseni oyuncunun renk seçimiyle oluşur, karakter giydirme. Süre yok, enerji yok, oyun ekranında banner yok.

## Core loop ve kurallar (v1 kararları)
1. Oyuncu uygulamayı açar; Home'da sıradaki proje kartı ("Atkı · 6/10 satır"), Dolap (tamamlanan giysi sayısı) ve Teyze avatarı görünür. "Ör" düğmesi birincil.
2. Sepet: dikey, 9:16; genişlik ekranın %88'i, yükseklik %50'si; üstte kesikli taşma çizgisi (üst kenarın 48 px altı). Yumaklar 8 kademe (yarıçap px, 390 referans: 14, 18, 24, 30, 38, 46, 56, 68). Aynı kademeden iki yumak temas edince bir üst kademeye birleşir (`MustyMergePhysics`, Daily Drop ile aynı çekirdek). İki 8. kademe birleşince yok olmaz: 8. kademe zaten olgundur, 1,5 sn sonra panoya gider.
3. Renk: her yumağın kademeden bağımsız bir rengi vardır (projenin paleti: 4 renk). Birleşmede yeni yumağın rengi, düşen (hareketli) yumağın rengidir; oyuncu rengi bırakarak kontrol eder. Kademe boyut ve iç halka sayısıyla okunur (renk körü için de güvenli).
4. Olgunlaşma: kademe ≥ 6 bir yumak `maturity_hold_ms` (1.500) boyunca durağan (hız < 0,05) kalırsa "olgun" olur: üstünde "olgun · örgüye gidiyor" etiketi 0,5 sn görünür, yumak sepetten yukarı uçar (bezier, 600 ms) ve örgü panosuna satır ekler: 6. kademe = 1 satır, 7. = 2, 8. = 4. Satırların rengi yumağın rengidir. Uçan yumak fizikten çıkar, sepetteki diğer yumaklar yerleşir.
5. Örgü panosu: ekranın üstünde, aktif projenin siluet maskesi içinde satır satır dolar; her satır 10 ilmek ("V" sprite). Desen = satır renklerinin sırası (düz: tek renk; çizgili: ardışık farklı renkler; çapraz: her satırda 1 ilmek kayma — 8. story kuralları).
6. Projeler sırayla açılır ve her birinin satır hedefi vardır: Atkı 10, Bere 14, Çorap 18, Eldiven 16, Hırka 24, Kazak 30 (sezon 1 listesi 9. story'de). Proje tamamlanınca giysi dolaba girer, Teyze giydirilir, sıradaki proje açılır.
7. Tur: sonsuz; bir tur bir projeye katkıdır. Düşürme: parmağı yatay sürükle, bırak; sonraki bırakma 0,45 sn sonra. Sıradaki 3 yumak (kademe + renk) önizlemede. Sıra kademeleri 1–5, ağırlık {1: 0.30, 2: 0.25, 3: 0.20, 4: 0.15, 5: 0.10}; renk sırası paletten rastgele (aynı renk art arda en fazla 2).
8. Tur sonu: bir yumak taşma çizgisinin üstünde 1,2 sn kalırsa. "Devam (reklam)" tur başına bir kez: çizgi üstündeki yumaklar ve en küçük 3 yumak sepetten kaldırılır, tur sürer. Tur bitince satırlar panoda kalır; yalnızca sepet boşalır ve tur puanı kapanır.
9. Sepeti genişlet (ödüllü video): sepet iç genişliği %88 → %100 (ekran payı), yalnızca o tur; tur içinde bir kez.
10. Reklam: tur arası interstitial günde ≤ 3, ilk 3 turda hiç yok, iki interstitial arası ≥ 180 sn; No Ads (3,99 USD) tüm interstitial'ı kapatır, ödüllü videoyu kapatmaz. Oyun ekranında banner yok.
11. IAP: desen/giysi paketi 2,99 USD, No Ads 3,99 USD, sezon dolabı 4,99 USD. Coin yok; v1'de harcanacak para birimi olmadığı için sahte ekonomi de yok.
12. Meta: dolap 30 giysi, 3 sezon (10'ar giysi); avatar (Teyze) 2D katmanlı giydirme (gövde + giysi sprite + renk tint, tint = giysinin baskın rengi). Kalıcılık: `WardrobeState` ve `KnitProgress` Cloud Save.
13. Ses: yün/kumaş ASMR ailesi (yumuşak sürtünme, pamuk "puf"); örgü satırı eklenince "tık-tık" şiş sesi; proje tamamlanınca kısa makas + kumaş serme sesi. Müzik: 1 ambient döngü, Ayarlar'dan kapanır.

## Kapsam dışı (v1)
- Zaman sayacı, enerji, hamle sınırı (süre yok vaadi).
- Gerçek örgü tarifleri/PDF, arkadaşa hediye, çok oyunculu, lig.
- Karakter animasyonları (v1'de Teyze statik, yalnızca kıyafet katmanı değişir).
- Coin/para birimi ve coin mağazası.
- iOS çıkışı MVP'de yok (Android soft launch sonrası).

## Başarı ölçütleri (kapılar)
- Soft launch (TR, PH, CA; Android; 14 gün): D1 ≥ %35, D7 ≥ %15, ilk giysiyi (Atkı) tamamlayan oranı ≥ %50 (yükleyenler içinde, ilk 3 gün), ortalama oturum ≥ 4 dk, ödüllü video izleme/teklif ≥ %25, çökme oranı < %0,5.
- Kapı geçilirse: iOS build, desen paketi fiyat testi, sezon 2 içeriği, Canlı.
- Kapı geçilmezse: iki hafta iterasyon (olgunlaşma süresi, satır değerleri, ilk proje uzunluğu, FTUE), yine geçilmezse rafa; `MustyMergePhysics` paketi portföyde kalır.

## Temalar ve story'ler (sıra)
**P1 · MVP**
- [ ] Yumak 1: Core paketini bağla ve proje iskeleti
- [ ] Yumak 2: MustyMergePhysics paketini Daily Drop'tan çıkar ve bağla
- [ ] Yumak 3: Sepet, taşma çizgisi ve tur bitişi tespiti
- [ ] Yumak 4: 8 kademeli yumak seti, renk ve birleşme kuralı
- [ ] Yumak 5: Düşürme kontrolü, sıradaki önizleme ve renk sırası
- [ ] Yumak 6: Puanlama, zincir ve his: yün ASMR, haptik, şiş sesi
- [ ] Yumak 7: Olgunlaşma kuralı ve sepetten panoya uçuş
- [ ] Yumak 8: Örgü panosu ve prosedürel ilmek satırı render'ı
- [ ] Yumak 9: Giysi şablonları, proje sırası ve ilerleme kalıcılığı
- [ ] Yumak 10: Giysi tamamlama anı ve dolaba giriş
- [ ] Yumak 11: Dolap koleksiyonu, sezonlar ve avatar giydirme
- [ ] Yumak 12: Tur sonu akışı: taşma, Devam (reklam) ve satırların korunması
- [ ] Yumak 13: Ödüllü sepet genişletme, interstitial kotası ve No Ads
- [ ] Yumak 14: İlk tur deneyimi (FTUE)
- [ ] Yumak 15: Analitik olay şeması ve kapı raporu
- [ ] Yumak 16: Mağaza sayfası, ASO ve soft launch yapılandırması
**P2 · Soft launch**
- [ ] Yumak 17: Desen paketleri ve sezon dolabı IAP
- [ ] Yumak 18: Günlük hediye yumak, günlük hedef ve haftalık proje
- [ ] Yumak 19: Ayarlar ve erişilebilirlik (büyük yazı, tek el, sakin mod)
**P3 · Canlı**
- [ ] Yumak 20: Giysi kartı paylaşımı
**SONRA**
- [ ] Yumak 21: Gerçek örgü desenleri (PDF indir)
- [ ] Yumak 22: Arkadaşa giysi hediye
- [ ] Yumak 23: Karakter animasyonları
- [ ] Yumak 24: Sezon etkinlikleri

## Riskler ve açık sorular
- Paket çıkarma zamanlaması: Daily Drop soft launch iterasyonunda fizik değerleri değişebilir; `MustyMergePhysics` sürüm etiketiyle tüketilir, değerler oyun tarafındaki `PhysicsProfile` SO'da kalır (2. story).
- Kademe okunabilirliği: renk kademeye bağlı olmadığı için oyuncu boyutu yanlış okuyabilir. Önlem: iç halka sayısı + boyut farkı ≥ %25; Remote Config `tier_color_mode = free|tier` ile acil geri dönüş (renk = kademe, desen tek renk).
- Olgunlaşma hızı: 6. kademe çok kolay geliyorsa sepet hiç dolmaz ve gerilim kaybolur; çok zor geliyorsa satır gelmez. Başlangıç: 1.500 ms, kademe ≥ 6; soft launch verisiyle `maturity_min_tier` 6↔7 denenir.
- Klon algısı: Suika'dan görsel ayrışma yün/örgü evreniyle doğal; ama "yarn merge" aramasında rakip az olduğu için ASO fırsatı aynı zamanda niş küçüklüğü riski.
- Yeni tema label'ı `tema:orgu` STYLE listesine eklenir (ASCII slug kuralına uyar).
- Teyze adı ve çizimi TR'de sıcak, EN'de "Auntie"/"Granny" tonlaması test edilecek; v1'de avatar adı yerelleştirilebilir metin.

## Done when
P1 story'leri kapalı, `MustyMergePhysics` paketi hem Daily Drop hem Yumak tarafından aynı sürümle tüketiliyor, soft launch 14 gün tamamlanmış, kapı raporu epic'e yorum olarak eklenmiş ve karar (Canlı / iterasyon / raf) yazılmış.

## STORY: Yumak 1: Core paketini bağla ve proje iskeleti
labels: tema:backend, P1, efor:M
milestone: MVP

**Yumak** epic'inin **1. story'si** (epic #EPIC). Tema: Altyapı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** Unity projesi, `MustyCozyCore` (`com.mtafasahin.cozycore`, monorepo `mtafasahin-unity-packages`) UPM bağımlılığı, sahne düzeni, UGS kurulumu

### Kullanıcı hikâyesi
Bir geliştirici olarak Yumak projesinin ortak paketi (reklam, IAP, UGS, yerelleştirme, ayarlar, günlük hediye iskeleti) ilk günden kullanmasını isterim, böylece oyuna özgü işler (örgü, dolap, avatar) dışında hiçbir altyapıyı yeniden yazmam.

### Kabul kriterleri
- [ ] **Senaryo: Temiz kurulum** · Verilen boş bir Unity 6000.6 projesi · Eğer `MustyCozyCore` paketi git URL ve sürüm etiketiyle eklenirse · O zaman proje derlenir ve `Bootstrap` sahnesi açılınca UGS oturumu (anonim Player Accounts) kurulur.
- [ ] **Senaryo: Sahne akışı** · Verilen `Bootstrap → Home → Play → Wardrobe` sahneleri · Eğer uygulama açılırsa · O zaman 2 sn içinde Home görünür; geri tuşu Home'da uygulamadan çıkar, Play'de "Çık" onayı sorar, Wardrobe'da Home'a döner.
- [ ] **Senaryo: Yerelleştirme** · Verilen TR ve EN tabloları · Eğer cihaz dili TR ise · O zaman tüm metinler TR, aksi halde EN; dil Ayarlar'dan değiştirilebilir; proje ve giysi adları tabloda anahtarlıdır (`garment.scarf`, `garment.beanie`).
- [ ] **Senaryo: Build** · Verilen Android build ayarları · Eğer `release` build alınırsa · O zaman paket adı `com.mtafasahin.yumak`, min SDK 24, 64-bit, boyut < 70 MB (örgü sprite atlası dahil).
- [ ] **Senaryo: Remote Config** · Verilen UGS Remote Config · Eğer `maturity_hold_ms` 1500'den 2000'e çekilirse · O zaman yeni oturumda değer uygulanır.
- [ ] **Senaryo: Büyük yazı** · Verilen sistem yazı boyutu "büyük" · Eğer Home açılırsa · O zaman Core büyük yazı modu otomatik açılır ve proje kartı metinleri kırpılmaz.

### Teknik görevler (Unity/C#)
- [ ] Repo iskeleti: `Assets/_Game/{Scripts,Prefabs,Art,Audio,Data}`, `.gitignore`, `docs/game-design.md` (bu epic'in kuralları), `docs/analytics.md`, `docs/knit-patterns.md`.
- [ ] `MustyCozyCore` bağımlılığı (UPM git URL, sürüm etiketi ≥ Daily Drop'un kullandığı sürüm): `AdsService`, `IapService`, `UgsBootstrap`, `Localization`, `SettingsScreen`, `BackButtonRouter`, `DailyGift` iskeleti, `CollectionScreen` iskeleti.
- [ ] Sahne yöneticisi: `SceneFlow` (async yükleme, geçiş perdesi: kumaş kapanma), `AppState` (ScriptableObject tabanlı oturum durumu).
- [ ] UGS: Player Accounts anonim giriş, Cloud Save, Analytics, Remote Config projesi ve ortamları (`dev`, `prod`); Leaderboards v1'de yok.
- [ ] Remote Config şeması: `drop_cooldown_ms`, `overflow_grace_ms`, `maturity_hold_ms`, `maturity_min_tier`, `rows_per_tier` (JSON {6:1,7:2,8:4}), `tier_color_mode`, `interstitial_daily_cap`, `interstitial_min_gap_s`, `continue_enabled`, `expand_enabled`.
- [ ] CI: GitHub Actions ile Android debug build (GameCI); artefakt olarak APK.
- [ ] Kod stili ve analiz: `.editorconfig`, Roslyn analizörleri, `Assembly Definition` dosyaları (Game, Game.Editor, Tests).
- [ ] Font: Nunito (TR karakter seti), büyük yazı modu için TextMeshPro auto-size sınırları 16–28 pt.

### Analitik
- `app_open` {first_open: bool, locale: string, build: int, large_text: bool} — her açılışta
- `session_start` {garments_done: int, active_project: string} — Home görünür olduğunda

### Notlar
- Core paketi Daily Drop ile olgunlaşmış olmalı; eksik modül varsa (örn. `CollectionScreen`) önce Core'da story açılır, burada efor L'ye çıkar.
- Karar: Reklam ağı AdMob (Core ile aynı; MAX adaptörü SONRA, CozyCore 33); IAP Unity IAP; makbuz doğrulama Core `PremiumGate`'in tüketilemeyen ürün sürümü.
- Hedef kitle 35+ olduğu için varsayılan HUD yazı boyutu Daily Drop'tan 2 pt büyük (18 pt), dokunma hedefleri ≥ 56 px.

## STORY: Yumak 2: MustyMergePhysics paketini Daily Drop'tan çıkar ve bağla
labels: tema:backend, P1, efor:M
milestone: MVP

**Yumak** epic'inin **2. story'si** (epic #EPIC). Tema: Altyapı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** monorepo `mtafasahin-unity-packages` içinde `com.mtafasahin.mergephysics` paketi (MergePhysics epic'i), Daily Drop repo'su (tüketici PR'ı), Yumak `Play` sahnesi

### Kullanıcı hikâyesi
Bir geliştirici olarak Daily Drop'ta yazdığım bırak-birleştir fizik çekirdeğini (top, birleşme, kap, havuz, taşma tespiti) tek bir pakete çıkarıp hem Daily Drop'un hem Yumak'ın aynı sürümü tüketmesini isterim, böylece fizik hatası tek yerde düzelir ve üçüncü merge oyunu bir günde ayağa kalkar.

### Kabul kriterleri
- [ ] **Senaryo: Paket içeriği** · Verilen `com.mtafasahin.mergephysics` paketi · Eğer incelenirse · O zaman `MergeBody`, `MergeDefinition` (SO), `MergeDefinitionSet`, `MergePool`, `MergeSystem`, `MergeEvent`, `MergeContainer`, `OverflowDetector`, `DropController`, `PhysicsProfile` (SO: gravity, iterations, friction, bounciness, drag eğrisi) ve `IMergeRule` arayüzü vardır; oyuna özgü hiçbir sınıf (DailySequence, ScoreSystem, KnitSystem) yoktur.
- [ ] **Senaryo: Daily Drop regresyonu** · Verilen Daily Drop repo'su paketi tüketiyor · Eğer mevcut Play Mode ve Edit Mode testleri çalıştırılırsa · O zaman hepsi geçer ve aynı seed'le 200 düşüşlük bir koşunun skoru paket öncesiyle aynıdır.
- [ ] **Senaryo: Yumak'ta kurulum** · Verilen Yumak projesi · Eğer paket git URL `#v0.1.0` ile eklenirse · O zaman boş `Play` sahnesinde `Container` + `BallPool` + `MergeSystem` prefab'ı ile 8 kademeli toplar düşer ve birleşir (henüz yün görseli yok).
- [ ] **Senaryo: Birleşme kuralı genişletme** · Verilen `IMergeRule` uygulaması · Eğer `CanMerge(a, b)` false dönerse · O zaman toplar birleşmez; `OnMerged(a, b, result)` çağrısında `result`'a oyun tarafı bileşen (renk) atanabilir.
- [ ] **Senaryo: Fizik dışı çıkarma** · Verilen sepetteki bir top · Eğer `MergeSystem.Detach(ball)` çağrılırsa · O zaman top fizik ve taşma tespitinden çıkar, havuza dönmez (oyun tarafı uçuş animasyonuna verir), kalan toplar yerleşir.
- [ ] **Senaryo: Sürümleme** · Verilen paketin `package.json` · Eğer kırıcı değişiklik yapılırsa · O zaman minor sürüm artar ve her iki oyun CHANGELOG'da hangi sürümü tükettiğini yazar.
- [ ] **Senaryo: Havuz** · Verilen 60 top · Eğer sürekli birleşme ve `Detach` olursa · O zaman `Instantiate/Destroy` yok, GC alloc 0 (Profiler).

### Teknik görevler (Unity/C#)
- [ ] Paket monorepo'da: `com.mtafasahin.mergephysics` (`package.json` 0.1.0, `Runtime/`, `Editor/`, `Tests/`, asmdef `Musty.MergePhysics`, README) — MergePhysics 1 story'si; bu story paketi tüketir, yazmaz.
- [ ] Daily Drop'tan taşıma MergePhysics 2–10 story'lerinde yapılır (`Ball → MergeBody`, `BallDefinition → MergeDefinition`, `BallPool → MergePool`, `Container → MergeContainer`, `MergeSystem`, `OverflowDetector`, `PhysicsProfile`); bu story o epic'in v0.1.0 tag'ini bekler.
- [ ] Pakette hazır: `IMergeRule { bool CanMerge(MergeBody a, MergeBody b); void OnMerged(MergeBody a, MergeBody b, MergeBody result); }` ve varsayılan `SameTierRule`; Yumak `ColorMatchRule` yazar (4. story).
- [ ] Pakette hazır: `MergeSystem.Detach(MergeBody)` ve `MergePool.Release(MergeBody)` ayrımı; `MergeBodyState` {Pooled, Held, Active, Merged, Detached} (MergePhysics 4).
- [ ] Pakette hazır: `MergeContainer.SetInnerWidth(float)` (13. story sepet genişletme; MergePhysics 5).
- [ ] Daily Drop PR'ı MergePhysics 10 story'sinde (o epic'te); burada yalnızca Yumak PR'ı.
- [ ] Yumak PR: paket bağımlılığı, `Play` sahnesinde iskelet prefab'ı, `PhysicsProfile_Yumak` (yün için friction 0.45, bounciness 0.05 — Daily Drop'tan daha "yumuşak").
- [ ] Yumak tarafı testi: paket tag'i ile `Play` iskeletinde 8 kademe birleşme (duman); paketin kendi testleri MergePhysics epic'inde.

### Analitik
- Yok (paket oyun olaylarını bilmez; `MergeEvent` oyun tarafında dinlenir).

### Notlar
- Karar (2026-10-14): Paketi çıkarma işi bu story'den **MergePhysics epic'ine** (monorepo) taşındı; bu story yalnızca Yumak'ı pakete bağlar ve profili ayarlar, efor L → M. Paket Daily Drop'un soft launch build'i alındıktan sonra çıkarılır; Daily Drop iterasyonunda fizik değeri değişirse yalnızca `PhysicsProfile` SO değişir, kod değişmez.
- Risk: `OverflowDetector`'ın "durağan" eşiği (0,05) ile Yumak'ın olgunlaşma eşiği aynı; ikisi de `PhysicsProfile.restVelocity`'den okunur, iki yerde sabit olmaz.
- Üçüncü tüketici adayı (Eğik Kavanoz) için `Container` eğim parametresi şimdiden `PhysicsProfile`'a değil ileride ayrı bir `ContainerShape` SO'ya girer; şimdi yapılmaz.

## STORY: Yumak 3: Sepet, taşma çizgisi ve tur bitişi tespiti
labels: tema:cekirdek, P1, efor:S
milestone: MVP

**Yumak** epic'inin **3. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** S · **Milestone:** MVP · **Dokunduğu yer:** `Play` sahnesi, `Basket` prefab (paket `Container` üstüne), `OverflowDetector` (paket), `RoundState`

### Kullanıcı hikâyesi
Bir oyuncu olarak yumakların içine düştüğü hasır sepeti, üstteki taşma çizgisini ve taşınca turun bittiğini (ama örgümün bitmediğini) net görmek isterim, böylece ne zaman risk aldığımı bilirim ve taşma beni korkutmaz.

### Kabul kriterleri
- [ ] **Senaryo: Sepet ölçüsü** · Verilen 390×844 referans ekran · Eğer Play sahnesi açılırsa · O zaman sepet iç genişliği ekranın %88'i, yüksekliği %50'si; üst %26'sı örgü panosuna (8. story) ayrılır; sepet görseli hasır dokulu 9-slice, kenarlık 12 px.
- [ ] **Senaryo: Taşma çizgisi** · Verilen sepet · Eğer tur başlarsa · O zaman sepetin üst kenarının 48 px altında kesikli çizgi ve sağında "taşma" etiketi görünür; çizgi rengi sepetle 3:1 kontrastta.
- [ ] **Senaryo: Taşma tespiti** · Verilen durağan bir yumak · Eğer yumağın merkezi çizginin üstünde `overflow_grace_ms` (1.200) boyunca kalırsa · O zaman `RoundState.Overflowed` olur, girdi kilitlenir ve 12. story'nin tur sonu akışı başlar.
- [ ] **Senaryo: Geçici taşma** · Verilen yeni düşen bir yumak · Eğer sıçrayıp 1,2 sn içinde çizginin altına inerse · O zaman tur bitmez.
- [ ] **Senaryo: Olgun yumak çizgide** · Verilen çizginin üstünde duran 6. kademe yumak · Eğer olgunlaşma süresi (1,5 sn) taşma süresinden (1,2 sn) uzunsa · O zaman taşma önce tetiklenir; ancak 12. story Devam akışında bu yumak "sepetten kaldırılan" değil "panoya giden" olarak işlenir (oyuncu lehine).
- [ ] **Senaryo: Taşma uyarısı** · Verilen bir yumak çizginin 60 px altında · Eğer 0,5 sn durursa · O zaman çizgi turuncuya döner ve sepet kenarı hafif "nefes alır" (Reduce Motion açıkken yalnızca renk).
- [ ] **Senaryo: Uygulama arka plana** · Verilen devam eden tur · Eğer uygulama arka plana alınırsa · O zaman fizik durur, dönünce kaldığı yerden sürer; olgunlaşma ve taşma zamanlayıcıları da durur.

### Teknik görevler (Unity/C#)
- [ ] `Basket` prefab: paket `Container` + hasır sprite (9-slice), `OverflowLine` etiketi, taşma uyarı rengi; `SafeArea` ile HUD kayması.
- [ ] `RoundState` enum {Idle, Playing, Overflowed, Continuing, Finished} ve `RoundController` (girdi kilidi, zamanlayıcı durdurma, 12. story'ye olay `RoundOverflowed`).
- [ ] `PhysicsProfile_Yumak`: `Physics2D.gravity = (0, -20)`, `fixedDeltaTime 1/60`, `velocityIterations 8`, `positionIterations 3`; katmanlar Ball, Wall, Detached.
- [ ] `OverflowDetector` paket bileşeni bağlanır; `overflow_grace_ms` Remote Config'ten `PhysicsProfile`'a değil `RoundConfig` SO'ya yazılır.
- [ ] Pause: `OnApplicationPause` ile `Time.timeScale = 0`, zamanlayıcılar `Time.fixedTime` tabanlı olduğu için otomatik durur; `OverflowDetector` ve `MaturityWatcher` (7. story) `PauseScope` ile duraklatılır.
- [ ] Edit Mode testi: taşma öncelik senaryosu (olgun yumak çizgide) için `RoundController` birim testi.

### Analitik
- `round_overflow` {round_n: int, drop_count: int, round_score: int, rows_this_round: int, duration_s: int, continue_available: bool} — taşma tespit edildiğinde

### Notlar
- Yerçekimi Daily Drop'tan (−22) hafif düşük (−20): yün daha yavaş düşer, cozy tempo; Play modunda ayarlanır, Remote Config'ten gelir.
- Sepetin alt köşeleri 36 px yuvarlatılmış (hasır sepet silueti): paket `Container`'ın 6 segmentli `EdgeCollider2D` tabanı kullanılır.

## STORY: Yumak 4: 8 kademeli yumak seti, renk ve birleşme kuralı
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Yumak** epic'inin **4. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `YarnDefinition` (SO, paket `BallDefinition` türevi), `YarnBall` bileşeni, `YarnMergeRule : IMergeRule`, `ColorPalette` (SO)

### Kullanıcı hikâyesi
Bir oyuncu olarak aynı boydaki iki yumağın değince bir üst boya dönüştüğünü, rengin ise bıraktığım yumaktan geldiğini görmek isterim, böylece hem yer açma hem de örgünün rengini seçme stratejisi kurabilirim.

### Kabul kriterleri
- [ ] **Senaryo: Kademe tablosu** · Verilen `YarnDefinition` listesi · Eğer oyun yüklenirse · O zaman 8 kademe vardır; yarıçaplar (px, 390 referans): 14, 18, 24, 30, 38, 46, 56, 68; her kademede iç halka sayısı 1–8 (gölgeli sarım halkaları, tasarım taslağındaki iç halka gölgesi); adlar TR/EN (İplik, Düğüm, Topak, Yumak, Yumacık, Koca Yumak, Dev Yumak, Yün Dağı).
- [ ] **Senaryo: Renk paleti** · Verilen aktif projenin paleti (4 renk; Atkı: kiremit, krem, lacivert, hardal) · Eğer yumak üretilirse · O zaman rengi paletten gelir; kademe rengi belirlemez (`tier_color_mode = free`).
- [ ] **Senaryo: Birleşme ve renk** · Verilen aynı kademeden iki yumak · Eğer temas ederlerse · O zaman ikisi yok olur ve temas noktasında bir üst kademe yumak oluşur; rengi, temas anında hızı daha yüksek olan (düşen) yumağın rengidir; hızlar eşitse son bırakılanın rengi.
- [ ] **Senaryo: Farklı renk birleşir** · Verilen aynı kademe, farklı renk iki yumak · Eğer temas ederlerse · O zaman birleşirler (renk birleşmeyi engellemez).
- [ ] **Senaryo: Üçlü temas** · Verilen aynı kademeden üç yumak aynı anda temas · Eğer birleşme tetiklenirse · O zaman yalnızca bir çift birleşir, üçüncü kalır.
- [ ] **Senaryo: 8. kademe** · Verilen iki 8. kademe yumak · Eğer temas ederlerse · O zaman birleşmezler (yok olma yok); her biri kendi olgunlaşma zamanlayıcısıyla panoya gider (7. story).
- [ ] **Senaryo: Kademe okunurluğu** · Verilen aynı renkte 4. ve 5. kademe yan yana · Eğer ekran görüntüsü 5 kullanıcıya gösterilirse · O zaman 5/5 hangisinin büyük olduğunu söyler (boyut farkı ≥ %25 + halka sayısı); Ayarlar'da "Kademe numarası göster" açılınca yumağın ortasında rakam.
- [ ] **Senaryo: Geri dönüş modu** · Verilen Remote Config `tier_color_mode = tier` · Eğer oyun yüklenirse · O zaman renk kademeye bağlanır (Daily Drop davranışı) ve desen tek renk üretilir.

### Teknik görevler (Unity/C#)
- [ ] `YarnDefinition : BallDefinition` (paket SO'yu genişletir): ringCount, nameKey, maturityRows (6:1, 7:2, 8:4; Remote Config `rows_per_tier` üzerine yazar).
- [ ] `YarnBall` (MonoBehaviour, `Ball`'un yanında): `ColorId`, `SpriteRenderer` tint + iç halka overlay sprite (kademe başına 1 sprite, 8 sprite'lık atlas), "Kademe numarası" overlay.
- [ ] `YarnMergeRule : IMergeRule`: `CanMerge` = aynı tier ve tier < 8; `OnMerged` = `result.ColorId` = hızı yüksek olanın rengi; `MergeEvent`'e `colorId` eklenir (paket `MergeEvent.Payload` sözlüğü).
- [ ] `ColorPalette` SO: 4 renk + ad anahtarı + HEX; proje şablonu (9. story) paleti taşır; `PaletteService.Active`.
- [ ] `YarnPool`: paket `BallPool` + `YarnBall` ön ısıtma kademe başına 12; renk ataması `Spawn(tier, colorId)`.
- [ ] Fizik materyali: friction 0.45, bounciness 0.05; `linearDrag` kademeyle 0.25 → 0.7 (yün "oturur").
- [ ] Play Mode testi: farklı renkli aynı kademe iki yumağın 1 sn içinde tek yumağa dönüştüğünü ve rengin düşen yumaktan geldiğini doğrulayan test.

### Analitik
- `merge` {tier: int, color: string, chain: int, drop_count: int} — her birleşmede (tier ≥ 5 için; küçükler sayılıp `round_end`'de toplam olarak gider)

### Notlar
- Karar: renk kademeden bağımsız. Örgü deseninin "oyuncunun eseri" olması bu karara bağlı; kademe okunurluğu riski halka sayısı ve numarayla karşılanır. Soft launch'ta `tier_color_mode` A/B.
- Sanat: düz renk + iç halka gölgesi + tek vurgu (tasarım taslağı); 8. kademe "Yün Dağı" hafif parıltılı.

## STORY: Yumak 5: Düşürme kontrolü, sıradaki önizleme ve renk sırası
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Yumak** epic'inin **5. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `DropController`, `InputRouter`, `NextPreview` HUD, `YarnSequence`

### Kullanıcı hikâyesi
Bir oyuncu olarak parmağımı sürükleyip bıraktığım yere yumağın düşmesini ve sıradaki üç yumağın boyunu ve rengini görmeyi isterim, böylece hem sepeti hem örgünün rengini planlayabilirim.

### Kabul kriterleri
- [ ] **Senaryo: Sürükleme** · Verilen elde tutulan yumak · Eğer parmak yatay sürüklenirse · O zaman yumak sepetin iç kenarları arasında (yarıçap payıyla) parmağı izler; dikey hareket yok sayılır; parmak yumağı kapatmasın diye yumak parmağın 80 px üstünde durur.
- [ ] **Senaryo: Bırakma** · Verilen sürüklenen yumak · Eğer parmak kaldırılırsa · O zaman yumak o x konumundan düşer; noktalı kılavuz çizgisi bırakma anında kaybolur.
- [ ] **Senaryo: Bekleme** · Verilen yeni bırakılmış yumak · Eğer `drop_cooldown_ms` (450) dolmadan dokunulursa · O zaman yeni yumak gelmez; dolunca sıradaki yumak elde belirir.
- [ ] **Senaryo: Önizleme** · Verilen tur sırası · Eğer tur sürüyorsa · O zaman HUD "Sıradaki" alanında sonraki 3 yumak kademe boyutu ve rengiyle, soluklaşarak görünür ve her düşüşte kayar.
- [ ] **Senaryo: Sıra üretimi** · Verilen 10.000 elemanlı sıra · Eğer sayılırsa · O zaman kademe oranları {1: 30, 2: 25, 3: 20, 4: 15, 5: 10} yüzde ± 2; renkler palette eşit dağılır ± 3; aynı renk art arda en fazla 2 kez.
- [ ] **Senaryo: Dokunma dışı** · Verilen HUD düğmesine (Dolap, Sepeti genişlet) dokunma · Eğer parmak düğme üzerindeyse · O zaman yumak sürüklenmez (UI raycast önceliği).
- [ ] **Senaryo: Tek el ve yavaş parmak** · Verilen 6,7" cihaz, senior kullanıcı · Eğer parmak 0,8 sn hareketsiz sonra kaldırılırsa · O zaman bu bir "bırakma"dır (uzun basma olarak yorumlanmaz); dokunma alanı tüm sepet genişliği, yükseklik sınırsız.

### Teknik görevler (Unity/C#)
- [ ] `InputRouter`: Unity Input System, `Pointer` aksiyonları; `EventSystem.IsPointerOverGameObject` ile UI önceliği; uzun basma yok.
- [ ] `DropController`: durum makinesi {Waiting, Holding, Dropped}, cooldown, elde yumak (kinematic) → bırakınca dynamic; parmak offset 80 px (Ayarlar'da 0/80/120).
- [ ] `YarnSequence`: xoshiro256** PRNG (Daily Drop'tan kopya, seed = `Guid` tur başı; deterministik gün yok), ağırlıklı kademe + palet renk seçimi, "art arda ≤ 2" kısıtı, `Peek(3)`, `Next()`.
- [ ] Kılavuz çizgi: `LineRenderer` noktalı materyal, yumağın renginde; Reduce Motion'da düz ince çizgi.
- [ ] `NextPreview`: 3 slot, `YarnDefinition.sprite` + tint; büyük yazı modunda slot boyutu +%20.
- [ ] Haptik: bırakmada hafif (Core `Haptics.Light`).
- [ ] Edit Mode testleri: dağılım, renk tekrar kısıtı.

### Analitik
- `drop` {n: int, tier: int, color: string, x_norm: float} — her bırakmada (yalnızca ilk 10 düşüş ve her 10'da bir)

### Notlar
- Cooldown, parmak offset ve kılavuz görünürlüğü Remote Config / Ayarlar.
- Karar: Daily Drop'taki "nefes" animasyonu burada da var (1,0 → 1,06 → 1,0, 120 ms) ama ses yün "puf".

## STORY: Yumak 6: Puanlama, zincir ve his: yün ASMR, haptik, şiş sesi
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Yumak** epic'inin **6. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `ScoreSystem`, `RoundStats`, `MergeFx`, `AudioDirector`, `Haptics`

### Kullanıcı hikâyesi
Bir oyuncu olarak her birleşmenin yumuşak bir yün sesi ve hafif titreşimle "oturduğunu", büyük birleşmelerin daha çok puan getirdiğini hissetmek isterim, böylece oyun sakin ama tatmin edici olsun.

### Kabul kriterleri
- [ ] **Senaryo: Temel puan** · Verilen kademe t birleşmesi · Eğer birleşme olursa · O zaman tur puanı `10 × t²` eklenir (t=1→2: 10 … t=7→8: 490); panoya giden yumak kademesine göre ek 100/250/600 (6/7/8).
- [ ] **Senaryo: Zincir** · Verilen bir düşüşün tetiklediği ardışık birleşmeler · Eğer ikinci ve sonraki birleşmeler 0,8 sn içinde gelirse · O zaman çarpan ×1,5, ×2, ×3 (üst sınır) ve HUD'da "Zincir ×2" rozeti.
- [ ] **Senaryo: Birleşme efekti** · Verilen kademe t birleşmesi · Eğer yeni yumak doğarsa · O zaman 0 → 1,12 → 1,0 ölçek (180 ms), temas noktasında 4–8 yün lifi parçacığı (t ile artar), ekran sarsıntısı yok (cozy).
- [ ] **Senaryo: Ses ailesi** · Verilen birleşme · Eğer ses çalarsa · O zaman yumuşak kumaş sürtünme + pamuk "puf"; zincirde perde yarım ton yükselir (en fazla 6 adım); taşma uyarısı düşük, nazik vızıltı (korkutucu değil).
- [ ] **Senaryo: Şiş sesi** · Verilen panoya satır eklenmesi (7. story olayı) · Eğer satır dolarsa · O zaman her satır için "tık-tık" şiş sesi (satır başına 1, en fazla 4 ardışık, 120 ms aralık).
- [ ] **Senaryo: Haptik** · Verilen cihaz haptik destekli · Eğer kademe ≥ 5 birleşirse · O zaman Light; satır eklenince Medium; giysi tamamlanınca Heavy; Ayarlar'da kapatılabilir.
- [ ] **Senaryo: Sessiz cihaz ve Reduce Motion** · Verilen telefon sessizde / Reduce Motion açık · Eğer oynanırsa · O zaman efektler görsel/haptik olarak eksiksiz; Reduce Motion'da parçacık yok, yalnızca ölçek ve renk.

### Teknik görevler (Unity/C#)
- [ ] `ScoreSystem`: `MergeEvent` ve `YarnMatured` (7. story) dinler, zincir zamanlayıcı, int aritmetik; `ScoreTable` SO.
- [ ] `RoundStats` struct {roundScore, dropCount, maxTier, chainMax, rowsAdded, durationS, continued, expanded} + `RoundStatsRecorder`.
- [ ] HUD: `ScoreView` (sayaç tween 250 ms, pop %105), `ChainBadge`; skor örgü panosunun sağ altında küçük (odak örgüde).
- [ ] `MergeFx`: lif parçacığı prefab (havuzlu), ölçek tween; `Reduce Motion` dalı.
- [ ] `AudioDirector`: klipler — 1 birleşme (3 varyant), 1 büyük birleşme, 1 bırakma, 1 olgunlaşma "hımm", 1 uçuş "vuş", 1 şiş tık-tık, 1 taşma uyarısı, 1 tur sonu, 1 giysi tamamlama (makas + kumaş); `pitch = 2^(step/12)`; ambient döngü 1 (lisanslı/özgün, akustik gitar + yağmur).
- [ ] `Haptics` (Core) sarmalayıcısı; haptik ayarı Core `SettingsScreen`.
- [ ] Efekt ilkesi: yoğunluk artsın, süre uzamasın (zincirleme efektler sırayla ≤ 200 ms); ekran sarsıntısı bu oyunda yok.
- [ ] Edit Mode testi: puan tablosu ve zincir çarpanı.

### Analitik
- `round_end` parametrelerine ekle (12. story): {max_tier: int, chain_max: int, merges_total: int}

### Notlar
- Puan ikincil: bu oyunda ilerleme satır ve giysidir; skor yalnızca tur içi geri bildirim ve ileride haftalık proje (18. story) için.
- Ses kimliği: ASMR yün; MustyBlockBlast ve Daily Drop seslerinden tamamen ayrı set.

## STORY: Yumak 7: Olgunlaşma kuralı ve sepetten panoya uçuş
labels: tema:orgu, P1, efor:M
milestone: MVP

**Yumak** epic'inin **7. story'si** (epic #EPIC). Tema: Örgü.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `MaturityWatcher`, `MaturityLabel`, `FlightAnimator`, `KnitSystem.AddRows` arayüzü, paket `MergeSystem.Detach`

### Kullanıcı hikâyesi
Bir oyuncu olarak yeterince büyüyen bir yumağın "olgun" olduğunu görmek, sepetten yukarı uçup örgüme satır eklediğini izlemek isterim, böylece sepetteki her büyük birleşme somut bir ürüne dönüşsün ve sepette yer açılsın.

### Kabul kriterleri
- [ ] **Senaryo: Olgunlaşma** · Verilen kademe ≥ `maturity_min_tier` (6) bir yumak · Eğer hızı `maturity_hold_ms` (1.500) boyunca kesintisiz < 0,05 kalırsa · O zaman yumak "olgun" işaretlenir, üstünde "olgun · örgüye gidiyor" etiketi 0,5 sn belirir ve haptik Light.
- [ ] **Senaryo: Zamanlayıcı sıfırlama** · Verilen olgunlaşmakta olan yumak · Eğer 1,5 sn dolmadan başka bir yumak çarpıp hızını 0,05 üstüne çıkarırsa · O zaman zamanlayıcı sıfırlanır; yumak üstündeki ilerleme halkası (ince, yumak çevresinde 0 → 360°) da sıfırlanır.
- [ ] **Senaryo: Uçuş** · Verilen olgun yumak · Eğer etiket süresi dolarsa · O zaman yumak fizikten çıkar (`Detach`), sepetten panodaki hedef satır konumuna kübik bezier eğrisiyle 600 ms'de uçar (kontrol noktası sepet üstü +180 px), uçarken %100 → %40 küçülür, varışta `KnitSystem.AddRows(colorId, n)` çağrılır (6→1, 7→2, 8→4).
- [ ] **Senaryo: Sepet yerleşmesi** · Verilen uçan yumağın altındaki yumaklar · Eğer yumak ayrılırsa · O zaman kalan yumaklar fizikle yerleşir; taşma çizgisi üstündeki bir yumak bu yerleşmeyle inerse taşma zamanlayıcısı sıfırlanır.
- [ ] **Senaryo: Birden fazla olgun** · Verilen aynı anda olgunlaşan 2 yumak · Eğer ikisi de süreyi doldurursa · O zaman uçuşlar 150 ms arayla sıralanır; satırlar varış sırasıyla eklenir.
- [ ] **Senaryo: Tur sonunda uçuş** · Verilen uçmakta olan yumak · Eğer o anda taşma tespit edilirse · O zaman uçuş tamamlanır ve satırlar eklenir, sonra tur sonu akışı başlar (satır kaybı yok).
- [ ] **Senaryo: Reduce Motion** · Verilen Reduce Motion açık · Eğer yumak olgunlaşırsa · O zaman uçuş yerine 200 ms solma + panoda belirme; satır ekleme aynı.
- [ ] **Senaryo: Arka plan** · Verilen olgunlaşma sayacı 1,0 sn'de · Eğer uygulama arka plana alınıp dönülürse · O zaman sayaç 1,0 sn'den sürer (duvar saati değil, fizik zamanı).

### Teknik görevler (Unity/C#)
- [ ] `MaturityWatcher` (FixedUpdate): aktif `YarnBall` listesi üzerinde tier ≥ `maturity_min_tier` ve `Rigidbody2D.velocity.magnitude < PhysicsProfile.restVelocity` kontrolü; yumak başına `holdTimer`; eşik dolunca `YarnMatured {ball, tier, colorId}` olayı; `PauseScope` ile duraklar.
- [ ] `MaturityLabel`: dünya uzayında TMP etiket + ilerleme halkası (`Image.fillAmount`); havuzlu, Reduce Motion'da halka yok.
- [ ] `FlightAnimator`: `MergeSystem.Detach(ball)` → katman `Detached`, collider kapalı, kübik bezier (kendi `Tween` yardımcı sınıfı; DOTween bağımlılığı yok), varışta `KnitSystem.AddRows(colorId, rowsForTier)` ve `YarnPool.Release(ball)`; kuyruk (150 ms aralık).
- [ ] Hedef konum: `KnitBoardView.NextRowAnchor()` (8. story); pano henüz yoksa pano merkezi.
- [ ] `RoundController`: `RoundOverflowed` geldiğinde `FlightAnimator.PendingCount > 0` ise tur sonu akışı bekletilir (`WaitForFlights`), üst sınır 1,2 sn.
- [ ] Remote Config: `maturity_min_tier`, `maturity_hold_ms`, `rows_per_tier`, `flight_ms`.
- [ ] Play Mode testi: 6. kademe yumak sepete kinematic yerleştirilir, 1,5 sn sonra `YarnMatured` tetiklenir ve `KnitSystem.AddRows(…, 1)` çağrılır (mock `IKnitSystem`).
- [ ] Edit Mode testi: hız eşiği aşımında zamanlayıcı sıfırlanması.

### Analitik
- `yarn_matured` {tier: int, color: string, rows: int, hold_resets: int, drop_count: int} — her olgunlaşmada
- `rows_added` {rows: int, total_rows: int, project: string} — her `AddRows` çağrısında

### Notlar
- Karar: olgunlaşma süresi (1,5 sn) taşma süresinden (1,2 sn) uzun: çizgide duran olgun yumak oyuncuyu kurtarmaz; "önce yerleştir, sonra olgunlaşsın" gerilimi. 12. story'deki Devam akışı bu yumağı panoya gönderir (lehine).
- Açık soru: 6. kademe çok sık gelirse sepet asla dolmaz. Soft launch'ta `maturity_min_tier = 7` kolu A/B.
- Uçuş sırasında yumak sepetle çarpışmaz; panoya varana kadar "görsel" nesnedir.

## STORY: Yumak 8: Örgü panosu ve prosedürel ilmek satırı render'ı
labels: tema:orgu, P1, efor:L
milestone: MVP

**Yumak** epic'inin **8. story'si** (epic #EPIC). Tema: Örgü.

**Öncelik:** P1 · **Efor:** L · **Milestone:** MVP · **Dokunduğu yer:** `KnitSystem`, `KnitBoardView`, `StitchGrid`, `PatternRule`, `GarmentTemplate` (SO, 9. story ile ortak)

### Kullanıcı hikâyesi
Bir oyuncu olarak ekranın üstünde ördüğüm giysinin satır satır, bıraktığım yumakların renginde dolduğunu görmek isterim, böylece "ben ördüm" hissi olsun ve bir sonraki turda hangi rengi istediğimi bilerek oynayayım.

### Kabul kriterleri
- [ ] **Senaryo: Pano düzeni** · Verilen Play sahnesi · Eğer açılırsa · O zaman ekranın üst %26'sında örgü panosu: solda giysi silueti içinde ilmek grid'i, sağda "Atkı · 6/10 satır" metni ve 10 segmentli ilerleme çubuğu (tasarım taslağı: üstte 10 sıralık atkı ilerlemesi, 6 örülü).
- [ ] **Senaryo: Satır ekleme** · Verilen `AddRows(kiremit, 2)` · Eğer çağrılırsa · O zaman 2 yeni satır, her biri 10 ilmek, 80 ms arayla soldan sağa "örülerek" belirir (ilmek başına 8 ms); satır rengi kiremit; şiş sesi her satırda.
- [ ] **Senaryo: İlmek görseli** · Verilen satır · Eğer çizilirse · O zaman her ilmek "V" şeklinde sprite (24×20 px, 390 referans), renk tint = yumak rengi, hafif gölge; ardışık satırlar 2 px üst üste biner (gerçek örgü dokusu).
- [ ] **Senaryo: Desen kuralı: düz** · Verilen ardışık 3 satır aynı renk · Eğer render edilirse · O zaman ilmek sütunları hizalı (kayma 0).
- [ ] **Senaryo: Desen kuralı: çizgili** · Verilen satır rengi bir öncekinden farklı · Eğer render edilirse · O zaman satır sınırında ince (1 px) koyu ayırıcı çizgi ve renk geçişi; desen adı HUD'da "çizgili" olarak görünür.
- [ ] **Senaryo: Desen kuralı: çapraz** · Verilen palette 4 rengin tamamı son 4 satırda sırayla kullanıldı · Eğer render edilirse · O zaman o 4 satırda her satır 1 ilmek sağa kayar (çapraz görünüm) ve HUD'da "çapraz" rozeti; oyuncuya "Desen: çapraz" 1 sn balonu.
- [ ] **Senaryo: Siluet** · Verilen Bere şablonu (14 satır) · Eğer grid dolarsa · O zaman satırlar siluet maskesi içinde kırpılır (üst satırlar daralır); taşan ilmek çizilmez.
- [ ] **Senaryo: Performans** · Verilen 30 satır × 10 ilmek (Kazak) · Eğer çizilirse · O zaman tek `SpriteRenderer` batch (atlas) veya tek `Mesh`; draw call ≤ 3, GC alloc 0 satır eklerken.

### Teknik görevler (Unity/C#)
- [ ] `KnitSystem` (`IKnitSystem`): `AddRows(colorId, n)`, `Rows: IReadOnlyList<KnitRow {colorId, offset}>`, `Progress` (rows/target), olaylar `RowAdded`, `ProjectCompleted`; kalıcılık 9. story.
- [ ] `PatternRule`: statik sınıf; `Classify(rows)` → {Plain, Striped, Diagonal}; çapraz kuralı: son 4 satır paletteki 4 farklı renk; kayma `offset = rowIndex % 4` yalnızca çapraz bloğunda; `docs/knit-patterns.md`'de kurallar.
- [ ] `StitchGrid`: `Mesh` tabanlı render (her ilmek 1 quad, UV "V" sprite'ına, vertex color = renk); `AddRow` quad ekler, mesh yeniden gönderir; alternatif `SpriteRenderer` havuzu (performans testine göre karar).
- [ ] `KnitBoardView`: siluet maskesi (`SpriteMask`, şablondan), ilerleme çubuğu, proje adı, `NextRowAnchor()` (7. story uçuş hedefi), satır örme animasyonu (ilmek başına 8 ms, Reduce Motion'da anında).
- [ ] `GarmentTemplate` SO (9. story ile ortak): id, nameKey, rowTarget, stitchesPerRow (10), silhouetteMask sprite, palette (`ColorPalette`), seasonId, wardrobeSprite, avatarSprite, previewSprite.
- [ ] HUD büyük yazı: "6/10" metni ≥ 18 pt, çubuk segmentleri ≥ 8 px.
- [ ] Edit Mode testleri: `PatternRule.Classify` (düz/çizgili/çapraz), siluet kırpma satır sayısı.

### Analitik
- `pattern_detected` {pattern: string, project: string, row_index: int} — desen sınıfı değiştiğinde (düz → çizgili vb.)

### Notlar
- Karar: v1'de "desen" oyuncunun bilinçli seçimi değil, renk sırasının sonucudur; 17. story'deki desen paketleri yalnızca ilmek sprite'ı ve palet değiştirir, kuralları değil.
- Risk: mesh yaklaşımı Kazak'ta (300 ilmek) bile ucuz; ama örme animasyonu için vertex color güncellemesi kare başına ≤ 1 mesh upload.
- Tasarım taslağı: atkı silueti dikdörtgen; bere yukarı daralan; çorap L biçimli — maskeler 9. story'de üretilir.

## STORY: Yumak 9: Giysi şablonları, proje sırası ve ilerleme kalıcılığı
labels: tema:orgu, P1, efor:M
milestone: MVP

**Yumak** epic'inin **9. story'si** (epic #EPIC). Tema: Örgü.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `GarmentTemplate` (SO) seti, `ProjectQueue`, `KnitProgress` (Cloud Save), Home proje kartı

### Kullanıcı hikâyesi
Bir oyuncu olarak hangi giysiyi ördüğümü, kaç satır kaldığını ve sonraki projeyi görmek; uygulamayı kapatsam, telefon değiştirsem bile satırlarımın yerinde durmasını isterim, böylece "yarım kalan örgü" beni geri getirsin.

### Kabul kriterleri
- [ ] **Senaryo: Sezon 1 listesi** · Verilen `GarmentTemplate` seti · Eğer oyun yüklenirse · O zaman sezon 1'de 10 giysi sırayla: Atkı 10, Bere 14, Eldiven 16, Çorap 18, Yelek 20, Hırka 24, Şal 22, Patik 16, Kazak 30, Battaniye 36 satır; her birinin paleti (4 renk) ve siluet maskesi vardır.
- [ ] **Senaryo: Sıra** · Verilen Atkı tamamlandı · Eğer bir sonraki tur başlarsa · O zaman aktif proje Bere'dir, pano boş, palet Bere'nin paleti; HUD'da "Sıradaki: Eldiven".
- [ ] **Senaryo: Kalıcılık** · Verilen Atkı 6/10, tur yarıda · Eğer uygulama kapatılıp 2 saat sonra açılırsa · O zaman Home kartı "Atkı · 6/10 satır" gösterir ve satır renkleri birebir aynıdır; sepet içeriği kaydedilmez (tur yeniden başlar, sepet boş).
- [ ] **Senaryo: Cloud Save** · Verilen yeni cihaz, aynı Player Account · Eğer uygulama açılırsa · O zaman `KnitProgress` ve `WardrobeState` bulutta geri gelir; çakışmada daha çok toplam satırı olan kazanır.
- [ ] **Senaryo: Yazma sıklığı** · Verilen satır ekleme · Eğer `AddRows` tamamlanırsa · O zaman yerel JSON anında, Cloud Save en geç 5 sn sonra (debounce) yazılır; tur sonunda zorunlu yazma.
- [ ] **Senaryo: Sezon sonu** · Verilen Battaniye (sezon 1 son) tamamlandı · Eğer sonraki proje istenirse · O zaman sezon 2 ilk giysisi açılır (sezon 2 ve 3 içerikleri 11. ve 17. story; yoksa sezon 1 yeni paletle tekrar, "2. tur" etiketi).
- [ ] **Senaryo: Remote sıra** · Verilen Remote Config `project_order` · Eğer sıra değiştirilirse · O zaman yalnızca henüz başlanmamış projeler etkilenir.

### Teknik görevler (Unity/C#)
- [ ] 10 `GarmentTemplate` SO + 10 siluet maskesi (PNG, 390 referansta pano boyutu 200×120 px) + 10 dolap sprite'ı + 10 avatar katman sprite'ı (11. story ile paylaşılır); `GarmentCatalog` SO (sezon → liste).
- [ ] `ProjectQueue`: `Active`, `Next`, `Advance()`; sıra `GarmentCatalog` + Remote Config `project_order`.
- [ ] `KnitProgress` modeli: {activeGarmentId, rows: List<{colorId}>, roundsPlayed, totalRowsEver}; `KnitProgressStore` (yerel JSON `persistentDataPath/knit.json` + Cloud Save anahtarı `knit/progress`), debounce 5 sn, `OnApplicationPause` yazma.
- [ ] Çakışma çözümü: `totalRowsEver` büyük olan; eşitse son yazılan.
- [ ] Home proje kartı: giysi önizleme (mini `StitchGrid` veya önceden render'lanmış sprite), "6/10 satır", "Ör" düğmesi, "Sıradaki" satırı.
- [ ] `docs/game-design.md`: proje listesi ve satır hedefleri tablosu.
- [ ] Edit Mode testi: `KnitProgress` serileştirme gidiş-dönüş; çakışma kuralı.

### Analitik
- `project_start` {project: string, season: int, index: int} — proje ilk satırı aldığında
- `home_view` {active_project: string, rows: int, target: int, garments_done: int} — Home her açıldığında

### Notlar
- Karar: sepet içeriği kaydedilmez (tur kısa, sepet geçici; kalıcı olan örgüdür). Bu, Daily Drop'un `BoardSnapshot`'ına göre daha basit ve "ilerleme kalıcı" vaadiyle tutarlı.
- Satır hedefleri başlangıç; ilk giysi (Atkı 10) ilk oturumda bitecek kadar kısa olmalı (kapı: ilk giysi ≥ %50). Soft launch'ta `scarf_rows` 8/10/12 A/B.

## STORY: Yumak 10: Giysi tamamlama anı ve dolaba giriş
labels: tema:orgu, P1, efor:M
milestone: MVP

**Yumak** epic'inin **10. story'si** (epic #EPIC). Tema: Örgü.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `GarmentCompleteFlow`, `GarmentCompleteView` (overlay), `WardrobeState`, Teyze avatar katmanı

### Kullanıcı hikâyesi
Bir oyuncu olarak son satırı ördüğümde giysinin "bittiğini" küçük ama sıcak bir anla görmek, Teyze'nin onu giydiğini ve dolaba girdiğini izlemek isterim, böylece her proje bir ödül gibi hissedilsin ve bir sonrakine başlamak isteyeyim.

### Kabul kriterleri
- [ ] **Senaryo: Tamamlama tetiği** · Verilen Atkı 9/10 · Eğer `AddRows(…, 2)` gelirse · O zaman 10. satır eklenir, fazla 1 satır sonraki projeye devredilmez (kaybolur, bir sonraki projenin ilk satırı olarak eklenmez) ve `ProjectCompleted` tetiklenir; tur **bitmez**, sepet aynen kalır.
- [ ] **Senaryo: Tamamlama anı** · Verilen `ProjectCompleted` · Eğer tetiklenirse · O zaman girdi 2,5 sn kilitlenir (fizik sürer), pano giysiyi "katlar" (siluet 300 ms'de küçülüp Teyze'ye uçar), Teyze giysiyi giyer (katman belirir, 200 ms), "Atkı tamam!" kartı 1,5 sn, makas + kumaş sesi, Heavy haptik, konfeti yerine 12 yün lifi.
- [ ] **Senaryo: Sonraki proje** · Verilen an bitti · Eğer kilit açılırsa · O zaman pano Bere siluetine geçer (boş), palet değişir, HUD "Bere · 0/14", elde tutulan ve sıradaki yumaklar yeni paletten yeniden üretilir; sepettekiler eski renkte kalır.
- [ ] **Senaryo: Dolaba giriş** · Verilen tamamlama · Eğer kart kapanırsa · O zaman `WardrobeState.owned` listesine {garmentId, rows (renkleriyle), pattern, completedAt} eklenir ve HUD "Dolap · 3" sayacı +1 olur (tasarım taslağı: Dolap · 3 düğmesi).
- [ ] **Senaryo: Reduce Motion** · Verilen açık · Eğer tamamlanırsa · O zaman uçuş ve katlama yerine 200 ms solma; kart ve ses aynı; kilit süresi 1,5 sn.
- [ ] **Senaryo: İlk giysi** · Verilen ilk kez tamamlanan giysi · Eğer kart kapanırsa · O zaman bir kez "Dolabını gör" ipucu balonu Dolap düğmesini işaret eder (girdiyi kilitlemez).
- [ ] **Senaryo: Kesinti** · Verilen tamamlama anı sürerken uygulama kapandı · Eğer yeniden açılırsa · O zaman giysi dolapta vardır, sonraki proje aktiftir (yazma kart açılmadan önce yapılır).

### Teknik görevler (Unity/C#)
- [ ] `GarmentCompleteFlow`: `KnitSystem.ProjectCompleted` dinler; sıra: `WardrobeStore.Add` → `KnitProgressStore.Advance` (yazma) → animasyon → kart → `ProjectQueue.Advance()` → `YarnSequence.Repalette()`.
- [ ] `GarmentCompleteView` overlay (uGUI): kart, giysi önizleme (`StitchGrid` snapshot `RenderTexture` → sprite, dolap için de kullanılır), kapanış.
- [ ] `WardrobeState` modeli {owned: List<OwnedGarment>, equippedByLayer: Dictionary<string, garmentId>, seasonUnlocked: int}; `WardrobeStore` (yerel JSON + Cloud Save `wardrobe/state`).
- [ ] Teyze avatarı Play HUD'unda küçük (sağ üst, 64 px): `AvatarView` (11. story) minimal sürümü — gövde + giysi katmanı.
- [ ] Fazla satır kuralı `KnitSystem.AddRows` içinde (`Math.Min`); analitikte `rows_wasted`.
- [ ] Giysi snapshot: 256×256 PNG `persistentDataPath/wardrobe/{id}_{n}.png`, dolap ızgarası bunu okur.
- [ ] Play Mode testi: 9/10 + 2 satır → tamamlanma, dolapta 1 giysi, aktif proje Bere.

### Analitik
- `garment_complete` {project: string, rounds_used: int, rows_wasted: int, pattern: string, total_garments: int, session_n: int} — tamamlanınca
- `first_garment` {minutes_since_install: int, rounds_used: int} — ilk giysi (kapı metriği)

### Notlar
- Kapı: ilk giysiyi tamamlayan oranı ≥ %50; `first_garment` olayının yükleyenlere oranı. Atkı 10 satır = yaklaşık 4–6 olgun yumak = 2–3 tur hedeflenir.
- Karar: tamamlama turu bitirmez; oyuncu sepetiyle devam eder, yeni projeye hemen katkı verir (akış kesilmez).
- Fazla satırın devredilmemesi kasıtlı: 8. kademeyi (4 satır) son 1 satırda harcamak oyuncunun kararı; HUD'da "kalan 1 satır" görünür.

## STORY: Yumak 11: Dolap koleksiyonu, sezonlar ve avatar giydirme
labels: tema:meta, P1, efor:L
milestone: MVP

**Yumak** epic'inin **11. story'si** (epic #EPIC). Tema: Dolap ve avatar.

**Öncelik:** P1 · **Efor:** L · **Milestone:** MVP · **Dokunduğu yer:** `Wardrobe` sahnesi, `WardrobeGrid`, `AvatarView`, `AvatarLayer`, `GarmentCatalog`, Core `CollectionScreen` iskeleti

### Kullanıcı hikâyesi
Bir oyuncu olarak ördüğüm giysileri dolapta görmek, Teyze'ye istediğimi giydirmek ve sezonlarda daha ne örebileceğimi bilmek isterim, böylece koleksiyonu tamamlamak için geri geleyim.

### Kabul kriterleri
- [ ] **Senaryo: Dolap ızgarası** · Verilen 3 giysi tamamlanmış · Eğer Dolap açılırsa · O zaman 3 sezon sekmesi (Sonbahar, Kış, Bahar; 10'ar giysi), aktif sezonda 3 renkli kart (oyuncunun ördüğü renklerle snapshot) + 7 silüet (kilitli, adı ve satır sayısı yazılı); başlık "Dolap · 3/30".
- [ ] **Senaryo: Giysi detayı** · Verilen dolaptaki Atkı · Eğer dokunulursa · O zaman büyük önizleme, desen adı (düz/çizgili/çapraz), tamamlanma tarihi, "Giydir" düğmesi.
- [ ] **Senaryo: Giydirme** · Verilen Atkı detayında "Giydir" · Eğer basılırsa · O zaman Teyze'nin `neck` katmanına atkı sprite'ı, tint = atkının baskın rengi, 200 ms belirme; aynı katmandaki önceki giysi çıkar; `WardrobeState.equippedByLayer` güncellenir ve Cloud Save'e yazılır.
- [ ] **Senaryo: Katmanlar** · Verilen giysi türleri · Eğer giydirilirse · O zaman katman eşlemesi: Atkı/Şal → neck, Bere → head, Eldiven → hands, Çorap/Patik → feet, Yelek/Hırka/Kazak → torso, Battaniye → lap; her katmanda tek giysi, 6 katman aynı anda dolu olabilir.
- [ ] **Senaryo: Aynı giysi iki kez** · Verilen Atkı ikinci kez örüldü (sezon tekrarı) · Eğer dolaba girerse · O zaman ayrı kart (farklı renkler), sayaç "Dolap · 4/30" değil "3/30 (+1 tekrar)".
- [ ] **Senaryo: Home avatarı** · Verilen giydirilmiş Teyze · Eğer Home açılırsa · O zaman avatar giysileriyle görünür (240 px), dokununca Dolap açılır.
- [ ] **Senaryo: Sezon kilidi** · Verilen sezon 1'de 10/10 · Eğer Kış sekmesi açılırsa · O zaman giysiler örülebilir hale gelir; 10/10 değilse Kış kartları "Sonbaharı tamamla" ipucuyla kilitli (17. story sezon dolabı IAP kilidi erken açar).
- [ ] **Senaryo: Büyük yazı ve geri** · Verilen büyük yazı modu · Eğer Dolap açılırsa · O zaman kart adları kırpılmaz (2 satıra iner); Android geri Home'a döner.

### Teknik görevler (Unity/C#)
- [ ] `Wardrobe` sahnesi: Core `CollectionScreen` iskeleti (sekmeler, ızgara, detay paneli) üstüne `WardrobeGrid` ve `GarmentCard` prefab'ı; snapshot sprite yükleme (`persistentDataPath`, yoksa yeniden render).
- [ ] `AvatarView`: 2D katmanlı giydirme; `AvatarLayer` enum {head, neck, torso, hands, feet, lap}; gövde sprite'ı + katman `SpriteRenderer` sıralaması (sorting order sabit: lap < torso < neck < hands < head); tint = `DominantColor(rows)`.
- [ ] `GarmentTemplate.avatarSprite` ve `layer` alanları; sezon 2–3 şablonları (20 giysi: Kış — kalın kazak, kulaklık, boyunluk…; Bahar — ince hırka, bandana, çanta…) v1'de siluet + ad olarak var, sprite'lar 17. story'de tamamlanır.
- [ ] `WardrobeStore` okuma/yazma, `seasonUnlocked` kuralı (`owned.Count(season) == 10` veya IAP).
- [ ] Teyze gövde sanatı: 1 poz, 3 ten tonu seçeneği (Ayarlar), oturur poz (örgü koltuğu); EN'de ad "Auntie" (yerelleştirme anahtarı `avatar.name`).
- [ ] Home avatar bileşeni (aynı `AvatarView`, ölçek 0,6).
- [ ] Edit Mode testi: `DominantColor` ve katman eşlemesi.

### Analitik
- `wardrobe_view` {owned: int, season: int} — Dolap açılışında
- `garment_equip` {project: string, layer: string} — giydirmede
- `season_unlock` {season: int, source: "complete"|"iap"} — sezon açılınca

### Notlar
- Tasarım taslağında "Dolap · 3" düğmesi Play HUD'unda; Play'den Dolap'a geçiş turu duraklatır (sepet korunur, `Time.timeScale = 0`).
- Karar: avatar v1'de statik; animasyon SONRA 23. Katmanlı yapı animasyona hazır (sprite swap ile çalışır).
- Risk: 30 giysi sanatı solo için yük; sezon 2–3 sprite'ları soft launch'a kadar şart değil, siluetler yeterli.

## STORY: Yumak 12: Tur sonu akışı: taşma, Devam (reklam) ve satırların korunması
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Yumak** epic'inin **12. story'si** (epic #EPIC). Tema: Tur yapısı.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `RoundController`, `RoundEndView` (overlay), `ContinueFlow`, `AdsService` (Core), `YarnPool`

### Kullanıcı hikâyesi
Bir oyuncu olarak sepet taşınca ördüğüm satırların yerinde durduğunu hemen görmek, istersem bir reklam izleyip aynı sepetle devam etmek, istemezsem yeni bir sepetle aynı projeye kaldığım yerden devam etmek isterim, böylece taşma bir ceza değil bir mola olsun.

### Kabul kriterleri
- [ ] **Senaryo: Taşma overlay'i** · Verilen `RoundOverflowed` (bekleyen uçuşlar bitti) · Eğer tetiklenirse · O zaman 0,6 sn sonra yarı saydam overlay: başlık "Sepet taştı", altında "Örgün duruyor: Atkı 6/10" ve pano vurgulanır (üstte görünür kalır), tur puanı, "Devam et · reklam" (birincil, hak varsa), "Yeni sepet" (ikincil).
- [ ] **Senaryo: Devam** · Verilen "Devam et · reklam", reklam yüklü · Eğer izlenip `OnRewarded` gelirse · O zaman taşma çizgisinin üstündeki tüm yumaklar + en küçük 3 yumak sepetten 300 ms'de solarak kaldırılır (olgunlaşmakta olan kademe ≥ 6 yumaklar kaldırılmaz, panoya uçar), `RoundState.Playing`, aynı tur sürer; `continueUsed = true`.
- [ ] **Senaryo: Devam bir kez** · Verilen devam kullanıldı · Eğer tekrar taşarsa · O zaman overlay'de yalnızca "Yeni sepet" vardır.
- [ ] **Senaryo: Reklam yok** · Verilen reklam yüklenemedi · Eğer overlay açılırsa · O zaman Devam düğmesi görünmez (boş teklif yok); 10 sn içinde yüklenirse belirir.
- [ ] **Senaryo: Yarıda kapatma** · Verilen reklam yarıda kapatıldı · Eğer overlay'e dönülürse · O zaman hak kullanılmamış sayılır, düğme kalır.
- [ ] **Senaryo: Yeni sepet** · Verilen "Yeni sepet" · Eğer basılırsa · O zaman `round_end` yazılır, sepet boşalır (yumaklar 400 ms'de aşağı "dökülür", Reduce Motion'da solar), pano ve satırlar aynen kalır, interstitial kotası uygunsa (13. story) interstitial, sonra yeni tur aynı projeyle başlar; sepet genişliği varsayılana döner.
- [ ] **Senaryo: Çıkış** · Verilen overlay'de Android geri · Eğer basılırsa · O zaman "Yeni sepet" ile aynı (tur biter, Home'a gidilir); satır kaybı yok.
- [ ] **Senaryo: No Ads** · Verilen No Ads satın alınmış · Eğer overlay açılırsa · O zaman Devam düğmesi yine "reklam" ister (ödüllü video No Ads'ten etkilenmez); interstitial yok.

### Teknik görevler (Unity/C#)
- [ ] `RoundController`: `RoundOverflowed` → `WaitForFlights` → `RoundEndView.Show(stats, continueAvailable)`; `EndRound(reason)` → `round_end`, `YarnPool.ReleaseAll()`, `Basket.ResetWidth()`, `RoundStats` sıfırla, `roundIndex++`.
- [ ] `ContinueFlow`: durum {Offered, Loading, Showing, Rewarded, Failed}; `AdsService.ShowRewarded(placement: "continue")`; ödül yalnızca SDK callback'inde; `RemoveForContinue()` = çizgi üstü + en küçük 3 (olgunlaşma adayları hariç).
- [ ] `RoundEndView` overlay (uGUI): pano üstte görünür kalır (overlay yalnızca sepet alanını kaplar), düğme durumları, büyük yazı uyumu.
- [ ] Dökülme animasyonu: `Basket` tabanı collider'ı 400 ms kapatılır, yumaklar düşer, ekran altında havuza döner; Reduce Motion'da `ReleaseAll` + solma.
- [ ] Remote Config: `continue_enabled`, `continue_remove_smallest` (3).
- [ ] Play Mode testi: taşma → Devam → çizgi üstü yumaklar kaldırıldı, satır sayısı değişmedi, `RoundState.Playing`.

### Analitik
- `round_end` {round_n: int, reason: "overflow"|"quit", round_score: int, drop_count: int, rows_this_round: int, continued: bool, expanded: bool, duration_s: int, project: string, rows_total: int} — tur bittiğinde
- `continue_offered` {round_n: int} · `continue_start` · `continue_rewarded` · `continue_failed` {reason: string}

### Notlar
- İlke: satır kaybı hiçbir yolla olmaz (çıkış, çökme, reklam hatası). 9. story yazma sıklığı bunun teminatı.
- "En küçük 3 yumak" kaldırma: oyuncuya görünür fayda (yer açılır) ama sepeti boşaltmaz; Play modunda ayarlanır.
- Açık soru: Devam sonrası ikinci bir "Devam" IAP (0,99 USD) soft launch'ta test edilebilir; v1'de yok.

## STORY: Yumak 13: Ödüllü sepet genişletme, interstitial kotası ve No Ads
labels: tema:monetizasyon, P1, efor:M
milestone: MVP

**Yumak** epic'inin **13. story'si** (epic #EPIC). Tema: Monetizasyon.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `ExpandBasketFlow`, `InterstitialPolicy`, `NoAdsGate`, `AdsService` ve `IapService` (Core), Play HUD "Sepeti genişlet · reklam"

### Kullanıcı hikâyesi
Bir oyuncu olarak sepet dolmaya başlayınca bir reklam izleyip bu turluk sepeti genişletmek isterim; reklamdan sıkılırsam tek seferlik No Ads alıp tur aralarındaki reklamları kapatmak isterim. Geliştirici olarak reklam yerleşimlerinin dürüst, ölçülü ve ölçülebilir olmasını isterim.

### Kabul kriterleri
- [ ] **Senaryo: Genişletme teklifi** · Verilen tur sürüyor, reklam yüklü, bu turda kullanılmadı · Eğer sepet doluluğu (en yüksek yumak merkezi) çizginin 120 px altına ulaşırsa · O zaman HUD'da "Sepeti genişlet · reklam" düğmesi belirir (tasarım taslağı); daha önce görünmez.
- [ ] **Senaryo: Genişletme** · Verilen düğmeye basıldı, reklam sonuna kadar izlendi · Eğer `OnRewarded` gelirse · O zaman sepet iç genişliği %88 → %100 ekran genişliğine 400 ms'de büyür (duvarlar dışa kayar, hasır görseli ölçeklenir), yumaklar fizikle yayılır, `expandUsed = true`, düğme kaybolur; tur bitince genişlik varsayılana döner.
- [ ] **Senaryo: Yarıda kapatma** · Verilen reklam yarıda kapatıldı · Eğer Play'e dönülürse · O zaman sepet aynı, düğme kalır, hak kullanılmamış.
- [ ] **Senaryo: Interstitial kotası** · Verilen "Yeni sepet" · Eğer koşullar sağlanırsa (tur sayısı ≥ 4, bugünkü interstitial < `interstitial_daily_cap` (3), son interstitial'dan ≥ `interstitial_min_gap_s` (180), No Ads yok, reklam yüklü) · O zaman tur arasında interstitial gösterilir; aksi halde gösterilmez, yeni tur hemen başlar.
- [ ] **Senaryo: İlk 3 tur temiz** · Verilen yeni oyuncu · Eğer ilk 3 tur oynanırsa · O zaman hiç interstitial yok (yalnızca oyuncunun seçtiği ödüllü videolar).
- [ ] **Senaryo: No Ads satın alma** · Verilen Ayarlar veya tur sonu overlay'indeki "Reklamları kaldır · 3,99 USD" · Eğer satın alınırsa · O zaman `NoAdsGate.IsActive` true, interstitial hiç gösterilmez, ödüllü video düğmeleri ("Devam", "Sepeti genişlet") aynen kalır; "Satın alımları geri yükle" çalışır.
- [ ] **Senaryo: Oyun ekranında reklam yok** · Verilen Play sahnesi · Eğer incelenirse · O zaman banner yoktur; interstitial yalnızca `RoundEndView` kapanışı ile yeni tur arasında.
- [ ] **Senaryo: Gün sıfırlama** · Verilen bugünkü interstitial 3 · Eğer yerel gün değişirse · O zaman sayaç 0.

### Teknik görevler (Unity/C#)
- [ ] `ExpandBasketFlow`: doluluk izleme (`MaturityWatcher` ile aynı aktif liste; en yüksek `position.y`), teklif eşiği `expand_offer_px` (120), `AdsService.ShowRewarded(placement: "expand")`, `Basket.SetInnerWidth(1.0f)` (paket `Container.SetInnerWidth`, 2. story), tween 400 ms.
- [ ] `InterstitialPolicy`: {roundsPlayedTotal, todayCount, lastShownAt (yerel), noAds}; `ShouldShow()`; sayaç `PlayerPrefs` + Cloud Save `ads/policy`; `AdsService.ShowInterstitial(placement: "round_gap")`.
- [ ] `NoAdsGate`: Unity IAP tüketilemeyen ürün `no_ads` (3,99 USD, TR yerel kademe); Core `PremiumGate` tüketilemeyen sürümü; makbuz doğrulama Cloud Code; yerel önbellek.
- [ ] Teklif noktaları: Ayarlar (satır), `RoundEndView` (interstitial gösterilecekse önce küçük "Reklamsız oyna · 3,99 USD" bağlantısı), ilk interstitial'dan sonra bir kez "Reklamları kaldırmak ister misin?" kartı.
- [ ] Remote Config: `interstitial_daily_cap`, `interstitial_min_gap_s`, `interstitial_min_rounds` (4), `expand_enabled`, `expand_offer_px`.
- [ ] Test reklam kimlikleri `dev`, gerçek `prod`; AdMob yerleşimleri (`AdsConfig`): `continue`, `expand` (ödüllü), `round_gap` (interstitial).
- [ ] Edit Mode testi: `InterstitialPolicy.ShouldShow` tüm koşullar (kota, aralık, ilk turlar, No Ads).

### Analitik
- `expand_offered` {round_n: int, drop_count: int} · `expand_start` · `expand_rewarded` · `expand_failed` {reason: string}
- `interstitial_shown` {round_n: int, today_count: int} · `interstitial_skipped` {reason: "cap"|"gap"|"early"|"noads"|"noload"}
- `noads_view` {source: string} · `noads_purchase` {price_local: string}

### Notlar
- Reklam ilkesi (BRIEF): interstitial günde ≤ 3, ödüllü video oyuncunun seçimi, No Ads tüm interstitial'ı kapatır, oyun ekranında banner yok. Fluffy Drop'un "çalışmayan No Ads" hatası: satın alma sonrası durum hem yerel hem bulutta, geri yükleme test senaryosu zorunlu.
- Gelir beklentisi: ödüllü eCPM ≈ 15 USD, interstitial 12–13 USD (ABD); 35+ kitlede ödüllü izleme oranı yüksek olabilir; kapı metriği ödüllü izleme/teklif ≥ %25.
- Karar: sepet genişletme turluk (kalıcı değil): kalıcı genişletme sepet kıtlığını (tutma kancası) bozar.

## STORY: Yumak 14: İlk tur deneyimi (FTUE)
labels: tema:cekirdek, P1, efor:M
milestone: MVP

**Yumak** epic'inin **14. story'si** (epic #EPIC). Tema: Çekirdek oynanış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** `TutorialDirector`, `TutorialFlags`, ilk tur sırası, Core `HintBubble`

### Kullanıcı hikâyesi
Bir yeni (muhtemelen 50+) oyuncu olarak kuralları oynarken, acele ettirilmeden öğrenmek ve ilk 3 dakikada ilk satırımı örmek isterim, böylece "bu oyun bana göre" diyeyim.

### Kabul kriterleri
- [ ] **Senaryo: İlk tur sırası** · Verilen ilk açılış · Eğer Play'e girilirse · O zaman ilk 12 yumak sabit bir öğretici sırayla gelir (3,3,4,4,5,5,4,4,3,3,5,5 kademe; 2 renk) — doğru oynanınca 2. düşüşte ilk birleşme, 8–10. düşüşte ilk 6. kademe ve ilk satır garantidir; sonrası rastgele.
- [ ] **Senaryo: Dört ipucu** · Verilen ilk tur · Eğer olaylar gelirse · O zaman 1) "Sürükle ve bırak" eli (ilk düşüşe kadar), 2) ilk birleşmede "Aynı boy yumaklar birleşir" balonu, 3) ilk olgunlaşmada "Büyüyen yumak örgüye gider" balonu panoyu işaret eder, 4) ilk satırda "İlk satırın! Atkı 1/10" balonu; hiçbiri girdiyi kilitlemez, hepsi 3 sn'de kendiliğinden kapanır.
- [ ] **Senaryo: Taşma ipucu** · Verilen ilk kez çizgiye 60 px yaklaşma · Eğer olursa · O zaman "Çizgiyi geçerse sepet taşar, örgün kalır" balonu bir kez.
- [ ] **Senaryo: İlk taşma** · Verilen ilk tur taşması · Eğer overlay açılırsa · O zaman "Örgün duruyor" satırı vurgulu (pano 2 kez nazikçe parlar) ve Devam teklifi ilk turda gösterilmez (reklamla tanışma ikinci turdan itibaren).
- [ ] **Senaryo: Tekrar yok** · Verilen ipuçları görüldü · Eğer ikinci tur oynanırsa · O zaman ipucu çıkmaz; Ayarlar'dan "İpuçlarını sıfırla" ile geri gelir.
- [ ] **Senaryo: Nasıl oynanır** · Verilen Home'daki ⓘ · Eğer basılırsa · O zaman 4 slaytlık kart (sürükle, birleştir, olgunlaş ve ör, taşsa da kalır); büyük yazı uyumlu; slayt başına ≤ 12 kelime.
- [ ] **Senaryo: Yavaş tempo** · Verilen ilk tur · Eğer oynanırsa · O zaman `drop_cooldown_ms` 600 (sonra 450); yerçekimi aynı.

### Teknik görevler (Unity/C#)
- [ ] `TutorialFlags` (Cloud Save `ftue/flags`), `TutorialDirector` (olaylar: FirstDrop, FirstMerge, FirstMatured, FirstRow, NearOverflow, FirstOverflow).
- [ ] `YarnSequence.WithScripted(int[] tiers, int[] colors)`: ilk 12 eleman sabit, sonra PRNG.
- [ ] El animasyonu ve balon prefab'ı (Core `HintBubble`, büyük yazı modunda ≥ 20 pt).
- [ ] "Nasıl oynanır" kartı: 4 slayt, TR/EN, çizimler (tasarım taslağı stilinde).
- [ ] `ContinueFlow` ve `ExpandBasketFlow`'a `firstRound` bayrağı (teklif gizleme).
- [ ] Remote Config: `ftue_scripted_sequence` (JSON), `ftue_cooldown_ms`.

### Analitik
- `ftue_step` {step: string, seconds_since_start: int} — her ipucu gösteriminde
- `first_row` {minutes_since_install: int, drop_count: int} — ilk satır (huni adımı)

### Notlar
- İlke: zorunlu, girdiyi kilitleyen eğitim yok (MustyBlockBlast ve Daily Drop ile aynı).
- Huni hedefi: yükleme → ilk satır ≤ 3 dk için ≥ %80; ilk satır → ilk giysi ≥ %60; çarpım kapı metriği (ilk giysi ≥ %50) ile tutarlı.

## STORY: Yumak 15: Analitik olay şeması ve kapı raporu
labels: tema:analitik, P1, efor:S
milestone: MVP

**Yumak** epic'inin **15. story'si** (epic #EPIC). Tema: Analitik.

**Öncelik:** P1 · **Efor:** S · **Milestone:** MVP · **Dokunduğu yer:** UGS Analytics olay tanımları, `docs/analytics.md`, SQL raporları

### Kullanıcı hikâyesi
Bir geliştirici olarak soft launch kapısını (D1, D7, ilk giysi oranı, oturum süresi, ödüllü izleme oranı) tek sorguyla görmek isterim, böylece karar veriyle verilir.

### Kabul kriterleri
- [ ] **Senaryo: Şema** · Verilen `docs/analytics.md` · Eğer okunursa · O zaman tüm olaylar (app_open, session_start, home_view, drop, merge, yarn_matured, rows_added, pattern_detected, project_start, garment_complete, first_garment, first_row, round_overflow, round_end, continue_*, expand_*, interstitial_*, noads_*, wardrobe_view, garment_equip, season_unlock, ftue_step, settings_change) parametre tipleriyle listelidir ve UGS'de tanımlıdır.
- [ ] **Senaryo: Doğrulama** · Verilen dev build · Eğer 3 tur oynanıp bir giysi tamamlanırsa · O zaman UGS Event Browser'da olaylar sırayla ve doğru parametrelerle görünür; `first_garment` tam bir kez.
- [ ] **Senaryo: Kapı sorgusu** · Verilen UGS SQL · Eğer `gate_report.sql` çalıştırılırsa · O zaman D1, D7, ilk giysi oranı (first_garment / app_open{first_open} ilk 3 gün), ortalama oturum, ödüllü izleme/teklif, interstitial/gün, çökme oranı tek tabloda gelir.
- [ ] **Senaryo: Huni** · Verilen `funnel_first_garment.sql` · Eğer çalıştırılırsa · O zaman app_open → first drop → first merge → first_row → first_garment adımları yüzdeyle gelir.
- [ ] **Senaryo: Gizlilik** · Verilen olaylar · Eğer incelenirse · O zaman kişisel veri (ad, e-posta, kesin konum) yoktur; ten tonu ayarı gönderilmez.

### Teknik görevler (Unity/C#)
- [ ] `Analytics` (Core) sarmalayıcısı: `Track(string, Dictionary<string, object>)`, kuyruk + çevrimdışı tampon; `drop` ve `merge` örnekleme kuralı burada.
- [ ] UGS Analytics custom event tanımları (dashboard'dan), parametre tipleri.
- [ ] `docs/analytics.md`, `docs/reports/gate_report.sql`, `funnel_first_garment.sql`, `ads_daily.sql` (yerleşim başına teklif/izleme/ödül), `rounds_per_garment.sql` (proje başına tur sayısı — satır hedefi ayarı için).
- [ ] Crashlytics (Firebase) veya UGS Crash raporlama: çökme oranı için.

### Analitik
- Bu story şemanın kendisidir.

### Notlar
- MustyBlockBlast'taki `docs/UGS SQL` yaklaşımı aynen uygulanır; Daily Drop'un `gate_report.sql` şablonu kopyalanıp kapı metrikleri değiştirilir.
- `rounds_per_garment` raporu 9. story satır hedeflerinin ve 7. story olgunlaşma süresinin ayar girdisidir.

## STORY: Yumak 16: Mağaza sayfası, ASO ve soft launch yapılandırması
labels: tema:magaza, P1, efor:M
milestone: MVP

**Yumak** epic'inin **16. story'si** (epic #EPIC). Tema: Mağaza ve çıkış.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** Play Console, mağaza metinleri, ekran görüntüleri, kapalı test

### Kullanıcı hikâyesi
Bir geliştirici olarak oyunu üç ülkede kapalı test ve ardından soft launch'a çıkarmak isterim, böylece gerçek oyuncu (35+ cozy kitle) verisi gelsin.

### Kabul kriterleri
- [ ] **Senaryo: Başlık ve metin** · Verilen Play listesi · Eğer yayımlanırsa · O zaman EN başlık "Yumak: Cozy Yarn Merge" (≤ 30 karakter), TR "Yumak: Örgü ve Yün Bulmacası"; kısa açıklama ≤ 80 karakter ("Merge yarn, knit cozy clothes. No timer, no pressure."); tam açıklamada "cozy knitting game", "yarn merge", "relaxing", "knit", "no timer" geçer; uzun tire yok.
- [ ] **Senaryo: Kategori** · Verilen Play Console · Eğer kategori seçilirse · O zaman "Puzzle" (alt: Merge); Fluffy Drop'un Trivia hilesi uygulanmaz (politika riski).
- [ ] **Senaryo: Görseller** · Verilen ekran görüntüleri · Eğer yüklenirse · O zaman 8 görüntü (sepet ve pano, olgunlaşma etiketi, satır örme, giysi tamamlama, dolap, Teyze giydirme, "taşsa da kalır" mesajı, büyük yazı modu), 1 özellik grafiği (Teyze + yumaklar), 15 sn video (isteğe bağlı); görüntü metinleri ≥ 36 pt (kitle).
- [ ] **Senaryo: Kapalı test** · Verilen 12+ test kullanıcısı (en az 4'ü 50+) · Eğer 14 gün sürerse · O zaman Play üretim erişimi formu doldurulur (MustyBlockBlast #1326 deneyimi).
- [ ] **Senaryo: Ülkeler** · Verilen soft launch · Eğer yayımlanırsa · O zaman yalnızca TR, PH, CA; IAP fiyatları yerel kademe (No Ads 3,99 USD, paket 2,99 USD, sezon 4,99 USD).
- [ ] **Senaryo: Sürüm bilgisi** · Verilen build · Eğer yayımlanırsa · O zaman versionCode ve versionName CHANGELOG ile eşleşir; gizlilik politikası ve veri güvenliği formu dolu; `MustyMergePhysics` ve `MustyCozyCore` sürümleri CHANGELOG'da.

### Teknik görevler (Unity/C#)
- [ ] Mağaza metinleri `docs/store/{en,tr}.md`; ASO anahtar kelime listesi ("cozy knitting game", "yarn merge", "relaxing merge", "knit", "no timer puzzle"); rakip başlıkları (Suika klonları, cozy knitting sim'ler).
- [ ] Ekran görüntüsü şablonu (Design kanvası, tasarım taslağından), 1080×1920; büyük yazı.
- [ ] Veri güvenliği formu: reklam SDK'sı, analitik, Cloud Save, IAP; `docs/privacy.md`.
- [ ] Play Console kurulumu: iç test → kapalı test → üretim (3 ülke); IAP ürünleri `no_ads`, `pattern_pack_*`, `season_*` tanımlı (17. story öncesi pasif).
- [ ] Sürümleme: `CHANGELOG.md`, `versionCode` CI'da otomatik.

### Analitik
- Mağaza: Play Console dönüşüm oranı; listing deneyi.

### Notlar
- Başlık testi: soft launch'ta "Cozy Yarn Merge" ile "Knit & Merge Puzzle" A/B (Play listing experiments); "No Timer" ifadesi kısa açıklamada her iki kolda.
- Organik indirmelerin ≈ %65'i mağaza aramasından: "yarn merge" niş ama rakipsiz; "cozy" hacimli ama rekabetli.

## STORY: Yumak 17: Desen paketleri ve sezon dolabı IAP
labels: tema:monetizasyon, P2, efor:L
milestone: Soft launch

**Yumak** epic'inin **17. story'si** (epic #EPIC). Tema: Monetizasyon.

**Öncelik:** P2 · **Efor:** L · **Milestone:** Soft launch · **Dokunduğu yer:** `PatternPack` (SO), `StitchStyle`, `ShopScreen` (Core iskeleti), `IapService` (Core), `WardrobeStore.seasonUnlocked`

### Kullanıcı hikâyesi
Bir sadık oyuncu olarak örgüme farklı ilmek dokuları ve paletler (desen paketi) almak, istersem Kış dolabını Sonbahar bitmeden açmak isterim; geliştirici olarak IAP'nin oyunun adaletini bozmadan (hız satmadan) gelir getirmesini isterim.

### Kabul kriterleri
- [ ] **Senaryo: Mağaza ekranı** · Verilen Home'daki "Mağaza" · Eğer açılırsa · O zaman 3 bölüm: Desen paketleri (Çiçekli Bahçe, Nordik, Anadolu Kilimi — her biri 2,99 USD), Sezon dolabı (Kış 4,99 USD, Bahar 4,99 USD), Reklamları kaldır (3,99 USD); "Satın alımları geri yükle"; fiyatlar yerel.
- [ ] **Senaryo: Desen paketi içeriği** · Verilen Nordik paketi · Eğer alınırsa · O zaman 1 ilmek stili (sprite seti: "V" yerine Nordik X ilmek), 2 palet (4'er renk), 1 Teyze aksesuarı (örgü sepeti kozmetiği); kurallar (düz/çizgili/çapraz) değişmez, satır hedefleri değişmez.
- [ ] **Senaryo: Paket uygulama** · Verilen satın alınmış paket · Eğer Dolap > "Stil" sekmesinde seçilirse · O zaman sonraki projeden itibaren ilmek stili ve palet seçeneği aktif (aktif projenin ortasında değişmez); daha önce örülen giysiler eski stiliyle kalır.
- [ ] **Senaryo: Sezon dolabı** · Verilen Sonbahar 4/10 · Eğer Kış dolabı alınırsa · O zaman Kış giysileri örülebilir olur ve oyuncu aktif projeyi Sonbahar/Kış arasından seçebilir ("Proje seç" paneli); Sonbahar tamamlanınca da aynı panel ücretsiz açılır.
- [ ] **Senaryo: Önizleme** · Verilen paket kartı · Eğer dokunulursa · O zaman oyuncunun son giysisi o ilmek stiliyle anında önizlenir (`StitchGrid` ile canlı render).
- [ ] **Senaryo: Geri yükleme** · Verilen yeni cihaz · Eğer "Geri yükle" basılırsa · O zaman tüm tüketilemeyen ürünler tanınır ve `WardrobeState` ile birleştirilir.
- [ ] **Senaryo: Hız satışı yok** · Verilen herhangi bir IAP · Eğer incelenirse · O zaman satır, yumak, olgunlaşma süresi veya sepet boyutu satılmaz (yalnızca görsel + içerik erişimi).

### Teknik görevler (Unity/C#)
- [ ] `PatternPack` SO: id, nameKey, `StitchStyle` (ilmek sprite seti 2 varyant: düz/çapraz), paletler, aksesuar sprite, sku, previewSprite.
- [ ] `StitchGrid`'e `StitchStyle` parametresi; `KnitRow`'a `styleId` (eski giysiler stilini korur); `OwnedGarment.styleId`.
- [ ] Unity IAP tüketilemeyen ürünler: `pattern_garden`, `pattern_nordic`, `pattern_kilim`, `season_winter`, `season_spring`, `no_ads`; Play Console ürün tanımları, TR yerel kademe.
- [ ] `ShopScreen` (Core iskeleti) + `PackCard`, canlı önizleme (`RenderTexture`).
- [ ] `WardrobeStore`: `ownedPacks`, `seasonUnlocked` kaynağı ("complete"|"iap"); `ProjectQueue.Choose(garmentId)` (çoklu sezon açıkken).
- [ ] Sezon 2–3 giysi sprite'ları ve maskeleri tamamlanır (11. story siluetlerinin üstüne).
- [ ] Makbuz doğrulama Core `PremiumGate` (Cloud Code), çevrimdışı önbellek.
- [ ] Fiyat testi: Remote Config ile paket fiyatı 1,99/2,99 USD (Play'de iki sku).

### Analitik
- `shop_view` {source: string} · `pack_preview` {sku: string} · `iap_purchase` {sku: string, price_local: string, garments_done: int} · `style_equip` {style: string} · `project_choose` {project: string, season: int}

### Notlar
- Karar: IAP yalnızca kozmetik + içerik; puzzle IAP +%20 büyüme trendi bu modelde (içerik paketi) görülüyor.
- Anadolu Kilimi paketi TR kitlesi için; design-identity ile uyumlu; EN'de "Anatolian Kilim".
- Teklif noktası: 2. giysi tamamlandığında mağaza bir kez tanıtılır (kart, kapatılabilir); agresif satış ekranı yok.

## STORY: Yumak 18: Günlük hediye yumak, günlük hedef ve haftalık proje
labels: tema:ritual, P2, efor:M
milestone: Soft launch

**Yumak** epic'inin **18. story'si** (epic #EPIC). Tema: Ritüel.

**Öncelik:** P2 · **Efor:** M · **Milestone:** Soft launch · **Dokunduğu yer:** `DailyGift` (Core), `DailyGoal`, `WeeklyProject`, Home kartları, yerel bildirim

### Kullanıcı hikâyesi
Bir oyuncu olarak her gün uygulamayı açınca küçük bir hediye (olgun bir yumak), günlük bir satır hedefi ve haftada bir özel proje bulmak isterim, böylece örgüye dönmek için bir sebebim olsun.

### Kabul kriterleri
- [ ] **Senaryo: Günlük hediye** · Verilen bugün ilk açılış · Eğer Home görünürse · O zaman "Günün yumağı" kartı: bir 6. kademe yumak (aktif paletten rastgele renk); "Al" basılınca sonraki turun ilk elde yumağı bu olur (sıraya önden eklenir); günde bir.
- [ ] **Senaryo: Seri** · Verilen ardışık günler · Eğer 7. gün alınırsa · O zaman hediye 7. kademe; seri kırılınca 6'ya döner; seri Home'da "3 gün" olarak görünür, kaçıran gün için ceza yok.
- [ ] **Senaryo: Günlük hedef** · Verilen gün başı · Eğer Home açılırsa · O zaman "Bugün 6 satır ör" hedefi (hedef = aktif projenin satır hedefinin %40'ı, 4–12 arası); tamamlanınca rozet + 1 ek günlük hediye (7. kademe); bildirim izni verildiyse akşam 19:00'da tamamlanmadıysa tek hatırlatma.
- [ ] **Senaryo: Haftalık proje** · Verilen Pazartesi · Eğer Home açılırsa · O zaman "Haftanın projesi: Çizgili Atkı" kartı: belirli desen kuralı (örn. en az 3 renk değişimi) ile ördüğünde dolapta özel etiketli giysi; Pazar 23:59'da kapanır; aktif projeden bağımsız, "Haftalık" sekmesinde ayrı pano.
- [ ] **Senaryo: Bildirim** · Verilen ilk giysi tamamlandı · Eğer kart kapanırsa · O zaman "Yarın hatırlatayım mı?" bir kez sorulur; evet denirse sistem izni; Ayarlar'dan saat ve kapatma.
- [ ] **Senaryo: Çevrimdışı** · Verilen bağlantı yok · Eğer hediye alınırsa · O zaman yerel kayıt, bağlantı gelince Cloud Save.

### Teknik görevler (Unity/C#)
- [ ] Core `DailyGift` iskeleti bağlama: ödül tipi `YarnGift {tier, colorId}`; `YarnSequence.PushFront(tier, color)`.
- [ ] `DailyGoal`: hedef hesabı, `rows_added` dinleme, rozet, `goal/{date}` Cloud Save.
- [ ] `WeeklyProject`: Remote Config `weekly_project` (JSON: garmentId, ruleId, palette, weekStart); `KnitSystem` ikinci örnek (haftalık pano); `PatternRule` ek kural `MinColorChanges(n)`.
- [ ] Unity Mobile Notifications: Android kanal, iOS izin; `ReminderScheduler` (günlük hedef 19:00, kullanıcı saati).
- [ ] Home kartları: hediye, hedef, haftalık; büyük yazı uyumu.
- [ ] Sunucu saati (Core `ServerClock`) ile gün değişimi; çevrimdışı yerel.

### Analitik
- `daily_gift_claim` {tier: int, streak: int} · `daily_goal_done` {rows: int, target: int} · `weekly_project_start` {id: string} · `weekly_project_done` {id: string} · `notification_opt_in` {enabled: bool, hour: int}

### Notlar
- Hediye "olgun yumak" = somut satır; coin yok, sahte ekonomi yok.
- Haftalık proje LiveOps'un en ucuz biçimi: içerik yok, kural ve palet var (Daily Drop 27 ile aynı felsefe).

## STORY: Yumak 19: Ayarlar ve erişilebilirlik (büyük yazı, tek el, sakin mod)
labels: tema:erisilebilirlik, P2, efor:S
milestone: Soft launch

**Yumak** epic'inin **19. story'si** (epic #EPIC). Tema: Erişilebilirlik.

**Öncelik:** P2 · **Efor:** S · **Milestone:** Soft launch · **Dokunduğu yer:** `SettingsScreen` (Core), oyun ayar bayrakları, `DropController`, `MaturityWatcher`

### Kullanıcı hikâyesi
Bir 35+/senior oyuncu olarak yazıları büyütmek, parmağımın yumağı kapatmamasını, sesleri ve titreşimi ayarlamak, istersem oyunu daha da yavaşlatmak isterim; renk ayırt etmekte zorlanan bir oyuncu olarak kademeleri rakamla görmek isterim.

### Kabul kriterleri
- [ ] **Senaryo: Ayarlar listesi** · Verilen Ayarlar · Eğer açılırsa · O zaman Ses, Müzik, Haptik, Reduce Motion, Büyük yazı (otomatik/açık/kapalı), Kademe numarası, Parmak payı (0/80/120 px), Sakin mod, Ten tonu (Teyze), Dil, Bildirim, Uygulamayı puanla, Reklamları kaldır, Gizlilik, Sürüm.
- [ ] **Senaryo: Sakin mod** · Verilen açık · Eğer oynanırsa · O zaman `drop_cooldown_ms` 600, yerçekimi −16, taşma süresi 1,8 sn, olgunlaşma 1,2 sn; puan tablosu aynı (haftalık proje puanı etkilenmez, çünkü puan ikincil); HUD'da küçük "sakin" rozeti.
- [ ] **Senaryo: Büyük yazı** · Verilen açık · Eğer HUD çizilirse · O zaman tüm metinler +25%, dokunma hedefleri ≥ 64 px, önizleme slotları +%20, Dolap kartları 2 sütun; kırpma yok.
- [ ] **Senaryo: Reduce Motion** · Verilen açık · Eğer oynanırsa · O zaman uçuş, katlama, dökülme, parçacık ve nefes animasyonları kapalı (solma ile); fonksiyon kaybı yok.
- [ ] **Senaryo: Kademe numarası** · Verilen açık · Eğer yumaklar çizilirse · O zaman her yumakta rakam (≥ 14 pt, kontrast 4,5:1).
- [ ] **Senaryo: Değerlendirme isteği** · Verilen 2. tamamlanan giysi · Eğer tamamlama kartı kapanırsa · O zaman mağaza içi değerlendirme isteği bir kez gösterilir.
- [ ] **Senaryo: Android geri** · Verilen Ayarlar açık · Eğer geri basılırsa · O zaman önceki sahneye dönülür.

### Teknik görevler (Unity/C#)
- [ ] Core `SettingsScreen` bağlama; oyuna özgü anahtarlar: `tier_numbers`, `finger_offset`, `calm_mode`, `skin_tone`.
- [ ] `CalmModeProfile`: `RoundConfig` ve `PhysicsProfile` üzerine yazan değer seti; tur başında uygulanır.
- [ ] TextMeshPro auto-size ve TR karakter atlası; büyük yazı için `LayoutGroup` ölçek bayrağı.
- [ ] In-app review (Google Play In-App Review) tetik kuralı.
- [ ] Kontrast denetimi: tüm palet renklerinin pano zemini ve rakam rengiyle 4,5:1; palet SO'larında editör uyarısı.

### Analitik
- `settings_change` {key: string, value: string} · `review_prompt` {shown: bool}

### Notlar
- Sakin mod bu kitle için ana özellik olabilir; soft launch'ta açma oranı yüksekse varsayılan olur (Remote Config `calm_default`).

## STORY: Yumak 20: Giysi kartı paylaşımı
labels: tema:paylasim, P3, efor:M
milestone: Canlı

**Yumak** epic'inin **20. story'si** (epic #EPIC). Tema: Paylaşım.

**Öncelik:** P3 · **Efor:** M · **Milestone:** Canlı · **Dokunduğu yer:** `ShareCardRenderer`, `ShareService` (Core), Dolap detay "Paylaş"

### Kullanıcı hikâyesi
Bir oyuncu olarak ördüğüm giysiyi Teyze'nin üstünde güzel bir kartla WhatsApp/Instagram'a göndermek isterim, böylece arkadaşlarım (ve torunlarım) ne ördüğümü görsün.

### Kabul kriterleri
- [ ] **Senaryo: Kart** · Verilen Dolap detayında "Paylaş" · Eğer basılırsa · O zaman 1080×1350 PNG: Teyze giysiyle, giysi büyük, desen adı, "N. giysim", renk paleti, mağaza linki (UTM); oyuncu adı yok.
- [ ] **Senaryo: Metin** · Verilen paylaşım · Eğer sistem sayfası açılırsa · O zaman "Yumak'ta 3. giysimi ördüm: Çizgili Atkı 🧶 <link>" (TR/EN).
- [ ] **Senaryo: Hız** · Verilen orta sınıf cihaz · Eğer Paylaş'a basılırsa · O zaman kart 300 ms içinde hazır.
- [ ] **Senaryo: Tamamlama anından** · Verilen tamamlama kartı (10. story) · Eğer "Paylaş" eklenmişse · O zaman aynı kart üretilir; düğme ikincil.

### Teknik görevler (Unity/C#)
- [ ] `ShareCardRenderer` (Daily Drop 12'den kopya, şablon farklı): ayrı `Camera` + `Canvas`, `RenderTexture` → PNG.
- [ ] Core `ShareService` (Android `FileProvider` + `Intent`, iOS native).
- [ ] Kart şablonu prefab (açık tema, kumaş dokusu), TR/EN font.
- [ ] UTM: `utm_source=share&utm_content=garment_{id}`.

### Analitik
- `share_tap` {project: string, garments_done: int} · `share_done` {target: string|null}

### Notlar
- Bu kitlede paylaşım kanalı WhatsApp aile grupları; kartta büyük yazı ve sade düzen.

## STORY: Yumak 21: Gerçek örgü desenleri (PDF indir)
labels: tema:meta, P3, efor:L, SONRA
milestone: SONRA

**Yumak** epic'inin **21. story'si** (epic #EPIC). Tema: Meta ve içerik.

**Öncelik:** P3 · **Efor:** L · **Milestone:** SONRA · **Dokunduğu yer:** `PatternExporter`, gerçek tarif şablonları, Dolap detay "Tarifi indir"

### Kullanıcı hikâyesi
Bir örgü meraklısı oyuncu olarak oyunda ördüğüm atkının gerçek örgü tarifini (ilmek sayısı, renk sırası, iplik önerisi) PDF olarak indirmek isterim, böylece oyun gerçek hobime bağlansın.

### Kabul kriterleri
- [ ] **Senaryo: Tarif üretimi** · Verilen dolaptaki Çizgili Atkı (10 satır, 4 renk) · Eğer "Tarifi indir" basılırsa · O zaman A4 PDF: giysi adı, önerilen iplik kalınlığı ve şiş numarası, gerçek ilmek sayısına ölçeklenmiş renk sırası (oyundaki 1 satır = gerçek 8 sıra), temel teknik (düz/ters), telif satırı.
- [ ] **Senaryo: Doğruluk** · Verilen 3 şablon (atkı, bere, çorap) · Eğer bir örgü bilen kişi uygularsa · O zaman tarif örülebilir (uzman incelemesi).
- [ ] **Senaryo: Ücret** · Verilen tarif · Eğer indirilirse · O zaman ücretsiz; desen paketi sahiplerine o paketin gerçek tarifleri eklenir.

### Teknik görevler (Unity/C#)
- [ ] `PatternExporter`: HTML şablon → PDF (Android `PdfDocument` native plugin veya sunucu tarafı Cloud Code + depolama); paylaşım `ShareService`.
- [ ] Gerçek tarif şablonları (3 giysi) — örgü bilen bir danışmanla yazılır.
- [ ] Ölçekleme kuralı `docs/knit-patterns.md`.

### Analitik
- `pattern_export` {project: string}

### Notlar
- ASO etkisi: "knitting patterns" aramasında organik; ama içerik doğruluğu için uzman şart; soft launch sonrası.

## STORY: Yumak 22: Arkadaşa giysi hediye
labels: tema:paylasim, P3, efor:L, SONRA
milestone: SONRA

**Yumak** epic'inin **22. story'si** (epic #EPIC). Tema: Paylaşım ve sosyal.

**Öncelik:** P3 · **Efor:** L · **Milestone:** SONRA · **Dokunduğu yer:** `GiftService` (Cloud Code), davet kodu, Dolap "Hediye et"

### Kullanıcı hikâyesi
Bir oyuncu olarak ördüğüm bir giysiyi arkadaşımın (kızımın, torunumun) Teyze'sine hediye etmek isterim, böylece oyun aile içinde bir bağ olsun.

### Kabul kriterleri
- [ ] **Senaryo: Kod** · Verilen profil · Eğer "Arkadaş ekle" açılırsa · O zaman 6 karakterli kod ve link.
- [ ] **Senaryo: Hediye** · Verilen dolaptaki giysi · Eğer "Hediye et" + arkadaş seçilirse · O zaman giysi kopyası arkadaşın dolabına "Hediye · Ayşe'den" etiketiyle girer; gönderenin kopyası kalır; günde 1 hediye.
- [ ] **Senaryo: Bildirim** · Verilen hediye alındı · Eğer uygulama açılırsa · O zaman Home'da "Hediyen var" kartı ve Teyze'ye giydirme teklifi.
- [ ] **Senaryo: Gizlilik** · Verilen takma ad · Eğer boşsa · O zaman "Örgücü" + 4 hane; gerçek ad istenmez.

### Teknik görevler (Unity/C#)
- [ ] UGS Friends veya Cloud Save + Cloud Code `sendGift` / `claimGifts`; `OwnedGarment.giftedBy`.
- [ ] Davet kodu (base32, çakışma kontrolü), Dolap "Hediye et" akışı, Home hediye kartı.
- [ ] Engelleme ve kaldırma.

### Analitik
- `gift_send` {project: string} · `gift_claim` {project: string} · `friend_added` {count: int}

### Notlar
- Bu kitlede aile bağı güçlü viral kanal; ama sosyal altyapı soft launch verisi olmadan erken.

## STORY: Yumak 23: Karakter animasyonları
labels: tema:meta, P3, efor:L, SONRA
milestone: SONRA

**Yumak** epic'inin **23. story'si** (epic #EPIC). Tema: Dolap ve avatar.

**Öncelik:** P3 · **Efor:** L · **Milestone:** SONRA · **Dokunduğu yer:** `AvatarView`, animasyon rig'i (Unity 2D Animation), tepki olayları

### Kullanıcı hikâyesi
Bir oyuncu olarak Teyze'nin örgü örerken şişlerini oynattığını, satır gelince gülümsediğini, giysi bitince giyip döndüğünü görmek isterim, böylece karakter canlı hissedilsin.

### Kabul kriterleri
- [ ] **Senaryo: Boşta** · Verilen Play'de Teyze · Eğer tur sürüyorsa · O zaman şiş örme döngüsü (2 sn), göz kırpma; Reduce Motion'da statik.
- [ ] **Senaryo: Tepkiler** · Verilen olaylar · Eğer `rows_added` / `garment_complete` / `round_overflow` gelirse · O zaman gülümseme / giyinip dönme / "olsun" omuz silkme (600 ms); giysi katmanları rig ile hareket eder.
- [ ] **Senaryo: Performans** · Verilen rig · Eğer oynarsa · O zaman ek draw call ≤ 4, orta sınıf cihazda 60 fps.

### Teknik görevler (Unity/C#)
- [ ] Unity 2D Animation (PSB rig, 2 poz: oturur/ayakta), katman sprite'larının rig'e bağlanması (`SpriteResolver`).
- [ ] `AvatarReactions`: olay → animasyon tetik; Reduce Motion bayrağı.
- [ ] Mevcut 30 giysi sprite'ının rig'e uyarlanması (en pahalı kalem).

### Analitik
- Yok.

### Notlar
- 11. story'nin katmanlı yapısı rig'e taşınabilir olmalı (sprite swap). Sanat yükü solo için ağır; sözleşmeli animatör düşünülür.

## STORY: Yumak 24: Sezon etkinlikleri
labels: tema:meta, P3, efor:M, SONRA
milestone: SONRA

**Yumak** epic'inin **24. story'si** (epic #EPIC). Tema: Meta ve tutma.

**Öncelik:** P3 · **Efor:** M · **Milestone:** SONRA · **Dokunduğu yer:** `EventSchedule` (Remote Config), etkinlik giysileri, Home etkinlik kartı

### Kullanıcı hikâyesi
Bir oyuncu olarak yılbaşında, anneler gününde veya kış başında sınırlı süreli özel bir giysi (yılbaşı çorabı, kalpli atkı) örmek isterim, böylece dolabım mevsimlerle anılarla dolsun.

### Kabul kriterleri
- [ ] **Senaryo: Takvim** · Verilen Remote Config `events` · Eğer tarih aralığındaysa · O zaman Home'da etkinlik kartı ve "Etkinlik" sekmesinde özel proje (özel palet + siluet), süre sonu geri sayımı.
- [ ] **Senaryo: Tamamlama** · Verilen etkinlik giysisi · Eğer süre içinde bitirilirse · O zaman dolapta "2026 Yılbaşı" etiketiyle kalıcı; süre dolunca satırlar kalır ama proje kapanır (bir sonraki yıl devam).
- [ ] **Senaryo: İçerik** · Verilen etkinlik · Eğer yayımlanırsa · O zaman yeni build gerekmez (sprite'lar Addressables ile uzaktan).

### Teknik görevler (Unity/C#)
- [ ] `EventSchedule` şeması, `KnitSystem` üçüncü örnek (etkinlik panosu), Addressables ile uzaktan sprite.
- [ ] Etkinlik giysi şablonları (yılda 4).

### Analitik
- `event_start` {id: string} · `event_complete` {id: string}

### Notlar
- Haftalık proje (18. story) altyapısının üstüne; LiveOps takvimi `docs/liveops.md`.
