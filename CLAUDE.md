# Yumak — ajan brief'i (CLAUDE.md)

Bu repo, Mustafa'nın mobil oyun portföyünün 4. oyunu: **Yumak** — yün yumaklarını sepete bırak, birleştir; olgun yumak örgüye gider, giysi dolaba girer.
Mağaza adı: Yumak: Cozy Yarn Merge / Yumak: Örgü ve Yün Bulmacası. Portföydeki rolü: Motor oturunca (4–5 hafta): Daily Drop'un fizik çekirdeğini (MustyMergePhysics) yeniden kullanır.

## Önce oku (sırayla)
1. `docs/game-design.md` — oyunun kuralları, v1 kararları, kapsam dışı, başarı kapıları (GitHub'daki epic issue ile aynı metin).
2. `docs/arastirma-raporu.md` — pazar araştırması: Fluffy Drop analizi, tür analizi, 2026 pazarı, 8 fikir, yol haritası, kaynaklar.
3. `docs/issues/yumak.md` — epic + 24 story'nin kaynağı; GitHub issue'ları bu dosyadan üretildi (başlıklar birebir).
4. `docs/design/` — tasarım ekranlarının kaynağı (`.dc.html`, 390×844 telefon artboard'ları). Canlı kanvas: https://claude.ai/artifact/3VMA4d5qTn8GF89PDf5Uj9
5. Canlı araştırma raporu (yorum/düzenleme): https://claude.ai/code/artifact/8ab0af47-8a63-4fac-8fd7-aad383f2fe81

## Roller
- **Product owner:** epic'teki sırayı korur; her story başlamadan refine eder (gerekirse mockup, onay, sonra kod); kabul kriterlerini günceller; kapı kararlarını (D1/D7/paylaşım) epic'e yorum olarak yazar; SONRA label'lı işleri canlı sonrasına erteler.
- **Analist:** `docs/arastirma-raporu.md` ve "Analitik olay şeması ve kapı raporu" story'sini temel alır; UGS Analytics olaylarını ve SQL raporlarını `docs/analytics.md` ve `docs/reports/` altında tutar; soft launch sonunda kapı raporunu üretir.
- **Developer:** story'yi alır; dal `story/<numara>-<kisa-ad>`; PR başlığı "<story başlığı> (#numara)"; PR gövdesinde kabul kriterleri işaretlenir ve "Closes #numara" yazılır; teknik görevler PR'ın checklist'idir.

## Kurallar
- Dil: issue, PR ve commit mesajları Türkçe; kod, sınıf ve dosya adları İngilizce; TR ve EN yerelleştirme birinci sınıf.
- Reklam ilkesi: seviye/tur arası zorunlu interstitial ya yok ya günde en fazla 3; ödüllü video her zaman oyuncunun seçimi; No Ads tüm interstitial'ı kapatır; oyun ekranında banner yok. Coin varsa harcanacak gerçek mağazası var. "Süre yok" vaadi verildiyse hiçbir seviyede sayaç yok.
- Erişilebilirlik: Reduce Motion tüm efektleri kapsar; büyük yazı modu; Android geri tuşu her ekranda doğru çalışır; ana menü düğmesi her ekranda.
- Eğitim: zorunlu, girdiyi kilitleyen tutorial yok; ipuçları olay tabanlı ve kapatılabilir.
- Efektler: yoğunluk artsın, süre uzamasın; sayılar (px, ms, puan) başlangıç değeridir, Remote Config'e bağlanır, Play modunda ayarlanır.
- Ortak paket: `MustyCozyCore` (UPM; reklam mediation, IAP, UGS, yerelleştirme, ayarlar, meta iskeleti, analitik). Henüz yoksa 1. story kapsamında MustyBlockBlast'tan ayrıştırılır; oyuna özgü kod pakete sızmaz.
- Teknoloji: Unity 2022 LTS+, C#, URP 2D, Unity Input System, UGS (Player Accounts, Cloud Save, Analytics, Remote Config, Leaderboards, Cloud Code), AppLovin MAX, Unity IAP, Unity Test Framework (Edit Mode + Play Mode). Çıkış: Android soft launch (3 ülke, 14 gün) → kapı → iOS.
- Her story bir PR; her PR en az bir test ya da gerekçeli "test yok" notu taşır; `docs/game-design.md` değişen karar varsa aynı PR'da güncellenir.

## Portföy
Diğer repolar: MustyDailyDrop, MustyInkBloom, MustyAileYapbozu, MustyNotaSort, MustyGununOkeyi, MustyEgikKavanoz, MustyTohum. Ortak paket: MustyCozyCore (ayrı repo). Yol haritası ve kapılar `docs/arastirma-raporu.md` "Önerilen yol haritası" bölümünde.
