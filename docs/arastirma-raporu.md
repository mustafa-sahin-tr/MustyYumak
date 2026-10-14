# Fluffy Drop Analizi ve Özgün Oyun Fikirleri Raporu

14 Ekim 2026 · Mustafa Şahin (araştırma ve yazım: Claude)
Canlı doküman (yorum ve düzenleme): https://claude.ai/code/artifact/8ab0af47-8a63-4fac-8fd7-aad383f2fe81
Tasarım kanvası (telefon ekranları): https://claude.ai/artifact/3VMA4d5qTn8GF89PDf5Uj9

> Bu dosya, portföydeki her oyun reposuna aynı içerikle kopyalanmıştır. Oyuna özgü karar ve kapsam `docs/game-design.md` dosyasındadır.

## Yönetici özeti

Fluffy Drop'tan alınacak ders oyun mekaniği değil, dağıtım tarifidir: isimde anahtar kelime, düşük rekabetli kategori, senior kitle, haftalık sürüm. Bu tarifi gerçekten yeni bir mekanik veya ritüelle birleştiren üç oyun öneriyoruz: Daily Drop (3 haftada çıkar), Ink Bloom (ana bahis) ve Aile Yapbozu (60+ serisinin temeli).

- Fluffy Drop (UniRise, Hong Kong) Nisan 2026'da çıktı, 6 ayda Google Play'de 1M+ indirme ve 4,6 puan aldı. "No Timer" başlığı, Trivia kategorisinde #2 sıralaması ve 60+ kitle organik büyümeyi sağladı; işlevsiz coin ekonomisi, agresif reklam ve çalışmayan No Ads ise 1 yıldızların kaynağı.
- Pazar indirme bazında küçülüyor (H1 2026: indirmeler −%12) ama hybrid-casual bulmaca +%23 büyüyor; Sort alt türü yeni girene açık, Block alt türü 6 ayda 2.000 klonla kapalı. Türkiye stüdyoları (Loom, Rollic, Grand) bu dalganın merkezinde.
- Sekiz özgün fikir içinden ilk üçü hem tek ekranda anlaşılıyor hem de 2–6 haftalık solo MVP'ye sığıyor. Ortak "cozy motor" olmadan 25 oyun hedefi solo yürümez; ilk hafta bu motoru mevcut Block Blast tarzı oyundan ayrıştırmaya harcanmalı.

İlk adım: motoru ayrıştır, Daily Drop'u üç haftada Android soft launch'a koy, paylaşım oranını ölç; eşik tutarsa Ink Bloom'a geç.

## Fluffy Drop: oyun analizi

Fluffy Drop, Hong Kong merkezli bir "portföy fabrikası"nın 6 ayda 1M+ indirmeye ulaşan, ASO ve niş kitle seçimiyle kazanan bir renk eşleştirme bulmacasıdır; oyun tasarımı değil, konumlandırma ve tempo başarısıdır.

**Künye**

