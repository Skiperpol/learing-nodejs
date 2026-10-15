# Pobieranie danych od użytkownika w terminalu (Node.js CLI)

## 1. Natywny moduł `node:readline` (Podejście klasyczne z Callbackami)
Node.js udostępnia wbudowany moduł `node:readline` do interaktywnej komunikacji z użytkownikiem poprzez standardowe strumienie wejścia/wyjścia (`process.stdin` i `process.stdout`).

```javascript
const readline = require('node:readline');

// Tworzenie interfejsu odczytu
const rl = readline.createInterface({
  input: process.stdin,
  output: process.stdout,
});

// Zadanie pytania i pobranie odpowiedzi
rl.question("Jak masz na imię? ", (name) => {
  console.log(`Witaj ${name}!`);
  rl.close(); // Zamknięcie interfejsu jest wymagane, aby zakończyć proces
});
```

---

## 2. Nowoczesne podejście: `readline/promises` (async/await)
W nowszych wersjach Node.js (v17+) dostępna jest natywna wersja modułu oparta na **Promise'ach**, co pozwala pisać czystszy kod z użyciem `async/await` zamiast zagłębiania się w callbacki:

```javascript
import * as readline from 'node:readline/promises';
import { stdin as input, stdout as output } from 'node:process';

const rl = readline.createInterface({ input, output });

const name = await rl.question("Jak masz na imię? ");
console.log(`Witaj ${name}!`);

rl.close();
```

---

## 3. Wyzwania: Pobieranie haseł i ukrywanie tekstu
Standardowa metoda `rl.question()` wyświetla wpisywane znaki w terminalu. Przy pobieraniu poufnych danych (np. haseł) zaleca się maskowanie znaków (np. gwiazdkami `*`) lub całkowite ukrywanie wpisywanego tekstu poprzez sterowanie strumieniem `process.stdin`.

---

## 4. Jak robi się to dziś w praktyce (Biblioteki CLI)?

Chociaż wbudowany moduł `node:readline` dobrze sprawdza się do prostych skryptów, w komercyjnych narzędziach CLI rzadko pisze się logikę interaktywną od zera. Do bardziej złożonych formularzy (listy wyboru, checkboxy, maskowane hasła, walidacja) używa się dedykowanych pakietów:

* **`@clack/prompts`** — Niezwykle popularna, nowoczesna biblioteka tworząca estetyczne i minimalistyczne interfejsy CLI.
* **`inquirer`** — Klasyk ekosystemu Node.js z bogatym zestawem gotowych promptów.
* **`prompts`** — Lekka i szybka alternatywa dla Inquirera.