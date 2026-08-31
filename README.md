# Workout — instalacja na telefonie

Aplikacja to PWA: 4 pliki statyczne, zero zależności, zero build stepu.
Działa offline po pierwszym otwarciu. Dane trzymane w `localStorage` przeglądarki.

```
index.html          cała aplikacja (HTML + CSS + JS)
manifest.json       nazwa, ikona, tryb standalone
sw.js               service worker (offline)
icon-192.png / icon-512.png / icon-512-maskable.png
```

## Wariant A — GitHub Pages (darmowy, HTTPS, ~5 min)

1. Nowe repo na GitHub, np. `workout`, publiczne.
2. Wrzuć wszystkie pliki do korzenia repo (drag & drop w "Add file → Upload files").
3. Settings → Pages → Source: `Deploy from a branch`, branch `main`, folder `/ (root)`. Save.
4. Po ~1 minucie adres to `https://<login>.github.io/workout/`.

## Wariant B — Netlify Drop (bez konta GitHub, ~1 min)

Wejdź na `app.netlify.com/drop` i przeciągnij folder `workout-tracker`. Dostajesz od razu adres HTTPS.

## Instalacja na telefonie

**iPhone (Safari — musi być Safari, nie Chrome):**
adres → przycisk Udostępnij → `Dodaj do ekranu początkowego`.

**Android (Chrome):**
adres → menu ⋮ → `Zainstaluj aplikację` / `Dodaj do ekranu głównego`.

Po instalacji odpala się na pełnym ekranie, bez paska przeglądarki, offline.

## HTTPS jest wymagany

Service worker i instalacja działają tylko po HTTPS (albo na `localhost`).
Otwarcie pliku przez `file://` uruchomi apkę, ale bez offline i bez instalacji.

Lokalny test: `python3 -m http.server 8000` w tym folderze, potem `http://localhost:8000`.

## Dane i backup

- Wszystko siedzi w `localStorage` na tym jednym urządzeniu — nic nie leci na serwer.
- Wyczyszczenie danych witryny / odinstalowanie apki = utrata historii.
- **Me → Export backup (.json)** zapisuje pełną kopię, **Import backup** ją odtwarza.
  Rób eksport raz na jakiś czas, to jedyne zabezpieczenie.
- Przeniesienie na nowy telefon: eksport → wyślij plik sobie → import.

## Aktualizacja aplikacji

Po podmianie `index.html` w repo podbij wersję cache w `sw.js`:
`const CACHE = 'workout-v2';` — inaczej telefon zostanie na starej, zacache'owanej wersji.

## Uwagi

- Jednostka (kg/lbs) jest globalna i zmienia tylko etykiety — zapisane liczby nie są przeliczane.
- Est. 1RM liczone wzorem Epleya: `ciężar × (1 + powtórzenia / 30)`.
- Baza ćwiczeń jest w `index.html` w stałej `DB` — dopisujesz tam własne kategorie,
  albo dodajesz ćwiczenia z poziomu apki (wyszukiwarka → "Create …").