| Alan | Değer |
| --- | --- |
| Yayıncı | UniRise Digital Limited (Hong Kong), marka: UniRise Studio |
| İlk sürüm | 21 Nisan 2026 (iOS 1.0); 6 ayda 26 sürüm, yaklaşık haftalık güncelleme (App Store UK) |
| Google Play | 1M+ indirme, 4,6 puan, 18,2K yorum, "#2 top free trivia" |
| App Store | ABD 4,9 (35K oy, Board #9); İngiltere 4,8 (3,8K oy, Board #54) |
| Boyut / dil | 344 MB; İngilizce + 14 dil (Türkçe dahil) |
| Portföy | 13 aktif uygulama, 6 ayda 104 sürüm; Mahjong, Sort, Word, Jigsaw ve dini kelime oyunları (Marlvel) |

**Mekanik ve core loop**

Mağaza sloganı "Drag to Eat Fluffy Balls!": oyuncu pofuduk renkli topları parmağıyla sürükleyip aynı renkteki bölgelere sokar, tahta temizlenince seviye biter. Yorumlara göre oyun kaydırmalı bulmaca (slide puzzle) gibi işler; zincir reaksiyon planlaması ve 3 adet kazanılan araç (booster) var. Seviye tabanlıdır (535+ seviye bildiren oyuncular var), harita/yol üzerinde ilerleme ve yakın zamanda eklenen 5 kişilik yarışma modu bulunur.

- Tek seanslık, 1–3 dakikalık seviyeler; her seviye sonunda interstitial reklam.
- "NO TIMER NO STRESS" vaadi; ancak bazı seviyelerde süre sayacı olduğu şikayet ediliyor.
- ASMR tarzı ses efektleri, pastel ve yumuşak animasyon; 80'ler arcade çağrışımı yapan renk paleti.

**Monetizasyon**

| Kalem | Fiyat (GBP, UK) | Not |
| --- | --- | --- |
| Reklam | — | Her seviye sonrası; "ad loop" şikayetleri yaygın |
| No Ads | 9,99 | Satın alınca reklamların kesilmediği şikayeti (iade alınmış) |
| Starter Pack | 1,99 | Giriş teklifi |
| 3-in-1 Bundle | 0,99 / 3,99 / 6,99 / 9,99 | Kademeli paket |
| Coin bundle | 2,99 / 4,99 | Coin'in harcanacak yeri yok |
| Booster Pack | 8,99 | |
| Piggy bank | 3,99 | |

**Neden tuttu**

1. Başlıkta "No Timer": rahatlatıcı/senior kitlenin aradığı anahtar kelime doğrudan isimde. Aynı yaklaşımı Block Color Mania No Timer, Drop Escape No Timer, Slide Block Rush No time limit gibi onlarca rakip kopyalıyor; bir arama trendi haline gelmiş.
2. Kategori seçimi: Puzzle yerine Trivia kategorisinde listelenerek düşük rekabette #2'ye çıktı; ABD, Almanya ve Japonya'da trivia sıralamalarında üstte (AppBrain).
3. Kitle: 60+ yaş ve çocuk. Yorumlar "senior", "beyin egzersizi", "yolculukta sakinlik" diyor. Bu kitle düşük CPI, yüksek sadakat ve sabırlı reklam toleransı demek.
4. Tempo: haftalık sürüm, 15 dil, offline oynanış; ucuz ama sürekli büyüme.
5. Dokunsal tatmin: pofuduk toplar + ASMR ses = izlemesi keyifli, UGC'ye uygun.

**Zayıf noktalar (fırsat alanı)**

- Coin ekonomisi işlevsiz; ödüller açıklanmıyor; ana menüye dönüş yolu yok.
- Reklam agresifliği ve çalışmayan No Ads, en çok tekrar eden 1–2 yıldız nedeni.
- Sosyal yarış modu, sakinlik için gelen senior kitlede tepki yarattı.
- 344 MB boyut, düşük cihazlı pazarlarda indirme engeli.

## Tür analizi: Fluffy Drop'un ait olduğu üç dalga

Fluffy Drop üç ayrı dalganın kesişiminde duruyor: Suika ile viral olan fizik tabanlı drop/merge, 2025–26'nın Sort/Block/Screw hybrid-casual bulmaca dalgası ve "No Timer / cozy" senior konumlandırması. Kazanan oyunların ortak noktası mekanik değil, tek ekran görüntüsünde anlaşılan dokunsal bir fikir ile ucuz ve sürekli dağıtımdır.

**Temsilci oyunlar**

| Oyun | Stüdyo | Mekanik | Model | Sonuç |
| --- | --- | --- | --- | --- |
| Suika Game | Aladdin X (Japonya) | Meyve bırak, aynıları birleştir, karpuza ulaş; kapta taşma = oyun biter | Ücretli (2,99 USD), reklamsız | 2021 Çin tarayıcı oyunundan türedi; 2023'te VTuber ve Twitch yayıncılarıyla viral oldu, satışlar 50.000 kat arttı |
| Fluffy Drop | UniRise (Hong Kong) | Pofuduk topları aynı renk bölgeye sürükle, tahtayı temizle | Reklam + IAP | 6 ayda 1M+ indirme; senior ve çocuk kitlesi |
| Pixel Flow | Loom Games (İstanbul, yaklaşık 20 kişi) | Sıradaki domuzlar renkli top atar, aynı renk piksel küpleri temizlenir | Hybrid (IAP + reklam) | İlk yılda 198,4 milyon USD oyuncu harcaması, 10M+ oyuncu, Scopely 1 milyar USD+ değerleme |
| Color Block Jam | Rollic (İstanbul) | Renkli blokları aynı renk çıkışa kaydır, süre sınırlı | Hybrid, agresif UA | Tek çeyrekte 42 milyon USD; Block alt türünün yaklaşık %75'i |
| Magic Sort | Grand Games (İstanbul) | Renk sıralama | Hybrid | 2025'te 50 milyon USD; Android CPI 0,50 USD |
| Screwdom | iKame (Vietnam) | 3D vida sökme, sınırlı vida yuvası | Hybrid, yüksek sürtünme | Screw alt türünün yaklaşık %45'i; ABD D30 tutma yaklaşık %17 |
| Block Blast! | Hungry Studio | Tetris tarzı blok yerleştirme | Yalnızca reklam | Ocak–Mayıs 2026 en çok reklam geliri: 127 milyon USD |

**Alt tür dinamikleri (2025, Naavik / Sensor Tower)**

| Alt tür | 2025 geliri | Yıllık büyüme | Pazar yapısı | Yeni giren için anlamı |
| --- | --- | --- | --- | --- |
| Match Swap (Match-3) | ≈5,3 milyar USD | %1 | Royal Match ve Candy Crush'ın elinde; 120 yeni oyundan 1'i ayda 100K USD geçiyor | Girilmez |
| Merge-2 | ≈1,7 milyar USD | %95 | Gossip Harbor, Tasty Travels; ağır meta ve LiveOps | Solo için fazla büyük |
| Sort | ≈280 milyon USD | %176 | "Demokratik orta sınıf": lider %18, bir düzine oyun 5–15 milyon USD arası | En açık kapı; oyuncular çeşitlilik arıyor |
| Screw | ≈206 milyon USD | ≈%100 | Screwdom %45; 3D geçiş ve kıtlık mekaniği ile yüksek tutma | Mekanik yenilik şart, kopya tutmaz |
| Block | ≈183 milyon USD | %2.900 | Color Block Jam tek başına %75; H1 2026'da 2.000+ yeni block oyunu çıktı | Klon mezarlığı; kaçınılmalı |

**Ortak başarı formülü**

1. Ekran görüntüsünden anlaşılan mekanik: Suika'nın meyveleri, Pixel Flow'un domuzları, Fluffy Drop'un pofudukları. Mağaza sayfasında ve 3 saniyelik videoda kendini anlatır.
2. Dokunsal tatmin: fizik, ASMR ses, yumuşak animasyon. Bu, izlenebilirlik demek; Suika yayıncılar sayesinde, Fluffy Drop walkthrough videolarıyla büyüdü.
3. Kasıtlı sürtünme: Screwdom'un 5 vida yuvası, Color Block Jam'in süresi, Sort'un seviye darboğazları; oyuncu reklam izler ya da booster alır. Fluffy Drop bunu beceremiyor (coin'in harcanacak yeri yok).
4. Anahtar kelime konumlandırması: "No Timer", "Relax", "Offline", "for Seniors" doğrudan isimde. Paxie Games (Türkiye) aynı taktikle "Relaxing puzzles for seniors" alt başlığıyla Jigsaw Gallery'yi çıkardı.
5. Hızlı iterasyon: haftalık sürüm, 70+ prototip (Loom kurucuları önceki stüdyolarında 70 prototip yapmış).

