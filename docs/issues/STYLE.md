# Portföy planları — yazım ve dosya formatı

Bu klasördeki her `<oyun>.md` dosyası bir GitHub reposuna issue olarak yazılır
(`create_issues.py`). Format katıdır; parser şu satırlara bakar.

## Dosya iskeleti

```
<!-- game: Daily Drop -->
<!-- repo: MustyDailyDrop -->
# EPIC: <epic başlığı>
labels: epic, enhancement

<epic gövdesi — markdown>

## STORY: <Oyun adı> <N>: <başlık>
labels: tema:<tema-slug>, P1, efor:M
milestone: MVP

<story gövdesi — markdown>

## STORY: ...
```

Kurallar:
- `# EPIC:` tam bir kez, dosyanın başında. `## STORY:` her story için bir kez. Başka `#`/`##` başlık kullanma;
  story gövdesi içinde `###` başlık kullan.
- `labels:` ve `milestone:` satırları başlığın hemen altında, boş satırdan önce.
- Milestone değerleri: `MVP` (ilk oynanabilir, soft launch'a giden), `Soft launch` (ölçüm ve iyileştirme), `Canlı` (dünya çapında çıkış ve LiveOps), `SONRA` (ertelenen; ayrıca `SONRA` label'ı).
- Öncelik label'ı: `P1` (MVP'siz olmaz), `P2` (soft launch'ta), `P3` (canlı sonrası).
- Efor label'ı: `efor:S` (≤ 1 gün), `efor:M` (2–3 gün), `efor:L` (4–6 gün). Solo, deneyimli Unity/C# geliştirici için.
- Tema label'ı slug: küçük harf, ASCII, tire: `tema:cekirdek`, `tema:ritual`, `tema:paylasim`, `tema:monetizasyon`, `tema:meta`, `tema:magaza`, `tema:erisilebilirlik`, `tema:analitik`, `tema:backend`.
- Story gövdesinde epic'e atıf `#EPIC` yazılır; script gerçek numarayla değiştirir.
- Epic gövdesindeki "Temalar ve story'ler" listesi story başlıklarını `- [ ] <tam story başlığı>` biçiminde sıralar; script her satıra `#numara` ekler.
- Dil: Türkçe. Terimler: story, epic, milestone, label İngilizce kalır; "seviye", "ödüllü video", "coin".
- `$` karakteri kullanma (USD yaz). Uzun tire (—) serbest.

## Epic gövdesi (sıra)

```
## Neden
## Hedef kitle ve konumlandırma
## Core loop ve kurallar (v1 kararları)
## Kapsam dışı (v1)
## Başarı ölçütleri (kapılar)
## Temalar ve story'ler (sıra)
**P1 · MVP**
- [ ] ...
**P2 · Soft launch**
- [ ] ...
**P3 · Canlı**
- [ ] ...
**SONRA**
- [ ] ...
## Riskler ve açık sorular
## Done when
```

## Story gövdesi (sıra)

```
**<Oyun>** epic'inin **<N>. story'si** (epic #EPIC). Tema: <tema adı>.

**Öncelik:** P1 · **Efor:** M · **Milestone:** MVP · **Dokunduğu yer:** <sistem/sınıf/ekran adları>

### Kullanıcı hikâyesi
Bir <rol> olarak <istek>, böylece <değer>.

### Kabul kriterleri
- [ ] **Senaryo: <ad>** · Verilen <...> · Eğer <...> · O zaman <...>
- [ ] ...

### Teknik görevler (Unity/C#)
- [ ] ...

### Analitik
- `event_name` {param: tip, ...} — ne zaman

### Notlar
- Karar / risk / açık soru / referans
```

Beklenen derinlik: her story 4–8 kabul kriteri, 4–10 teknik görev, 1–4 analitik olay.
Her oyun: 18–26 story (3–6 tanesi SONRA). Sayılar (px, ms, puan) başlangıç değeridir; "Play modunda ayarlanır" notu düşülür.

## Ortak teknik varsayımlar (tüm oyunlar)
- Unity 2022 LTS+ / C#, URP 2D. Tek ortak paket: `MustyCozyCore` (UPM): reklam mediation (AppLovin MAX veya LevelPlay; ödüllü video + interstitial kotası), IAP (Unity IAP), UGS (Player Accounts, Cloud Save, Analytics, Leaderboards, Remote Config), yerelleştirme (TR/EN + 13 dil tablosu), büyük yazı modu, Reduce Motion, haptik, ses yöneticisi, meta harita/koleksiyon iskeleti, günlük hediye, ayarlar ekranı, Android geri tuşu, mağaza içi değerlendirme isteği.
- Oyuna özgü story'ler bu paketi "kullanır"; paketi kurmak ayrı bir story'dir ("Core paketini bağla").
- Analitik: UGS Analytics custom events; her olay `snake_case`, parametreler tipli.
- Reklam ilkesi: seviye/tur sonu zorunlu interstitial yok ya da günde en fazla 3; ödüllü video her zaman oyuncunun seçimi. No Ads satın alımı tüm interstitial'ı kapatır, ödüllü videoyu kapatmaz.
- Yerelleştirme: TR ve EN birinci sınıf; diğer diller tablo çevirisi.
- Çıkış: Android önce (soft launch 3 ülke), iOS sonra.
