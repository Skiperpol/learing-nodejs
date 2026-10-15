# Najlepsze praktyki bezpieczeństwa w Node.js (Security Best Practices)

## 1. Koncepcja: Model Zagrożeń Node.js
* **Zasada działania:** Node.js z założenia uważa kod aplikacji, uruchamiane zależności oraz lokalny system plików za **zaufane**.
* **Wniosek:** Większość podatności (np. DoS, wycieki danych, obce pakiety) nie leży w samym rdzeniu Node.js, lecz w **kodzie aplikacji i konfiguracji**. Bezpieczeństwo jest odpowiedzialnością dewelopera.

---

## 2. Kluczowe zagrożenia i sposoby ochrony

| Zagrożenie | Opis | Sposób ochrony (Mitigation) |
| --- | --- | --- |
| **Denial of Service (DoS)** | Przeciążenie serwera lub wywołanie awarii (np. ataki Slowloris, brak obsługi błędów na gniazdach). | • Stosowanie **Reverse Proxy** (np. Nginx).<br>• Poprawna obsługa błędów gniazd (`socket.on('error')`).<br>• Ustawienie timeoutów (`requestTimeout`, `headersTimeout`). |
| **Wyciek danych przy publikacji (CWE-552)** | Przypadkowe opublikowanie sekretów/plików do rejestru `npm`. | • Weryfikacja poleceniem `npm publish --dry-run`.<br>• Stosowanie `.gitignore`, `.npmignore` oraz sekcji `"files"` w `package.json`. |
| **Timing Attacks (CWE-208)** | Odgadywanie sekretów (np. haseł) na podstawie czasu odpowiedzi serwera przy porównywaniu ciągów znaków. | • Używanie `crypto.timingSafeEqual()` do porównywania sekretów w stałym czasie.<br>• Używanie `crypto.scrypt` do haseł. |
| **HTTP Request Smuggling (CWE-444)** | Różnice w interpretacji zapytań HTTP między serwerem proxy a Node.js. | • Unikanie opcji `insecureHTTPParser`.<br>• Stosowanie protokołu HTTP/2 end-to-end. |
| **Prototype Pollution (CWE-1321)** | Iniekcja właściwości do globalnych obiektów JavaScript poprzez `__proto__`. | • Tworzenie obiektów bez prototypu: `Object.create(null)`.<br>• Zamrażanie prototypów: `Object.freeze()`.<br>• Używanie flagi `--disable-proto`.<br>• Sprawdzanie właściwości przez `Object.hasOwn()`. |
| **Monkey Patching (CWE-349)** | Nadpisywanie w czasie działania aplikacji wbudowanych metod (np. `Array.prototype.push`). | • Używanie flagi `--frozen-intrinsics`.<br>• Zamrożenie obiektów globalnych: `Object.freeze(globalThis)`. |
| **DNS Rebinding (CWE-346)** | Atak na włączony inspector debugowania (`--inspect`). | • **Nigdy** nie uruchamiaj inspectora na produkcji.<br>• Wyłączaj inspector przy sygnale `SIGUSR1`. |

---

## 3. Bezpieczeństwo Łańcucha Dostaw (npm & zewn. pakiety)

Zależności (zarówno bezpośrednie, jak i tzw. *transitive dependencies*) stanowią jedno z największych zagrożeń w ekosystemie Node.js (typosquatting, przejęte konta twórców, lockfile poisoning).

* **Zasada instalacji:** Używaj `npm ci` zamiast `npm install` (wymusza ścisłą zgodność z plikiem `package-lock.json`).
* **Blokada skryptów:** Wyłączaj automatyczne wykonywanie skryptów po instalacji:
  ```bash
  npm config set ignore-scripts true
  ```
* **Kwarantanna nowych wydań (Cooldown):** Używaj opcji `--min-release-age` (dostępnej od npm v11.10.0+), aby unikać instalacji pakietów wydanych w ciągu ostatnich X dni (np. `--min-release-age=1`).
* **Skanowanie:** Automatyzuj audyty bezpieczeństwa w CI/CD za pomocą `npm audit` lub narzędzi statycznych (np. Socket).

---

## 4. Natywny Model Uprawnień Node.js (Permission Model)

Odpalanego procesu Node.js można pilnować natywnym mechanizmem uprawnień (flaga `--permission`). Pozwala on precyzyjnie ograniczać zaufany i niezaufany kod w kwestiach takich jak:
* Dostęp do systemu plików (odczyt/zapis).
* Dostęp do sieci (przychodzący/wychodzący).
* Tworzenie procesów potomnych (`child_process`).

---

> **Podsumowanie:** Bezpieczeństwo w Node.js opiera się na: **izolacji sieciowej** (Reverse Proxy), **rygorystycznej kontroli npm** (`npm ci`, `ignore-scripts`), **bezpiecznym kodowaniu** (stały czas porównań, ochrona prototypów) oraz **używaniu aktualnych mechanizmów ochronnych** (Permission Model).