**Tutma açığı = fırsat**

Sort (yaklaşık %8) ve Block (yaklaşık %4) D30 tutma oranları Royal Match ve Gossip Harbor gibi "sonsuz" türlerin çok altında; Screwdom %17 ile ara bir yerde. Yeni alt türlerin henüz çözemediği problem uzun vadeli tutma; koleksiyon, günlük ritüel ve hafif anlatı katmanı ekleyen küçük bir oyun bu boşlukta fark yaratabilir.

Türkiye avantajı: Rollic, Grand Games, Loom ve Unico (1 milyar+ indirme, İstanbul/Ankara) hybrid-casual'ın küresel merkezlerinden; yerel yayıncı, mentor ve işe alım ağı elinin altında.

## 2026 pazarı: indirmeler düşüyor, harcama hybrid-casual'a kayıyor

Pazar indirme bazında küçülüyor ama hybrid-casual bulmaca tek büyüyen segment; solo bir geliştirici için fırsat ucuz indirme değil, niş bir kitlede tutulan ve hibrit kazanan küçük bir oyun.

Uygulama içi harcamada yıllık değişim, H1 2026 / H1 2025 (Sensor Tower H1 2026 Digital Gaming Market Index, Cinevva derlemesi):

| Segment | Yıllık değişim |
| --- | --- |
| Hybrid-casual | +%23 |
| Puzzle (IAP) | +%20 |
| Casual | %0 |
| Mobil oyun toplamı | −%2 |
| Mid-core | −%7 |

H1 2026'da mobil oyun harcaması yaklaşık 40 milyar USD ile %2 geriledi, indirmeler %12 düşerek 24 milyara indi; aynı dönemde hybrid-casual 2,4 milyar USD ile %23 büyüdü.

**Rakamlar**

- Q1 2026: 20,4 milyar USD harcama (+%0,5), indirme −%11,9; puzzle geliri +%21 (Sensor Tower).
- H1 2026: puzzle IAP 8,07 milyar USD (+%20); Block Blast! Ocak–Mayıs'ta 127 milyon USD ile en çok reklam geliri; hybrid-casual başlıkların D7 tutması, top-25 casual'ı geçti (casual ortalaması %14,9, Aralık 2025).
- AppMagic H1 2026: casual IAP 11 milyar USD (yatay); puzzle %44 (4,9 milyar USD); Merge-2 +%74 (1,3 milyar USD); sort +%229; 6 ayda 2.000+ block puzzle çıktı (+%89); 120 yeni match-3'ten yalnızca 1'i ayda 100 bin USD geçiyor.
- Reklam: 2025'te 12 milyar USD+; puzzle, tür reklam gelirinin %53'ü. ABD'de gelirin %22'si reklam, Hindistan'da %70. ABD eCPM: ödüllü video yaklaşık 15 USD, interstitial 12–13 USD, banner 0,50 USD. Android reklam gelirinin %57'sini taşıyor.
- Bölge: 2025'te casual IAP Avrupa'da +%14, Kuzey Amerika'da +%9, Asya'da −%3; para Batı'da.
- Mağaza kuralları: ABD'de Apple dış ödeme linkleri komisyonsuz (2026 ortası itibarıyla); AB'de Ocak 2026'dan beri %5 Core Technology Commission; Google Play'de 22 Temmuz 2026'dan itibaren ABD'de alternatif mağazalar.
- Organik keşif: Apple Custom Product Page sayısı 35'ten 70'e çıktı ve organik aramada sıralanıyor; indirmelerin yaklaşık %65'i mağaza aramasından geliyor (Game Growth Advisor).
- Üretim: geliştiricilerin %52'si üretken AI kullanan şirkette; prototip maliyeti düştü, klon hızı da arttı.

**Solo geliştirici için beş çıkarım**

1. Block alt türüne girme: 6 ayda 2.000+ klon, tek kazanan. Sort'un "orta sınıfı" ve Screw'un tutma dersi daha açık kapı.
2. Niş kitle + anahtar kelime: "no timer", "offline", "seniors", "cozy" doğrudan isimde. UniRise'ın tarifi UA bütçesi gerektirmiyor.
3. İzlenebilirlik bedava UA: Suika'yı yayıncılar, Fluffy Drop'u walkthrough videoları büyüttü. Oyun kaybederken bile izlemesi keyifli olmalı.
4. Hibrit ama saygılı monetizasyon: ödüllü video + küçük IAP paketleri. Fluffy Drop'un işlevsiz coin'i ve çalışmayan No Ads'i 1 yıldızların ana kaynağı; aynı hatayı yapmamak başlı başına fark.
5. Tutma açığını hedefle: yeni alt türlerde D30 %4–8. Günlük ritüel, koleksiyon ve "yarın ne olacak" kancası ekleyen oyun, aynı mekanikle daha yüksek LTV üretir.

