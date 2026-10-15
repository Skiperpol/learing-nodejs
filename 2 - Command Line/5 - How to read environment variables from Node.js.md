# Odczytywanie zmiennych środowiskowych w Node.js

## 1. Dostęp do zmiennych (`process.env`)
Globalny obiekt `process` udostępnia właściwość `process.env`, która zawiera wszystkie zmienne środowiskowe przekazane do procesu w momencie jego uruchomienia:

```bash
USER_ID=239482 USER_KEY=foobar node app.js
```

```javascript
// Obiekt 'process' jest globalny, nie wymaga importu
console.log(process.env.USER_ID);  // "239482"
console.log(process.env.USER_KEY); // "foobar"
```

---

## 2. Natywna obsługa plików `.env` w Node.js (bez zewnętrznych paczek)

Nowoczesne wersje Node.js oferują natywne wczytywanie plików `.env` bez konieczności instalacji biblioteki `dotenv`.

### A. Flaga CLI `--env-file`
Pozwala wczytać plik z konfiguracją przy uruchamianiu procesu:

```bash
# Wczytanie domyślnego pliku .env
node --env-file=.env app.js

# Wczytanie wielu plików (późniejsze pliki nadpisują wcześniejsze)
node --env-file=.env --env-file=.development.env app.js

# Ciche ignorowanie braku pliku (brak błędu gdy plik nie istnieje)
node --env-file-if-exists=.env app.js
```

> **Ważne:** Zmienne przekazane bezpośrednio w systemie/terminalu mają wyższy priorytet niż te z pliku `.env`.

### B. Programowalne wczytywanie (`process.loadEnvFile()`)
Plik `.env` można również wczytać bezpośrednio w kodzie aplikacji:

```javascript
import { loadEnvFile } from 'node:process';

// Wczytuje domyślny .env lub ze wskazanej ścieżki
loadEnvFile('./config/.env');

console.log(process.env.PORT);
```

---

## 3. Jak robi się to dziś i jak robić to profesjonalnie?

W komercyjnych projektach produkcyjnych samo pobieranie `process.env.VARIABLE` nie wystarcza — brakująca lub niepoprawna zmienna może doprowadzić do cichych błędów lub awarii aplikacji w czasie działania (*runtime*).

### Profesjonalny standard: Ścisła walidacja schematu (np. z biblioteką Zod)

Dobra praktyka wymaga, aby weryfikować i typować całą konfigurację **przy starcie aplikacji**. Jeśli brakuje wymaganej zmiennej (np. `DATABASE_URL`), aplikacja powinna od razu zgłosić błąd i uniemożliwić uruchomienie.

#### Przykład profesjonalnego modułu konfiguracji (`src/config.ts`):

```typescript
import { z } from 'zod';

// 1. Definicja schematu z walidacją typów i wartości
const envSchema = z.object({
  NODE_ENV: z.enum(['development', 'production', 'test']).default('development'),
  PORT: z.string().transform(Number).default('3000'),
  DATABASE_URL: z.string().url('DATABASE_URL musi być poprawnym adresem URL'),
  API_KEY: z.string().min(1, 'API_KEY jest wymagany'),
});

// 2. Walidacja process.env przy starcie aplikacji
const parseResult = envSchema.safeParse(process.env);

if (!parseResult.success) {
  console.error('❌ Niepoprawna konfiguracja zmiennych środowiskowych:');
  console.error(parseResult.error.format());
  process.exit(1); // Natychmiastowe przerwanie procesu
}

// 3. Eksport w pełni otypowanego i bezpiecznego obiektu
export const env = parseResult.data;
```

#### Użycie w reszcie aplikacji:
```typescript
import { env } from './config';

// Pełne autouzupełnianie i gwarancja typu (env.PORT to number, nie string!)
app.listen(env.PORT, () => {
  console.log(`Serwer działa na porcie ${env.PORT}`);
});
```

### Podsumowanie dobrych praktyk
1. **Nie używaj `process.env` bezpośrednio w kodzie biznesowym** — zaciągaj zmienne wyłącznie z jednego, centralnego pliku konfiguracji z walidacją (np. `config.ts`).
2. **Nigdy nie commituj plików `.env` do repozytorium** — w kodzie trzymaj jedynie plik wzorcowy `.env.example` z nazwiskami zmiennych bez wartości wrażliwych.
3. **Na środowisku produkcyjnym (np. Docker / Kubernetes / AWS)** nie używaj plików `.env`, lecz przekazuj zmienne bezpośrednio przez kontener lub menedżery sekretów (np. AWS Secrets Manager).