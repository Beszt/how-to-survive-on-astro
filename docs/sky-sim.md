# Symulator nieba

> Podejście: **uproszczony symulator, nie turbo-rzeczywisty.**
> Gwiazdozbiory = prawdziwe, obiekty = prawdziwe, ale całość uproszczona.
> Na razie: **północna półkula** (niebo północne).
>
> **Niebo = główny i najważniejszy obiekt gry** — zawsze u góry; gracz chodzi po ziemi i rozgląda się w górę (Stellarium-lite, low poly, bez miliona gwiazd).

## Obecny prosty stan (Etap 0)

**Zakres pierwszego prototypu — i tylko tyle:**
- nieruchome gwiazdy jako **białe, okrągłe punkty** (czysty biały `#ffffff`; rozmiar = jasność, minimalne różnice)
- stały kolor nieba (bez dynamicznego Bortle’a)
- jedna lokalizacja
- swobodne chodzenie w 3D, **kamera pierwszoosobowa** (WASD + mysz) ✅
- bez gwiazdozbiorów/linii/etykiet w samym Etapie 0 — ale **architektura na to gotowa** (zobacz niżej)

### Specyfikacja gwiazd (ustalona)

- **Kształt:** okrągły punkt (idealny dysk) — mały, równy kółko, nie kwadrat.
- **Kolor:** zawsze czysta biel `#ffffff`; jasność gwiazdy pokazuje **wyłącznie rozmiar**.
- **Rozmiar:** minimalnie zróżnicowany — reprezentacja jasności (mag. wizualna V); im jaśniejsza gwiazda, tym większy punkt; zakres celowo mały (czytelność, low poly).
- **Katalog:** mały, starannie wybrany zestaw najjaśniejszych gwiazd — **bez „pierdyliarda gwiazd"**. Każda gwiazda = dane: nazwa, kierunek na sferze niebieskiej, jasność.
- **Dlaczego to ważne:** gwiazdy jako **dane/obiekty** (nie „szum na teksturze") — do każdej można potem przyczepić linię gwiazdozbioru, etykietę, marker obiektu.

### Opcje techniczne gwiazd (decyzja: ✅ Opcja A)

**Opcja A (wybrana): `MultiMeshInstance3D` — gwiazdy jako dane**
- katalog gwiazd → transforamcje MultiMesha na sferze (jeden draw call)
- mesh = mały quad + shader rysujący idealny dysk (odcina rogi); materiał unshaded, biały
- rozmiar gwiazdy = skala jej transformacji (jasność → rozmiar)
- ruch nieba (potem) = obrót całego nodu `Sky`

**Opcja B (rezerwa): shader-skydome** — gwiazdy liczone per-piksel (hash).
- mocna pod Bortle-fade / Drogę Mleczną, słabsza pod „gwiazdy jako obiekty z pozycją"
- ewentualny powrót przy rozbudowie o Bortle, jeśli MultiMesh zacznie ograniczać

**Niebo (stały ciemny kolor):** `WorldEnvironment` + `ProceduralSkyMaterial` z ciemnym kolorem (`sky_top_color`, `sky_horizon_color`) — bez zmian.

> ⚠️ Promień sfery gwiazd musi być **mniejszy niż `far` kamera** (domyślnie 4000).

### Drzewo sceny (Etap 0 + architektura pod rozbudowę)

```
Prototype (Node3D)
├── WorldEnvironment     (Environment + ProceduralSkyMaterial, ciemny)
├── Ground               (MeshInstance3D, płaska płaszczyzna, płaski kolor)
├── Player               (CharacterBody3D + Camera3D pierwszoosobowa + CollisionShape3D)
└── Sky                  (Node3D — „sfera niebieska", główny obiekt gry)
    ├── Stars            (MultiMeshInstance3D, białe dyski, rozmiar = jasność)     [Etap 0]
    ├── Constellations   (gwiazdozbiór → Lines (Line3D) + Label (Label3D))         [potem]
    └── Objects          (markery planet / jasnych mgławic)                        [potem]
```

- **Animacja (ruch nieba, potem):** obrót `Sky` wokół osi bieguna niebieskiego — całe niebo się „przesuwa".
- **Zoom (potem):** tween `Camera3D.fov` (np. 75° → 30°).
- **Teksty/etykiety (potem):** `Label3D` na sferze.

## Planowane rozszerzenia (późniejsze etapy)

### Gwiazdozbiory, etykiety, zoom (pierwsza rozbudowa nieba)
- linie między gwiazdami gwiazdozbioru (`Line3D`) + nazwy (`Label3D`)
- mały katalog obiektów (planety / jasne mgławice) jako czytelne markery
- zoom na obiekt (FOV kamery), „szukanie" obiektu
- architektura już na to gotowa — zobacz drzewo sceny wyżej

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

- [x] Opcja techniczna gwiazd → **A (MultiMesh)** ✅
- [x] Kamera → **pierwszoosobowa** ✅
- [x] Księżyc / Droga Mleczna w Etapie 0 → **nie** ✅
- [ ] Dokładny rozmiar katalogu gwiazd na start (tendencja: najjaśniejsze, kilkadziesiąt)
- [ ] Czy pokazywać biegun niebieski / oś rotacji (gdy wdrożymy ruch)?