## Özgün oyun fikirleri

Sekiz fikir; hepsi tek ekran görüntüsünde anlaşılır, süresiz/cozy konumlanır, hibrit kazanır ve solo bir geliştiricinin 2–6 haftada MVP'ye götürebileceği ölçektedir. İlk ikisi (Ink Bloom, Daily Drop) yeni bir core mekanik ya da yeni bir ritüel getirir; diğerleri kanıtlanmış bir mekaniğe tek bir güçlü bükülme ekler.

**1. Ink Bloom (Mürekkep Çiçeği) — renk karıştıran damla bulmacası**

- Konsept: Islak kağıda renkli mürekkep damlaları bırakırsın; damla dairesel yayılır, komşu damlayla karışır (kırmızı + mavi = mor, sarı + mavi = yeşil). Her seviyede bir hedef desen var: bölgeleri doğru renkle doldur, dışına taşırma.
- Core loop: damla seç → konum ve boyut (basılı tutma süresi) → yayılma ve karışma animasyonu → hedefle karşılaştırma, yıldız → ortaya çıkan eser galeriye eklenir.
- Özgün kanca: Renk karışımı kuralı hiçbir büyük casual oyunda core mekanik değil. Her seviyenin çıktısı paylaşılabilir bir görsel; renk teorisi öğrettiği için eğitim/okul nişine de girer.
- Monetizasyon: Ödüllü video (ek damla, kurutma kağıdı, silgi), No Ads, sezonluk galeri temaları, eseri yüksek çözünürlükte indirme (küçük IAP).
- Kitle ve ASO: "relaxing color puzzle", "art puzzle", "no timer"; 25–55 yaş, sanat ve cozy kitlesi, kadın ağırlıklı.
- Solo süre: 5–6 hafta MVP. Risk: renk karışımı sezgisel değilse zorluk eğrisi kırılır; ilk 10 seviye tutorial olmalı.

**2. Daily Drop (Günün Damlası) — herkese aynı sıra, tek deneme, paylaşılabilir kart**

- Konsept: Suika tarzı bırak-birleştir; ama her gün dünya genelinde aynı top sırası ve tek deneme hakkı. Sonuç, spoiler vermeyen kartla paylaşılır (Wordle modeli). Arkadaş ligleri asenkron.
- Core loop: Aç → günün sırasını oyna (3–5 dk) → sonuç kartı → paylaş → yarın tekrar.
- Özgün kanca: Suika'nın izlenebilirliğini Wordle'ın ritüeliyle birleştirir. Sıra aynı olduğu için karşılaştırma adil; yayıncılar günün seed'ini canlı oynar.
- Monetizasyon: Ödüllü video ile bir tekrar hakkı; Premium abonelik (arşiv, sınırsız pratik modu, istatistik); kozmetik top setleri.
- Kitle ve ASO: "daily puzzle", "watermelon game", "drop merge"; 18–45, küresel; TikTok ve Twitch.
- Solo süre: 2–3 hafta MVP. Risk: klon algısı; isim ve sunum farklı olmalı.

**3. Yumak (Yarn Drop & Knit) — birleştirme üretime dönüşür**

- Konsept: Yün yumaklarını sepete bırak, aynı renkler birleşip büyüsün; tam boy yumak sepetten çıkıp örgüye dönüşür, her seviye bir atkı ya da kazak satırı örer. Biten giysiler dolapta koleksiyon olur, karakter giydirilir.
- Özgün kanca: Merge'in çıktısı kaybolmak değil üretmek; her oturumun kalıcı izi var, bu tutma açığını doğrudan hedefler. "Knitting" ve "yarn" ASO nişi.
- Monetizasyon: Ödüllü video (ek yumak, sepet genişletme), desen paketleri, No Ads, sezon dolabı.
- Kitle ve ASO: "cozy knitting game", "yarn merge", "relaxing"; 35+ kadın ve senior.
- Solo süre: 4–5 hafta. Risk: giysi görselleri için sanat yükü; prosedürel ilmek grid'i şart.

**4. Nota Sort — renk yerine ses sıralama**

- Konsept: Her şişe bir nota; aynı notaları aynı şişede topla. Şişe dolunca akor çalar, seviye bitince kısa bir melodi (kamu malı klasikler) tamamlanır.
- Özgün kanca: Sort'un düzen tatminini işitsel ödüle bağlar; renk körü oyuncular için erişilebilir; ASMR.
- Monetizasyon: Ödüllü video (ekstra şişe, geri alma), enstrüman paketleri, No Ads.
- Kitle ve ASO: "music puzzle", "relaxing sort", "ear training"; 25–60.
- Solo süre: 3 hafta. Risk: mobil oyuncuların çoğu sessiz oynar; renk ve şekil kodlaması yedek kanal olarak şart.

**5. Büyük ve Sakin serisi — 60+ için portföy, ilk oyun Aile Yapbozu**

