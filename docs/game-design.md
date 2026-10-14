<!-- Kaynak: docs/issues/yumak.md (epic bölümü). GitHub'daki epic issue ile aynı metin; karar değişirse önce burası, sonra epic güncellenir. -->

# Yumak — birleştir, olgunlaştır, ör: sepet taşsa da örgü kalır

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
