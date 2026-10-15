# Profilowanie aplikacji w Node.js

## 1. Podsumowanie: Wbudowany profiler V8 (`--prof`)
* **Czym jest profilowanie:** Mierzenie zużycia CPU, pamięci i czasu wykonywania funkcji w celu identyfikacji tzw. *wąskich gardeł* (*bottlenecks*).
* **Natywny profiler Node.js:** Wykorzystuje profiler silnika V8 i próbkuje kod w trakcie działania.
  * **Uruchomienie:**
    ```bash
    NODE_ENV=production node --prof app.js
    ```
    Generuje plik logów próbkowania (np. `isolate-0x...-v8.log`).
  * **Przetwarzanie logów do pliku tekstowego:**
    ```bash
    node --prof-process isolate-0x...-v8.log > processed.txt
    ```
* **Analiza raportu:**
  * Podział czasu CPU na kod **JavaScript**, **C++** oraz **Garbage Collector (GC)**.
  * Sekcja `[Bottom up (heavy) profile]` wskazuje konkretne funkcje i ich wywoływaczy.
* **Kluczowy wniosek z przykładu:**
  * Synchroniczne blokowanie Pętli Zdarzeń (np. `crypto.pbkdf2Sync`) drastycznie obniża wydajność.
  * Zamiana na funkcję asynchroniczną (`crypto.pbkdf2`) odblokowuje Event Loop — w przykładzie zwiększyło to przepustowość z ~5 do ~20 żądań/sek (4x szybciej).

---

## 2. Profilowanie w praktyce firmowej (Jak robi się to w branży?)

Wbudowany profiler CLI (`--prof`) jest używany rzadko, ponieważ raport tekstowy jest trudny w analizie przy dużych systemach. W firmach stosuje się nowocześniejsze i bardziej wizualne rozwiązania:

### A. Środowisko Lokalne / Testowe
* **Chrome DevTools (`node --inspect`):** Łączenie przeglądarki z procesem Node.js w celu wygenerowania interaktywnego **Flame Graphu** (wykresu płomieniowego).
* **Clinic.js:** Zestaw dedykowanych narzędzi do diagnozy:
  * `clinic doctor` — automatyczna diagnoza problemu (CPU, I/O, pamięć).
  * `clinic flame` — generowanie wizualnych wykresów płomieniowych.

### B. Środowisko Produkcyjne (APM)
Na produkcji nie odpala się ręcznie profilerów. Używa się narzędzi typu **APM (Application Performance Monitoring)** działających stale w tle:
* **Datadog / New Relic / Dynatrace / Sentry**
* Automatycznie mierzą czasy zapytań (*tracing*), analizują zużycie CPU/RAM i wyłapują regresje wydajnościowe.

---

> **Podsumowanie:** Narzędzie `--prof` jest dobre do nauki działania V8, ale w codziennej pracy używa się **Chrome DevTools / Clinic.js** (lokalnie) oraz **APM** (na produkcji).