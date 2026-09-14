# Symulator nieba

> Podejście: **uproszczony symulator, nie turbo-rzeczywisty.**
> Gwiazdozbiory = prawdziwe, obiekty = prawdziwe, ale całość uproszczona.
> Na razie: **północna półkula** (niebo północne).

## Obecny prosty stan (Etap 0)

**Zakres pierwszego prototypu — i tylko tyle:**
- nieruchome gwiazdy jako białe punkty
- stały kolor nieba (bez dynamicznego Bortle’a)
- jedna lokalizacja
- swobodne chodzenie w 3D

### Opcje techniczne (do wyboru)

**Niebo (stały ciemny kolor):**
- **Opcja A (najprostsza):** `WorldEnvironment` + `ProceduralSkyMaterial` z ciemnym kolorem (`sky_top_color`, `sky_horizon_color`, `ground_bottom_color`).
- **Opcja B (więcej kontroli):** custom skydome (duża sfera) z spatial shaderem.

**Gwiazdy (białe punkty):**
- **Opcja A (najprostsza):** `MultiMeshInstance3D` — 1000–3000 małych białych quadów/sfer rozłożonych na dużej sferze. Jeden draw call.
- **Opcja B (przyszłościowo):** shader na skydome z proceduralnymi gwiazdami (hash) — łatwiej później dodać rotację i „świtanie" Bortle’a.

**Rekomendacja:** dla Etapu 0 → **Opcja A** (szybko, natively w Godocie). Przejście na Opcję B przy dodawaniu ruchu nieba / Bortle’a.

> ⚠️ Promień sfery gwiazd musi być **mniejszy niż `far` kamera** (domyślnie 4000).

### Drzewo sceny (Etap 0)

```
Prototype (Node3D)
├── WorldEnvironment     (Environment + ProceduralSkyMaterial, ciemny)
├── Stars                (MultiMeshInstance3D, białe punkty)
├── Ground               (MeshInstance3D, płaska płaszczyzna, płaski kolor)
└── Player               (CharacterBody3D)
    ├── Camera3D
    └── CollisionShape3D
```

> Pierwszoosobowa kontrola (WASD + mysz) to najprostsza droga — trzecioosobowa to dodatkowy koszt. Decyzja: ⬜ do ustalenia.

## Planowane rozszerzenia (późniejsze etapy)

### Skala Bortle (zanieczyszczenie świetlne)
- Parametr 1–9 (1 = najciemniejsze, 9 = miasto).
- Widoczne jako **kolor nieba / jasność horyzontu** (im jaśniejszy „blask", tym mniej gwiazd).
- Każda lokalizacja ma swój Bortle (zobacz milestones Etap 3).
- Wpływa na to, które obiekty są widoczne.

### Ruch nieba (pozorny)
- Pozorny ruch w czasie realnym (niebo obraca się wokół bieguna niebieskiego).
- Na razie uproszczone (stała prędkość kątowa, bez refrakcji/atmosfery).
- Osobny etap (zobacz milestones „Później").

### Pora roku
- Niebo zależy od pory roku (które gwiazdozbiory nad horyzontem).
- Na razie: stała „domyślna" pora roku.

### Lokalizacje
- Na razie: **jedna** lokalizacja.
- Docelowo: „silnik miejscówek" — każda lokalizacja różni się głównie **jakością nieba** (Bortle).
- Bez szaleństwa z ilością miejscówek na start.

## Otwarte pytania

- [ ] Która opcja techniczna gwiazd (A czy B) na start?
- [ ] Czy w Etapie 0 potrzebny Księżyc / Droga Mleczna? (tendencja: nie)
- [ ] Jak uproszczony ma być „prawdziwy" katalog gwiazd (które gwiazdy wchodzą)?
- [ ] Czy pokazywać biegun niebieski / oś rotacji (gdy wdrożymy ruch)?
