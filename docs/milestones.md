# Etapy / kroki milowe

> Każdy Etap to **osobna, niezależna do rozwinięcia całość**. Pracujemy tylko nad bieżącym Etapem.
> Status: ⬜ nie zaczęto · 🚧 w trakcie · ✅ gotowe

## Etap 0 — punkt startowy + pierwszy prototyp

**Status:** 🚧 w trakcie (dokumentacja)

**Cel:** „walking skeleton" — scena 3D z gwiazdami i swobodnym chodzeniem. **Zero mechanik.**

**Zakres (wchodzi):**
- [x] dokumentacja (ten plik, [sky-sim.md](sky-sim.md), [concept.md](concept.md))
- [ ] scena 3D: podłoże, niebo, nieruchome gwiazdy (białe, okrągłe punkty; rozmiar = jasność)
- [ ] stały kolor nieba (bez dynamicznego Bortle’a)
- [ ] jedna lokalizacja
- [ ] swobodne chodzenie w 3D, **kamera pierwszoosobowa** (WASD + mysz) ✅
- [ ] architektura pod rozbudowę: warstwa `Sky`, gwiazdy jako dane (zobacz [sky-sim.md](sky-sim.md))
- [ ] **i tyle** — brak UI, HUD-u, mechanik (menu/HUD można dodać w dowolnym momencie)

**Plan sceny** (szczegóły w [sky-sim.md](sky-sim.md)):

```
Prototype (Node3D)
├── WorldEnvironment     (ciemne niebo + ambient)
├── Ground               (MeshInstance3D, płaska płaszczyzna)
├── Player               (CharacterBody3D + Camera3D, pierwszoosobowa)
└── Sky                  (Node3D — „sfera niebieska", główny obiekt gry)
    └── Stars            (MultiMeshInstance3D, białe dyski, rozmiar = jasność)
```

**Poza zakresem (na razie):**
- skala Bortle, ruch nieba, pory roku
- scena pokoju (dzień)
- jakiekolwiek mechaniki (pieniądze, zdjęcia, sprzęt)

**Definicja gotowości:**
- gracz może chodzić i patrzeć na niebo
- gwiazdy widoczne jako białe punkty, niebo w stałym ciemnym kolorze
- działa stabilnie (bez crashy)

---

## Etap 1 — pierwsze zlecenie / pierwszy zarobek

**Status:** ⬜ nie zaczęto

**Cel:** pierwsze pieniądze w grze. Gracz wykonuje proste „zlecenie" i zarabia.

**Zakres (koncept):**
- [ ] scena pokoju (dzień) + drzwi wyjściowe
- [ ] prosta mechanika „zdjęcia" (niebo z Etapu 0 → „robisz zdjęcie")
- [ ] pierwsze zlecenie/klient, pierwsze pieniądze
- [ ] podstawowy HUD z pieniędzmi

---

## Etap 2 — pierwszy upgrade sprzętu

**Status:** ⬜ nie zaczęto

**Zakres (koncept):**
- [ ] sklep / zamawianie sprzętu
- [ ] pierwszy upgrade (np. lepszy montaż lub odroszczacz)
- [ ] upgrade widocznie poprawia jakość zdjęć / przychód

---

## Etap 3 — nowa lokalizacja / lepsze niebo

**Status:** ⬜ nie zaczęto

**Zakres (koncept):**
- [ ] druga lokalizacja (ciemniejsze niebo, niższy Bortle)
- [ ] mechanika dojazdu (paliwo!)
- [ ] skala Bortle widoczna jako kolor nieba (zobacz [sky-sim.md](sky-sim.md))

---

## Później (pomyślnie, jeszcze bez etapu)

- gwiazdozbiory: linie + nazwy na niebie (pierwsza rozbudowa nieba)
- zoom na obiekty (FOV kamery)
- ruch nieba (pozorny) w czasie
- pory roku
- więcej lokalizacji (silnik „miejscówek")
- pogoda, chmury
- (zobacz [backlog.md](backlog.md))
