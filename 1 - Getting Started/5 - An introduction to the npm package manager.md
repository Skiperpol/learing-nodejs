Czym jest npm / pnpm: menedżerem paczek do pobierania i zarządzania bibliotekami (zależnościami) dla Node.js. pozwala instalować konkretne wersje paczek, co zapobiega błędom u innych członków zespołu.

Dlaczego pnpm jest zazwyczaj lepszy od npm?
- Oszczędność miejsca: zapisuje paczkę tylko raz na dysku w globalnym magazynie i tworzy odnośniki (linki), zamiast kopiować te same pliki do każdego projektu.
- Szybkość: pobiera i instaluje paczki znacznie szybciej niż npm.
- Porządek: blokuje niewidoczne/niezadeklarowane zależności (zapobiega błędom w kodzie).

Kiedy jesteśmy zmuszeni użyć npm?
- W starych projektach lub środowiskach firmowych (CI/CD), które są na sztywno skonfigurowane pod package-lock.json.
- Gdy używamy bardzo starych paczek, które nie działają ze strukturą linków w pnpm.

Komendy:
pnpm install - instalacja wszystkich paczek z projektu
pnpm add <package-name> - instalowanie pojedyńczej paczki do projektu
    --save-dev – zapisuje w devDependencies (narzędzia tylko do tworzenia kodu, np. do testów/builda, nie idą na produkcję).
    --no-save – instaluje paczkę bez zapisywania jej w pliku package.json. (do testowania)
    --save-optional – opcjonalna zależność (jeśli instalacja się nie uda, aplikacja nadal zadziała).
    --no-optional - używa się do install i oznacza żeby pobrać wszystkie wymagane paczki do projektu, bez tych z devDependencies
pnpm update - aktualizuje wszystkie paczki wg zasad z package.json
pnpm update <package-name> - aktualizuje tylko wskazaną paczkę
pnpm add express@latest - można aktualizować/dodać paczkę z najnowszą wersją
pnpm add <package-name>@<version> - instalacja konkretnej wersji paczki

pnpm <task-name> - np. pnpm dev (słowo 'run' jest opcjonalne)
{
  "scripts": {
    "start-dev": "node lib/server-development",
    "start": "node lib/server-production"
  }
}

pnpm update --interactive --latest - interaktywne sprawdzanie dostępności nowych paczek