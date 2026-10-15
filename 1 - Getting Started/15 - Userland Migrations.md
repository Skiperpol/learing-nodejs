# Node.js Userland Migrations

## 1. Czym są Userland Migrations?

- **Pojęcie:** Wbudowana we współpracę z platformą **Codemod** inicjatywa Node.js oferująca gotowe skrypty (codemody) do automatycznej aktualizacji kodu użytkownika (*userland*).
- **Oficjalne pakiety:** Publikowane w rejestrze Codemod w przestrzeni `@nodejs` (tworzone lub weryfikowane przez członków zespołu Node.js).
- **Główny cel:** Ułatwienie deweloperom przechodzenia na nowsze wersje Node.js poprzez automatyczne dostosowanie kodu do wycofywanych funkcji (*deprecations*) oraz krytycznych zmian (*breaking changes*).

---

## 2. Jak uruchomić codemod?

Migrację wykonuje się bezpośrednio w terminalu za pomocą komendy `npx`:

```bash
npx codemod <nazwa-codemoda>
```

**Przykład:**

```bash
npx codemod @nodejs/import-assertions-to-attributes
```

---



## 3. Dobre praktyki przy migracji

1. **Osobna gałąź w Git:** Zawsze uruchamiaj migrację na nowym *branchu*, aby bezpiecznie przeanalizować wprowadzone zmiany przed połączeniem z głównym kodem.
2. **Przegląd kodu (Review):** Ręcznie przejrzyj zmieniony kod i upewnij się, że migracja nie wygenerowała niepożądanych efektów ubocznych.
3. **Testy:** Uruchom całą suitę testową, aby sprawdzić, czy aplikacja działa poprawnie.
4. **Linter i formatowanie:** Po wykonaniu skryptu sformatuj kod (np. Prettierem) i uruchom linter (np. ESLint), aby zachować standardy projektu.

---



## 4. Rejestr i zasoby

- **Rejestr Codemod:** Zawiera pełną listę skryptów (niektóre dotyczą ogólnych wzorców kodu, a nie tylko konkretnych wersji Node.js).
- **Tablica na GitHubie:** Postępy prac, przewodniki migracyjne oraz zadania do dołączenia się do projektu (sekcja "todo") można śledzić na oficjalnej tablicy projektowej GitHub zespołu *Node.js Userland Migrations*.

