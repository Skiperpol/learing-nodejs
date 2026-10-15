ECMAScript (ES6+) w Node.js
Node.js bazuje na silniku V8 z przeglądarki Chrome i dzieli nowe funkcje języka JavaScript na 3 grupy:

Shipping (Stabilne) - włączone domyślnie, nie wymagają żadnych flag.

Staged (Prawie ukończone) - wymagają flagi --harmony (lub --es_staging).
Uwaga: Nie należy ich używać w środowiskach produkcyjnych, bo mogą ulec zmianie przy aktualizacji Node.js.

In Progress (W trakcie tworzenia / bardzo niestabilne) - wymagają indywidualnych flag V8. Służą wyłącznie do testów. Listę można sprawdzić w terminalu: node --v8-options | grep "in progress".

Przydatne narzędzia i komendy:
- sprawdzenie wsparcia funkcji w wersjach Node.js: strona node.green
- sprawdzenie wersji silnika V8 w Twoim Node.js: node -p process.versions.v8