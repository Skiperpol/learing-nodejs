Debugowanie Node.js
Domyślnie Node.js otwiera port debugera na 127.0.0.1:9229.

node --inspect app.js - włącza debuger i od razu uruchamia kod.
node --inspect-brk app.js - włącza debuger, ale zatrzymuje wykonywanie kodu na pierwszej linijce (pozwala ustawić breakpointy zanim cokolwiek się wykona).
node --inspect-wait app.js - włącza debuger i czeka na podłączenie klienta przed rozpoczęciem wykonywania.
node inspect app.js - uruchamia prosty debuger tekstowy wprost w terminalu (CLI).

Bezpieczeństwo:
Debuger daje pełny dostęp do systemu, dlatego też osoba/program podłączony do portu debugowania może wykonać dowolny kod na danym komputerze/serwerze.
Nie wolno wystawiać portu na publiczny IP (0.0.0.0), jeśli trzeba zdebugować serwer zdalny, warto uruchomić go lokalnie (127.0.0.1), a połączyć się przez Tunel SSH: ssh -L 9221:localhost:9229 user@remote-server.com

Gdzie można debugować?
VS Code - Nie trzeba ręcznie pisać --inspect. Wystarczy nacisnąć F5 lub stworzyć plik .vscode/launch.json, a VS Code sam podepnie się pod proces.
Chrome DevTools - Po wpisaniu node --inspect app.js, należy otworzyć w przeglądarce chrome://inspect i kliknąć "Open dedicated DevTools for Node".
JetBrains (WebStorm) - Domyślnie obsługuje --inspect po kliknięciu ikony debugowania (zielony robaczek).

Rzadko kiedy wpisujesz ręcznie node --inspect w terminalu. W VS Code wystarczy kliknąć przycisk "Run and Debug" (lub F5), stawić kropkę (breakpoint) przy linii kodu i przeglądać zmienne w interfejsie IDE.