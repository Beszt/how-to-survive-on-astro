# Plan: implementacja Etapu 0 (walking skeleton)

## 1. Przeczytaj najpierw (w tej kolejności)

1. `README.md` — projekt, stack, zasady rozwoju
2. `docs/concept.md` — koncept, ton, kierunek artystyczny (low poly)
3. `docs/milestones.md` → sekcja **Etap 0** — zakres i definicja gotowości (to Twój kontrakt)
4. `docs/sky-sim.md` — **specyfikacja gwiazd**, opcja techniczna (wybrana: MultiMesh), drzewo sceny
5. `docs/gameplay-loop.md` — kontekst (Scena B = ta, którą budujesz)

Jeśli w docs znajdziesz sprzeczność lub lukę, **nie zgaduj** — zatrzymaj się i zapytaj użytkownika.

## 2. Stan projektu (ważne!)

- Godot **4.7**, renderer **Forward+**, Windows d3d12, fizyka **Jolt** (zobacz `project.godot`)
- Konfiguracja .NET jest (`[dotnet] project/assembly_name`), **ALE brak `*.csproj` i jakiegokolwiek kodu C#** — to Twój pierwszy realny krok
- Brak akcji wejścia w `project.godot` (sekcja `[input]` nie istnieje)
- Repo prywatne, bez licencji OSS — **nie commituj, jeśli użytkownik tego nie prosi**

## 3. Zakres — wchodzi (i tylko to)

