# How to Survive on Astro?!

**A Starving Astrophotographer Simulator** — humorystyczny symulator początkującego astrofotografa, który postanawia utrzymywać się z fotografowania nocnego nieba.

## Status

🚧 **Pre-produkcja / koncept** — faza dokumentacji. Brak grywalnego kodu.
Pierwszy grywalny cel: prosta scena 3D z gwiazdami i swobodnym chodzeniem (zobacz [docs/milestones.md](docs/milestones.md) → Etap 0).

## Języki gry

- 🇵🇱 polski (główny)
- 🇬🇧 angielski (główny)
- możliwość rozbudowy o kolejne języki

## Stack techniczny (ustalony)

| Element | Wybór |
|---|---|
| Silnik | Godot 4.x (projekt: 4.7) |
| Build | .NET (C#) |
| Renderer | Forward+ |
| Język główny | C# (skrypty nodów) |
| Styl graficzny | low poly — **świadomy wybór estetyczny**, nie ograniczenie |
| Edytor | Godot (sceny/GUI) + VS Code (C#), wspomagane agentowym kodowaniem |
| Repo | prywatne na GitHub, bez licencji open source |

## Dokumentacja

Małe, osobne pliki (przjazne dla ADHD) — jeden temat na plik:

| Plik | Zawartość |
|---|---|
| [docs/concept.md](docs/concept.md) | koncept, ton, humor, tytuł PL/EN |
| [docs/gameplay-loop.md](docs/gameplay-loop.md) | pętla dzień/noc, opis obu scen |
| [docs/economy.md](docs/economy.md) | ekonomia, sprzęt, budżet (luźne pomysły) |
| [docs/milestones.md](docs/milestones.md) | etapy 0–3+ i zakres każdego |
| [docs/sky-sim.md](docs/sky-sim.md) | symulator nieba: stan obecny + plan rozbudowy |
| [docs/backlog.md](docs/backlog.md) | luźne pomysły, easter eggi |

## Zasady rozwoju

- Rozwój **przyrostowy**: najpierw spisać pomysł, potem implementować mały prototyp.
- Każdy etap to osobna, niezależna do rozwinięcia całość (zobacz [milestones.md](docs/milestones.md)).
- Żaden pomysł nie ginie — wszystko, co warto zapamiętać, trafia do [backlog.md](docs/backlog.md).
- Najpierw dokument, potem kod.
