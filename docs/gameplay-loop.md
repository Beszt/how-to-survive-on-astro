# Pętla gameplayu: dzień / noc

## Pętla (jeden „dzień")

```
[DNIEŃ]  zaplanuj sesję → obrób materiał → zarządzaj sprzętem
    ↓  (przejście przez drzwi)
[NOC]    sprawdź plan w praktyce → fotografuj niebo
    ↓  (powrót)
[DNIEŃ]  pieniądze/postęp → powtórz z lepszym warunkami
```

## Scena A: Pokój (dzień)

Baza gracza. Prosty pokój, low poly.

Zawartość:
- laptop (obróbka materiału, planowanie sesji)
- zgromadzony sprzęt (teleskop na montażu, torby, części)
- **drzwi wyjściowe** (przejście do sceny nocnej)

Działania w dzień (koncept, nie zaimplementowane):
- planowanie sesji: cel, czas, lokalizacja, pogoda
- obróbka zdjęć z poprzedniej nocy
- zamawianie / kupno / naprawa sprzętu
- zarządzanie pieniędzmi (paliwo, jedzenie — zobacz [economy.md](economy.md))

## Scena B: Nocne niebo (noc)

Otwarty krajobraz. Różny zależnie od skali Bortle’a (zanieczyszczenie świetlne).

- sprawdzenie planu w praktyce
- próba fotografowania nocnego nieba
- niebo = **główny i najważniejszy obiekt gry** (gracz na ziemi, rozgląda się w górę: szukanie obiektów, zoom) — zobacz [sky-sim.md](sky-sim.md)
- **kamera pierwszoosobowa** (WASD + mysz) ✅

## Przejścia między scenami

- drzwi w pokoju → scena nocna (na razie: bezpośrednia zmiana sceny)
- powrót z nocy → dzień, z wynikiem (zdjęcia/pieniądze)

## Aktualny stan

- [ ] obie sceny to na razie **koncept**
- [ ] pierwszy prototyp: **tylko Scena B** (nocne niebo + chodzenie) — zobacz [milestones.md](milestones.md)

## Otwarte pytania

- [ ] Przejście dzień/noc: natychmiast, czy z „dojazdem"/animacją?
- [ ] Czy czas idzie w real-time, czy „ruchomy" (krok po kroku)?