- Konsept: 60+ için tasarlanmış seri: dev dokunma alanları, yüksek kontrast, sesli yönlendirme, süresiz. İlk uygulama Aile Yapbozu: çocuklar ve torunlar fotoğraf yollar, büyükanne onları yapboz olarak çözer, bitince gönderenin sesli mesajını dinler.
- Özgün kanca: Oyun tek kişilik ama bağ ailede; fotoğrafı yükleyen kişi (çocuk) ödeme yapan kişi. Puzzles for Seniors (8,3K yorum, 4,7 puan) talebi kanıtlıyor.
- Monetizasyon: Aile aboneliği (aylık), minimal reklam.
- Seri mantığı: Aynı motorla Büyük Sudoku, Büyük Kelime Avı (Türkçe), Büyük Solitaire; 25 uygulama hedefine en uygun hat.
- Solo süre: 3 hafta ilk uygulama. Risk: fotoğraf paylaşımı için backend, KVKK ve GDPR.

**6. Günün Okeyi — tek kişilik okey bulmacası (Türkiye nişi)**

- Konsept: Verilen 14 taş ve gösterge ile en yüksek puanlı diziyi kur. Günlük "aynı taşlar herkese" yarışması ve sonsuz pratik modu. Sonra 101 Okey ve tavla bulmacası varyantları.
- Özgün kanca: Mevcut okey uygulamaları çok oyunculu ve kumar hissiyatlı; bulmaca formatı yok. Türkiye + diaspora (Almanya, Hollanda), 45+.
- Monetizasyon: Reklam + No Ads, taş seti kozmetikleri, aylık arşiv.
- Solo süre: 2–3 hafta. Risk: pazar sınırlı ve ARPU düşük; ama içerik maliyeti sıfır, portföyde düzenli nakit akışı oyunu.

**7. Eğik Kavanoz (Tilt Jar) — kabı yatırmak ikinci kaynak**

- Konsept: Suika'nın kabı sabit değil: oyuncu seviyede iki "yatırma" hakkı harcar, kavanoz 15 derece yana yatar, fizik yeniden akar, zincir birleşmeler patlar. Tema akvaryum.
- Özgün kanca: Suika'da kontrol edilen tek şey düşüş noktası; burada yatırma stratejik bir kaynak. İzlenebilirlik anı: yatırmadaki çığ.
- Monetizasyon: Ödüllü video (ek yatırma, geri alma), tema setleri, No Ads.
- Kitle ve ASO: "watermelon game", "physics merge", "aquarium"; 16–35, Twitch ve TikTok.
- Solo süre: 3 hafta. Risk: klon algısı; akvaryum estetiği ve ses tasarımı ile ayrışmalı.

**8. Tohum (Bloom Drop) — Fluffy Drop'un dürüst versiyonu**

- Konsept: Renkli tohum toplarını aynı renk çiçek tarhına sürükle, tahta temizlenince tarh çiçek açar. Seviyeler bir bahçe haritasında ilerler ve bahçe kalıcı olarak büyür.
- Özgün kanca: Mekanik değil, sözleşme: gerçekten süresiz, seviye arası zorunlu reklam yok (ödüllü + günde en fazla üç interstitial), coin'in gerçek mağazası var, büyük yazı, ana menü düğmesi.
- Monetizasyon: Ödüllü video, No Ads (4,99 USD), bahçe dekor paketleri.
- Kitle ve ASO: "no timer", "no ads between levels", "relaxing"; senior ve çocuk.
- Solo süre: 4 hafta. Risk: farklılaşma zayıf; ama Sort benzeri "demokratik" pazar hızlı takipçiyi ödüllendiriyor.

