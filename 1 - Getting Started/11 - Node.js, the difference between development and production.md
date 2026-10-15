Node.js: Development vs Production i antywzorzec NODE_ENV

Zasada ogólna: Node.js sam w sobie nie ma osobnych ustawień produkcyjnych, ale wiele bibliotek z npm sprawdza NODE_ENV. Dlatego na produkcji (i stagingu) zawsze należy ustawiać NODE_ENV=production dla optymalizacji.

4 standardowe środowiska:
- Development (Deweloperskie / lokalne)
- Testing (Testowe)
- Staging (Przedprodukcyjne – kopia produkcji do ostatecznych testów)
- Production (Produkcyjne – dla użytkowników końcowych)

Dlaczego uzależnianie logiki od NODE_ENV to antywzorzec?
Pisanie instrukcji warunkowych w kodzie na podstawie środowiska (np. if (NODE_ENV === 'development')) sprawia, że kod zachowuje się inaczej na poszczególnych etapach. W efekcie testy mogą przechodzić na środowisku deweloperskim lub testowym, a aplikacja wyłoży się na produkcji.

Aby wyeliminować uzależnianie logiki kodu od NODE_ENV, stosuje się trzy zasady:
- używać flag funkcjonalności (Feature Flags): Zamiast sprawdać nazwę środowiska (np. if (NODE_ENV === 'dev')), stworzyć dedykowaną zmienną opisującą konkretną funkcję (np. ENABLE_MOCK_PAYMENTS=true).
- zarządzać konfiguracją przez zmienne środowiskowe (Twelve-Factor App): Wszelkie zmienne wartości (poziomy logowania, adresy URL baz danych, klucze API) wyciągać z powiązanych zmiennych środowiskowych, a nie z warunków w kodzie.
- trzymać NODE_ENV=production na Stagingu i Produkcji: Pozwolić NODE_ENV pełnić tylko jedną rolę, a dokładniej informować zewnętrzne biblioteki (np. Express, React), by włączyły produkcyjne optymalizacje wydajnościowe.