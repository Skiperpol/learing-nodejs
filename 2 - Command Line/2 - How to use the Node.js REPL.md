# Node.js REPL (Read-Eval-Print Loop)

## 1. Czym jest REPL?
* **Pojęcie:** Interaktywne środowisko uruchomieniowe built-in w Node.js, działające w pętli: **R**ead (odczytaj) -> **E**val (przetwórz) -> **P**rint (wyświetl) -> **L**oop (powtórz).
* **Zastosowanie:** Szybkie testowanie fragmentów kodu JavaScript bez konieczności tworzenia plików.
* **Uruchomienie:** Wpisanie komendy `node` w terminalu bez żadnych argumentów:
  ```bash
  node
  ```

---

## 2. Podstawowe zachowanie i funkcjonalności

### Instrukcje vs Wyrażenia
* REPL automatycznie wypisuje wynik każdego wyrażenia.
* Wywołanie `console.log('test')` wyświetli najpierw napis `test` (efekt uboczny wywołania), a następnie `undefined` (zwracana wartość funkcji).
* Wyrażenie `5 === '5'` od razu zwróci wynik `false`.

### Specjalne funkcje i skróty
* **Zmienna `_`:** Przechowuje wynik ostatnio wykonanej operacji.
* **Strzałka w górę (`↑`):** Daje dostęp do historii komend (również z poprzednich sesji REPL).
* **Tryb wielolinijkowy:** REPL automatycznie wykrywa niedokończony kod (np. otwartą klamrę `{` lub funkcję) i przechodzi do trybu wieloliniowego oznaczonego potrójną kropką (`...`).

---

## 3. Komendy specjalne (Dot Commands)

Komendy specjalne zaczynają się od kropki `.`:

| Komenda | Opis |
| --- | --- |
| `.help` | Wyświetla pomoc i listę dostępnych komend z kropką. |
| `.editor` | Wchodzi w tryb edytora do wygodnego pisania wielolinijkowych bloków kodu. |
| `.break` / `.clear` | Przerywa wprowadzanie wielolinijkowej instrukcji (działa jak `Ctrl + C`). |
| `.load` | Wczytuje i wykonuje plik JavaScript (ścieżka względna do katalogu). |
| `.save` | Zapisuje całą historię obecnej sesji REPL do wskazanego pliku. |
| `.exit` | Zamyka sesję REPL (działa jak dwukrotne naciśnięcie `Ctrl + C`). |

---

## 4. Programowalne użycie REPL w kodzie (`node:repl`)

Środowisko REPL można osadzić we własnej aplikacji Node.js przy użyciu natywnego modułu `node:repl`:

```javascript
const repl = require('node:repl');

// Uruchomienie sesji z własnym znakiem zachęty (prompt)
const local = repl.start('$ ');

// Obsługa zdarzenia wyjścia z REPL
local.on('exit', () => {
  console.log('Zamykanie sesji REPL...');
  process.exit();
});
```