Tüm fikirlerin ortak altyapısı: tek bir "cozy motor" (Unity/C#): meta harita, koleksiyon, ödüllü video ve IAP katmanı, 15 dil, büyük yazı modu, analitik. Her oyun bu motorun üstüne bir core mekanik ve tema ekler; 25 oyun hedefi ancak bu şekilde solo yürür.

## Fikirlerin karşılaştırmalı değerlendirmesi

Önerilen sıra: Daily Drop (2–3 haftada çıkar, ritüeli ve paylaşımı test eder), ardından Ink Bloom (yeni core mekanik, en yüksek tavan), paralelde Büyük ve Sakin serisi (motoru amorti eden düzenli hat).

Solo MVP süresi × organik büyüme potansiyeli (1–5, Claude değerlendirmesi):

| Fikir | Solo MVP süresi | Potansiyel | Çeyrek |
| --- | --- | --- | --- |
| Daily Drop | 2–3 hafta | 4,2 | Hemen başla |
| Büyük ve Sakin (Aile Yapbozu) | 3 hafta | 3,7 | Hemen başla |
| Ink Bloom | 5–6 hafta | 4,5 | Büyük bahis |
| Yumak | 4–5 hafta | 3,6 | Büyük bahis |
| Eğik Kavanoz | 3 hafta | 3,3 | Portföy dolgusu |
| Nota Sort | 3 hafta | 3,0 | Portföy dolgusu |
| Günün Okeyi | 2–3 hafta | 2,4 | Portföy dolgusu |
| Tohum | 4 hafta | 2,8 | Sonraya bırak |

| Fikir | Ana gelir modeli | Tutma kancası | En büyük risk | İlk 3 ay için rol |
| --- | --- | --- | --- | --- |
| Daily Drop | Ödüllü video + Premium abonelik | Günlük aynı seed, paylaşım kartı | Klon algısı | İlk çıkış: 3 haftada mağazada, sinyal toplar |
| Ink Bloom | Ödüllü video + No Ads + galeri temaları | Eser galerisi, paylaşılabilir görsel | Zorluk eğrisi | İkinci çıkış: ana bahis |
| Büyük ve Sakin (Aile Yapbozu) | Aile aboneliği | Aileden gelen içerik | KVKK/GDPR, backend | Seri temeli; her ay bir uygulama |
| Yumak | Ödüllü video + desen paketleri | Dolap koleksiyonu | Sanat yükü | Motor oturunca, 4. ay |
| Eğik Kavanoz | Ödüllü video + temalar | Yüksek skor, klip paylaşımı | Klon algısı | Daily Drop'un fizik kodunu yeniden kullanır |
| Nota Sort | Ödüllü video + enstrüman paketleri | Melodi koleksiyonu | Sessiz oynayanlar | Sort motoru ile 3 haftalık dolgu |
| Tohum | Ödüllü video + No Ads | Kalıcı bahçe | Zayıf farklılaşma | "No ads between levels" konumlanması testi |
| Günün Okeyi | Reklam + No Ads | Günlük yarışma | Küçük pazar | Türkiye nakit akışı, 2 haftalık iş |

## Tasarım taslakları

Tüm ekranlar 390×844 telefon artboard'ları olarak tasarım kanvasında; her oyunun reposunda `docs/design/` altında ilgili `.dc.html` kaynakları bulunur.

| Ekran | Ne gösteriyor | Tasarım kararı |
| --- | --- | --- |
| Daily Drop · Oyun | Günün numarası, "herkes aynı sırayı oynuyor" satırı, 1 deneme rozeti, kavanozda yığılmış 8 kademeli toplar, taşma çizgisi, sıradaki 3 top | Gece laciverti zemin, Nunito; toplar düz renk + tek vurgu gölgesi; UI minimum, kavanoz ekranın %55'i |
| Daily Drop · Sonuç | Skor, "47. düşüşte taştın", spoiler vermeyen 16 kareli özet, seri/en büyük/ortalama, Paylaş (birincil), reklamla ek deneme (ikincil), Premium (metin), arkadaş sıralaması | Paylaş düğmesi tek sarı vurgu; ödüllü video ikinci sırada, asla zorunlu değil |
| Ink Bloom · Oyun | Islak kağıt, iki mürekkep damlasının üst üste gelip mor üretmesi, kesikli hedef bölgeleri, doğruluk çubuğu, 3 mürekkep kuyusu ve kalan damla sayısı | Kağıt tonu zemin, Fraunces serif başlık; renk karışımı ekranın kendisinde öğretiliyor |
| Ink Bloom · Galeri | Son eser büyük kart, 2 sütun eser ızgarası, kilitli sonraki seviye, sezon teması kartı | Her eser oyuncunun ürettiği görsel |
| Aile Yapbozu · 60+ | "Günaydın, Ayşe Hanım", gönderenin avatarı, 3×3 büyük parça, boş yuvalarda "buraya", 100 px parça tepsisi, 64 px İPUCU düğmesi, bitince açılan sesli mesaj, "Süre yok · Reklam yok" satırı | Atkinson Hyperlegible, 4,5:1 üstü kontrast, dokunma hedefleri 88–100 px, tek vurgu rengi |
| Aile Yapbozu · Gönder | Çocuk tarafı: fotoğraf, zorluk seçimi, annenin son süresine göre öneri, 12 sn sesli mesaj, küçük not, GÖNDER, Aile Planı kartı | Ödeme yapan taraf bu ekran |
| Yumak · Oyun | Sepete düşen yumaklar, olgunlaşan yumağın "örgüye gidiyor" etiketi, üstte 10 sıralık atkı ilerlemesi, Dolap düğmesi, reklamla sepet genişletme | Yün kremi zemin; meta (örgü) oyun ekranında sürekli görünür |
| Nota Sort · Oyun | 5 şişe, her segmentte nota adı, seçili şişeden kaldırılmış Re, melodi ilerlemesi 3/5, enstrüman seçici, sessiz mod notu | Renk + nota adı çift kodlama (renk körü ve sessiz oyuncu) |
| Günün Okeyi · Oyun | Gösterge taşı, okey ve sahte okey satırı, bulunan diziler, ahşap ıstakada 14 taş, puan 41 / günün en iyisi 56, "Diziyi gönder" | Çuha yeşili ve ahşap, gerçek okey renkleri, 58 px taşlar; tek gönderim ritüeli |
| Eğik Kavanoz · Oyun | 12 derece yatık akvaryum kavanozu, köşeye kayan balıklar, "Zincir ×3" rozeti, yatırma hakkı 1/2 ve iki yön düğmesi | Derin mavi zemin; yatırma kontrolü başparmak hizasında |
| Tohum · Oyun | Dürüstlük şeridi ("Süre yok · Bugün reklam 1/3 · Seviye arası reklam yok"), sürüklenen tohum, üç renkli tarh, coin fiyatlı boosterlar, "Bahçem 12/40" | Fluffy Drop'un her 1 yıldız nedeni burada özellik |

## Önerilen yol haritası

İlk 12 haftada önce mevcut Block Blast tarzı oyundan ortak "cozy motor" çıkarılır, sonra en ucuz sinyali veren Daily Drop mağazaya konur; Ink Bloom'a ancak motor iki oyunda çalışıp Daily Drop paylaşım oranı ölçüldükten sonra başlanır.

| Faz | Hafta | İçerik | Çıkış kapısı |
| --- | --- | --- | --- |
| Cozy motor | 0–1 | Reklam, IAP, meta iskeleti, analitik, 15 dil | Motor 2 oyunda çalışıyor |
| Daily Drop | 1–3 | Soft launch (TR, PH, CA) | D1 ≥ %35, paylaşım ≥ %5 |
| Aile Yapbozu | 3–5 | Davet, backend, aile aboneliği | Davet kabulü ≥ %40 |
| Ink Bloom | 5–10 | 60 seviye, galeri | D7 ≥ %15, 10. seviye ≥ %50 |
| Dolgu oyunlar | 10–12 | Günün Okeyi, Nota Sort; ASO döngüsü | — |

Her kapı bir sayfalık karar: eşik tutmuyorsa sonraki faza geçmeden önce iki hafta iterasyon ya da fikri rafa kaldırma. Casual D1 ortalaması %30–40, D7 %14,9 (Aralık 2025, Sensor Tower); eşikler bu ortalamaların biraz üstünde.

**Cozy motor (hafta 0–1)**: mevcut oyundan ayrıştır: ödüllü video + interstitial kotası (günde en fazla 3), IAP katalogları, No Ads bayrağı, 15 dil, büyük yazı modu, meta harita ve koleksiyon ekranı, analitik olayları. Mimari: Unity/C# ortak paket (UPM), her oyun ayrı repo; seviye verisi JSON, seviye editörü ortak. Reklam mediation tek entegrasyon; Android önce.

**Ortak motor kararı (14 Ekim 2026, kod envanteri sonrası)**: MustyBlockBlast'ın 30 sistemlik envanteri çıkarıldı (`docs/core-inventory.md`, monorepo). Motor **yeni repo değil**, mevcut monorepo `mtafasahin-unity-packages`'ta iki yeni paket olur ve oyunlar git URL + tag ile tüketir (MustyBlockBlast'ın `com.mtafasahin.reactive` ve `com.mtafasahin.mobileservices`'i tükettiği mekanizma):

- `com.mtafasahin.cozycore` ("CozyCore", epic + 35 story): MBB'den kopyalanan, oyun bilmeyen 10 modül — Abstractions, Ads (AdMob + UMP; ödüllü, interstitial kotası), Iap (Unity IAP 5 + restore, No Ads), Ugs (auth, Remote Config, leaderboard, hesap), Save (PlayerPrefs + Cloud Save çakışma kararı), Localization (15 dil), Settings (Reduce Motion, büyük yazı, haptik), Ui (ilk kez prefab: Ayarlar, consent, ipucu, tutorial eli), Meta (cüzdan, envanter, can, günlük hediye, rozet), Analytics. Fazlar: v0.1 (Daily Drop'u açar) → v0.2 (reklam + IAP) → v0.3 (bulut + lider tablosu) → v0.4 (meta) → v0.5 (bildirim, makbuz doğrulama, MAX adaptörü).
- `com.mtafasahin.mergephysics` ("MergePhysics", epic + 18 story): bırak-birleştir fizik çekirdeği; MBB'de fizik olmadığı için sıfırdan, Daily Drop'un içinde doğar (story 2–5), soft launch build'inden sonra pakete çıkar; Yumak ve Eğik Kavanoz aynı tag'i tüketir. v0.1 (çıkarma, Daily Drop regresyonu) → v0.2 (kinematik kavanoz, kapsül gövde, replay) → v0.3 (performans bütçesi).

