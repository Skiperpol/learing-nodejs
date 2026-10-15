# Uruchamianie skryptów Node.js z wiersza poleceń

## 1. Podstawowe sposoby uruchamiania

- **Uruchomienie pliku:** 
  ```bash
  node app.js
  ```
- **Pliki wykonywalne (Shebang):** Dodanie nagłówka w pierwszej linii pliku pozwala uruchamiać go jak plik wykonywalny systemu (po nadaniu uprawnień `chmod u+x app.js` na Linux/macOS):
  ```javascript
  #!/usr/bin/env node
  // kod JavaScript...
  ```
- **Wykonanie kodu inline (bez pliku):** Użycie flagi `-e` / `--eval`:
  ```bash
  node -e "console.log(123)"
  ```

---

## 2. Automatyczne przeładowanie (`--watch`)

Node.js posiada wbudowany tryb obserwowania plików. Po zmianie w kodzie aplikacja automatycznie się restartuje:

```bash
node --watch app.js
```

---



## 3. Natywny Task Runner (`node --run`)

Node.js umożliwia uruchamianie skryptów zdefiniowanych w pliku `package.json` bez używania `npm run`:

```json
{
  "type": "module",
  "scripts": {
    "start": "node app.js",
    "test": "node --test"
  }
}
```

- **Uruchomienie:** `node --run test`
- **Przekazywanie argumentów:** Należy użyć separatora `--`:
  ```bash
  node --run start -- --port 8080
  ```
- **Zmienne środowiskowe:** Automatycznie ustawia `NODE_RUN_SCRIPT_NAME` oraz `NODE_RUN_PACKAGE_JSON_PATH`.
- **Cechy:** Został zaprojektowany z myślą o maksymalnej wydajności — nie obsługuje hooków `pre` / `post` znanych z `npm run`.

---



## 4. Nowoczesny Workflow w branży (Jak robi się to dziś?)

1. **Praca z TypeScript (**`tsx`**):**
  Większość komercyjnych projektów pisze się w TypeScript. Standardowym narzędziem do dewelopmentu jest pakiet `tsx`, który zastępuje `node` i od razu obsługuje tryb obserwacji plików:
2. **Natywne paski uruchamiania TypeScript w Node.js:**
  Nowsze wersje Node.js umożliwiają uruchamianie plików `.ts` bezpośrednio:
3. **Rezygnacja z** `nodemon`**:**
  Narzędzia typu `nodemon` są dziś uważane za zbędne — zastąpiła je natywna flaga `node --watch` oraz wbudowany watch-mode w `tsx` i frameworkach testowych.
4. **Szybsze menedżery pakietów (**`pnpm` **/** `bun`**):**
  Zamiast `npm run`, w dużych projektach standardem stał się `pnpm` (np. `pnpm dev`), ze względu na szybkość i oszczędność miejsca na dysku.