- [ ] `how-to-survive-on-astro.csproj` (Godot.NET.Sdk, wersja dopasowana do zainstalowanego Godota, `net8.0`)
- [ ] scena `res://prototypes/prototype.tscn` wg drzewa poniżej
- [ ] stałe, ciemne niebo (`WorldEnvironment` + `ProceduralSkyMaterial`)
- [ ] płaskie podłoże (płaski kolor, low poly) + kolizja
- [ ] **kamera pierwszoosobowa**: WASD + mysz (C#, `CharacterBody3D`)
- [ ] gwiazdy: **białe, okrągłe punkty (dyski), rozmiar = jasność (drobne różnice), czysta biel `#ffffff`** — `MultiMeshInstance3D`
- [ ] mały, starannie wybrany katalog najjaśniejszych gwiazd (~30–60; jasność V ≈ 0–2)
- [ ] główna scena ustawiona na `prototype.tscn`

## 4. Zakres — NIE wchodzi (twarde reguły)

- Brak UI/HUD/menu (doda się w dowolnym momencie — nie jest blokada)
- Brak skali Bortle, ruchu nieba, pór roku, pogody
- Brak linii gwiazdozbiorów, etykiet, obiektów, zoomu — **ale architektura musi to wspierać** (wszystko niebieskie pod nodem `Sky`, gwiazdy = dane: nazwa + kierunek + jasność)
- Brak sceny pokoju, mechanik, ekonomii, Księżyca, Drogi Mlecznej
- Nie zmieniaj decyzji z docs; nie dodawaj „drobnych ulepszeń” poza zakresem

## 5. Drzewo sceny (zgodnie z `docs/sky-sim.md`)

```
Prototype (Node3D)
├── WorldEnvironment     (Environment + ProceduralSkyMaterial, ciemny)
├── Ground               (MeshInstance3D PlaneMesh, płaski kolor)
│   └── StaticBody3D + CollisionShape3D  (kolizja podłoża)
├── Player               (CharacterBody3D)
│   ├── Camera3D         (pozycja ~y=1.7, fov ~75)
│   └── CollisionShape3D (capsule ~1.8)
└── Sky                  (Node3D — „sfera niebieska”, główny obiekt gry)
    └── Stars            (MultiMeshInstance3D, białe dyski, rozmiar = jasność)
```

> ⚠️ Promień sfery gwiazd (sugeruję **1000**) musi być **mniejszy niż `far` kamery** (domyślnie 4000).

## 6. Szczegóły techniczne

### Niebo (stały ciemny kolor)
- `Environment.background_mode = BG_SKY`, `Sky` z `ProceduralSkyMaterial`
- Ciemne barwy nocy: `sky_top_color` (np. `#05070f`), `sky_horizon_color` (np. `#0d1526`) — agent może dobrać odcień, kierunek: chłodne, ciemne
- Ambient od nieba (`ambient_light_source = AMBIENT_SOURCE_SKY`)

### Gwiazdy (specyfikacja z `docs/sky-sim.md`)
- `MultiMeshInstance3D`, mesh bazowy: `QuadMesh` 1×1
- Materiał: spatial shader, **unshaded**, `ALBEDO = vec3(1.0)`, fragment **odrzuca fragmenty poza kołem** (idealny dysk), np. `vec2 p = UV - 0.5; if (dot(p, p) > 0.25) discard;` — jeśli punkty znikają pod pewnym kątem, dodaj hint `cull_disabled`
- Orientacja każdego quada: twarz do środka sceny (gracz w środku) — `Basis.LookingAt(Vector3.ZERO, pozycja)`; wtedy punkt jest zawsze pełnym kołem
- **Rozmiar = jasność**: skala per gwiazda z zakresu **0.6–1.6** (jaśniejsza [mniejsza mag.] = większa) — **minimalne** różnice, czytelność
- Katalog jako dane C# (np. `StarCatalog.cs`): `nazwa`, `kierunek (Vector3 znormalizowany)`, `magV`. Wybierz ~30–60 najjaśniejszych gwiazd (Sirius, Canopus, Arcturus, Wega, Kapella, Rigel, Procyon, Betelgeza, Aldebaran, Antares, Spica, Polluks, Deneb, Regulus, Altair, Polaris, Alnilam, Alnitak, Alhena, Saiph, Mintaka, Bellatrix, Elnath, Akrab, Szaul, Mimosa, Hadar, Gacrux, Alphecca, Wezen…) — pozycje przybliżone (kierunki na sferze), to prototyp
- Jedno MultiMesh, jeden draw call

### Gracz (first person)
- `CharacterBody3D` + C#: prędkość ~5 m/s, grawitacja ~-9.8*1.6 (bo low-poly prototyp), delta-based
- Mysz: `Input.MouseMode = MouseMode.Captured` po kliknięciu, ESC zwalnia (i chowa kursor) — minimalnie
- Akcje wejścia: dodaj `move_forward` (W), `move_back` (S), `move_left` (A), `move_right` (D) w `project.godot` (sekcja `[input]`) lub przez Project Settings w edytorze; fallback: wbudowane `ui_up`/`ui_down`/`ui_left`/`ui_right`

## 7. Kroki implementacji (kolejność)

1. **Setup**: `dotnet --version`; utwórz `how-to-survive-on-astro.csproj` (`Godot.NET.Sdk` pod wersję zainstalowanego Godota 4.7, `net8.0`); zweryfikuj, że `dotnet build` przechodzi na pustym projekcie
2. **Scena bazowa**: `prototype.tscn` — WorldEnvironment (ciemne niebo) + Ground (płaszczyzna + kolizja). Uruchom: widać ciemne niebo i podłoże
3. **Player**: CharacterBody3D + Camera3D + CollisionShape3D + skrypt FP (WASD + mysz). Uruchom: chodzenie i rozglądanie działają
4. **Sky + Stars**: node `Sky`, `Stars` (MultiMesh + shader dysku) + `StarCatalog.cs` z danymi; wygeneruj transforamcje (kierunek × promień 1000, skala = jasność). Uruchom: gwiazdy widoczne jako białe kółka
5. **Finalizacja**: główna scena = `prototype.tscn`; czysta konsola (brak czerwonych błędów/warningów); porządek w scenie zgodny z drzewem z sekcji 5

## 8. Definicja gotowości (z `docs/milestones.md`)

- [ ] gracz może chodzić i patrzeć na niebo (WASD + mysz)
- [ ] gwiazdy widoczne jako **białe, okrągłe punkty** (rozmiar = jasność, drobne różnice), czysta biel
- [ ] niebo w stałym ciemnym kolorze, jedna lokalizacja
- [ ] działa stabilnie — brak crashy, brak czerwonych błędów w konsoli
- [ ] `dotnet build` przechodzi

## 9. Weryfikacja

- `dotnet build` z katalogu projektu
- Uruchomienie sceny (edytor Godot .NET → F5, lub `godot --headless` do sanity-checka ładowania sceny)
- Ręcznie: rozglądanie w górę (gwiazdy w każdym kierunku), chodzenie (podłoże nie przenika), ESC zwalnia kursor

## 10. Reguły

- Kod C#: **pełne typowanie**, idiomy Godot C# (patrz skill `csharp-godot`), **bez komentarzy** (chyba że użytkownik poprosi)
- Stosuj się do konwencji projektu (`.editorconfig`)
- Załaduj przydatne skille: `csharp-godot`, `player-controller`, `3d-essentials`, `shader-basics`
- Architektura pod przyszłość: `Sky` jako kontener (potem `Constellations`/`Objects`), gwiazdy jako dane — **nawet jeśli Etap 0 tego nie renderuje**
- Nie commituj bez prośby użytkownika
- Po zakończeniu: zaktualizuj checkboxy Etapu 0 w `docs/milestones.md` i raport: co zrobione, jak uruchomić, co ewentualnie wymaga decyzji użytkownika