İki düzeltme bu envanterden çıktı: hedef sürüm **Unity 6 (6000.6.x)** (MBB ve paketler bunu ister; "2022 LTS" ifadeleri değiştirildi) ve reklam ağı **AdMob** (Google Mobile Ads 11.5 + UMP, MBB ile aynı; AppLovin MAX adaptörü eCPM verisine göre SONRA). Motorun gerçek takvimi: CozyCore v0.1 + v0.2 ≈ 20 story (çoğu M) ≈ 5–6 hafta solo; yani tablodaki "0–1 hafta" iyimser, Daily Drop motorla paralel yürür ve Daily Drop 1. story CozyCore v0.1'i bekler.

**Daily Drop MVP (hafta 1–3)**: seed'li deterministik sıra, 8 kademeli top seti, günlük sıra, paylaşım kartı (PNG + metin), arkadaş ligi için basit backend, ödüllü video ile ek deneme, Premium abonelik iskeleti. Soft launch: Türkiye, Filipinler ve Kanada'da Android; 2 hafta veri; ölçüm: D1, paylaşım oranı, ortalama oturum.

**Aile Yapbozu MVP (hafta 3–5)**: davet linki, fotoğraf yükleme, 9/16/25 parça, sesli mesaj, 60+ arayüz; aile aboneliği. KVKK/GDPR: şifreli depolama, süreli silme, rıza ekranı.

**Ink Bloom (hafta 5–10)**: renk karışımı modeli, damla boyutu, 60 el yapımı seviye + prosedürel üretici, galeri, paylaşım. İlk 10 seviye tutorial; zorluk ayarı için seviye başına deneme sayısı ve terk noktası izlenir.

**Dolgu oyunlar ve ASO (hafta 10–12)**: Günün Okeyi ve Nota Sort 2'şer haftalık; ASO döngüsü: her oyunda 3 Custom Product Page (cozy, senior, daily), başlık anahtar kelime testi, ekran görüntüsü A/B.

