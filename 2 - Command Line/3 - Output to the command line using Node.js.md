# Output / Wypisywanie danych na konsolę w Node.js

## 1. Podstawy i formatowanie (`console.log`)
Moduł `console` w Node.js działa bardzo podobnie do tego znanego z przeglądarek internetowych.

* **Podstawowe wypisywanie:** `console.log()` przyjmuje wiele argumentów oddzielonych przecinkami:
  ```javascript
  const x = 'x', y = 'y';
  console.log(x, y); // Wypisze: x y
  ```
* **Specyfikatory formatowania:** Pozwalają na wstawianie zmiennych wewnątrz ciągu tekstowego:
  * `%s` — formatuje jako ciąg znaków (string).
  * `%d` — formatuje jako liczbę (number).
  * `%i` — formatuje tylko część całkowitą liczby (integer).
  * `%o` — formatuje jako obiekt.
  ```javascript
  console.log('Mój %s ma %d lata', 'kot', 2);
  ```
* **Czyszczenie konsoli:** `console.clear()` czyszczący ekran terminala.

---

## 2. Zliczanie i stos wywołań

### Liczniki (`console.count` / `console.countReset`)
Zlicza, ile razy dana etykieta została przekazana do funkcji:
```javascript
console.count('jabłko'); // jabłko: 1
console.count('jabłko'); // jabłko: 2
console.countReset('jabłko'); // Resetuje licznik dla 'jabłko'
console.count('jabłko'); // jabłko: 1
```

### Śledzenie stosu wywołań (`console.trace`)
Wypisuje pełny stos wywołań (stack trace), co ułatwia debugowanie i sprawdzenie, skąd dana funkcja została wywołana:
```javascript
const f2 = () => console.trace();
const f1 = () => f2();
f1(); // Wypisze ścieżkę wywołań: f2 -> f1 -> plik główny
```

---

## 3. Mierzenie czasu wykonania kodu
Użycie pary metod `console.time()` oraz `console.timeEnd()` pozwala zmierzyć czas (w milisekundach) wykonywania danego fragmentu kodu:

```javascript
console.time('pętla-testowa');

for (let i = 0; i < 1000000; i++) {
  // jakaś operacja...
}

console.timeEnd('pętla-testowa'); // Wypisze np.: pętla-testowa: 2.415ms
```

---

## 4. Strumienie wyjściowe: `stdout` vs `stderr`

* **`console.log()`** — kieruje dane do standardowego strumienia wyjściowego (**`stdout`**).
* **`console.error()`** — kieruje dane do strumienia błędów (**`stderr`**).

Podział ten pozwala w środowiskach serwerowych (oraz w terminalu) na oddzielne przekierowywanie logów informacyjnych i logów błędów.

---

## 5. Stylizowanie i kolorowanie tekstu (`node:util`)

W nowszych wersjach Node.js (od v22.11) do stylizowania tekstu w terminalu dostępna jest natywna funkcja `styleText` z modułu `node:util` (dzięki czemu nie trzeba instalować zewnętrznych bibliotek, takich jak `chalk`):

```javascript
import { styleText } from 'node:util';

console.log(
  styleText(['red'], 'Czerwony tekst ') +
  styleText(['green', 'bold'], 'Zielony pogrubiony tekst ') +
  'zwykły tekst'
);
```