**25 oyun hedefi için kural**: motor oturduktan sonra ritim 2–3 haftada bir oyun; her 5 oyunda bir "büyük bahis" (Ink Bloom ölçeğinde), aradakiler seri ve dolgu. Klon sayısı 6 ayda 2.000'i geçen Block alt türüne hiç girilmez; her yeni oyun ya yeni bir mekanik ya yeni bir ritüel ya da yeni bir kitle getirir.

## GitHub repoları ve planlama

Her oyun kendi private reposunda; her repoda 1 epic + story'ler (Gherkin kabul kriterleri + teknik görevler), `docs/` altında bu rapor, oyun tasarım dokümanı, tasarım ekranları ve ajan brief'i (`CLAUDE.md`). Ortak paketler monorepo'da.

| Repo | Oyun | Epic + story | Paket / bağımlılık |
| --- | --- | --- | --- |
| `mustafa-sahin-tr/mtafasahin-unity-packages` | — (monorepo) | CozyCore #1 + 35 story (#2–#36) · MergePhysics #37 + 18 story (#38–#55) | `com.mtafasahin.cozycore`, `com.mtafasahin.mergephysics` (+ mevcut `reactive`, `mobileservices`) |
| `MustyDailyDrop` | Daily Drop | #1 + 27 | CozyCore v0.1 → v0.2; MergePhysics burada doğar |
| `MustyInkBloom` | Ink Bloom | #1 + 24 | CozyCore v0.2 |
| `MustyAileYapbozu` | Aile Yapbozu | #1 + 26 | CozyCore v0.1 (reklamsız) + Firebase |
| `MustyYumak` | Yumak | #1 + 24 | CozyCore v0.2 + MergePhysics v0.1 |
| `MustyNotaSort` | Nota Sort | #1 + 24 | CozyCore v0.2 |
| `MustyGununOkeyi` | Günün Okeyi | #1 + 23 | CozyCore v0.3 (lider tablosu) |
| `MustyEgikKavanoz` | Eğik Kavanoz | #1 + 26 | CozyCore v0.2 + MergePhysics v0.2 |
| `MustyTohum` | Tohum | #1 + 24 | CozyCore v0.4 (meta) |

Toplam: 10 epic, 251 story. Story kaynakları `docs/issues/<oyun>.md` (format `STYLE.md`); issue'lar script ile üretildi ve senkronlanır (`create_issues.py`, `sync_issues.py`). Etkileşimli yol haritası: monorepo `docs/roadmap/` (kapasite ve paralellik ayarlı takvim; canlı sürümün linki aynı klasördeki README'de).

## Kaynaklar

Açılan sayfalar, 14 Ekim 2026 itibarıyla.

- Fluffy Drop - No Timer, App Store UK: https://apps.apple.com/gb/app/fluffy-drop-no-timer/id6762342518 — sürüm geçmişi, IAP fiyatları, yorumlar, yayıncı portföyü
- Fluffy Drop - No Timer, Google Play: https://play.google.com/store/apps/details?id=com.fluffydrop.op.gp&hl=en_AU — indirme, puan, kategori sıralaması, yorumlar
- Fluffy Drop, App Store US: https://apps.apple.com/us/app/fluffy-drop-no-timer/id6762342518 — ABD puan ve sıralama
- AppBrain: Fluffy Drop: https://www.appbrain.com/app/fluffy-drop-no-timer/com.fluffydrop.op.gp — trivia sıralamaları
- Marlvel: UniRise Studio yayıncı profili: https://marlvel.ai/api/llm/publishers/unirise-studio/overview — portföy ve sürüm sıklığı
- Naavik: How Niche Subgenres are Reshaping the Mobile Puzzle Market: https://naavik.co/digest/how-niche-subgenres-are-reshaping-the-mobile-puzzle-market/ — Sort/Screw/Block gelir, büyüme, tutma
- Cinevva: Casual Games Trends in 2026: https://app.cinevva.com/guides/casual-games-trends-2026 — Sensor Tower, AppMagic ve GameRefinery derlemesi; H1 2026 rakamları, reklam eCPM, mağaza kuralları
- Mobidictum / AppMagic: Casual Games Report H1 2026: https://mobidictum.com/casual-games-mobile-marketing-report-h1-2026/ — puzzle payı, merge büyümesi
- PocketGamer.biz: Pixel Flow ilk yıl 198,4 milyon USD: https://www.pocketgamer.biz/loom-games-pixel-flow-makes-almost-200m-in-player-spending-during-first-year/
- PocketGamer.biz: Scopely, Loom Games çoğunluk hissesi: https://www.pocketgamer.biz/scopely-acquires-majority-stake-in-istanbuls-pixel-flow-developer-loom-games
- Wikipedia: Suika Game: https://en.wikipedia.org/wiki/Suika_Game — köken ve viral dönem
- Game Growth Advisor: ASO for Mobile Games 2026: https://gamegrowthadvisor.com/blog/2026-04-28-aso-app-store-optimization-mobile-games-guide/ — Custom Product Page ve organik pay
- App Store: Jigsaw Gallery (Paxie Games): https://apps.apple.com/us/app/-/id6754933484 — "for seniors" konumlandırma örneği
- Marlvel: Puzzles for Seniors: https://marlvel.ai/apps/puzzles-for-seniors — senior nişi talep göstergesi
