# Node.js z WebAssembly (Wasm)

## 1. Czym jest WebAssembly?

- **Pojęcie:** Niskopoziomowy, wysokowydajny kod przypominający asembler, kompilowany z języków takich jak C/C++, Rust czy AssemblyScript.
- **Wsparcie:** Działa natywnie we wszystkich głównych przeglądarkach oraz w środowisku **Node.js** (poprzez globalny obiekt `WebAssembly`).
- **Formaty plików:**
  - `.wasm` — skompilowany plik binarny (**WebAssembly Module**).
  - `.wat` — czytelna dla człowieka reprezentacja tekstowa (**WebAssembly Text**).

---

## 2. Kluczowe pojęcia (Key Concepts)


| Pojęcie      | Opis                                                                           |
| ------------ | ------------------------------------------------------------------------------ |
| **Module**   | Skompilowany plik binarny (`.wasm`).                                           |
| **Memory**   | Dynamicznie rozszerzalna tablica bajtów (`ArrayBuffer`).                       |
| **Table**    | Rozszerzalna tablica referencji przechowywana poza główną pamięcią (`Memory`). |
| **Instance** | Uruchomiona instancja modułu połączona z jego pamięcią, tablicami i zmiennymi. |


---



## 3. Generowanie plików `.wasm`

Pliki binarne można tworzyć na kilka sposobów:

- **AssemblyScript:** Najprostsze podejście dla deweloperów JS/TS (składnia zbliżona do TypeScriptu).
- `wasm-pack`**:** Dedykowany dla aplikacji pisanych w **Runcie**.
- **Emscripten:** Dedykowany dla kodu w **C/C++**.
- **Narzędzia CLI (np.** `wabt`**):** Ręczne pisanie formatu tekstowego `.wat` i konwersja na `.wasm`.

---



## 4. Użycie w Node.js (Przykładowy kod)

Aby wykonać kod WebAssembly w Node.js, wczytuje się plik `.wasm` do bufora, a następnie używa metody `WebAssembly.instantiate()`:

```javascript
const fs = require('node:fs');

// 1. Wczytanie pliku binarnego .wasm
const wasmBuffer = fs.readFileSync('./add.wasm');

// 2. Instancjalizacja modułu
WebAssembly.instantiate(wasmBuffer).then(wasmModule => {
  // 3. Wyciągnięcie wyeksportowanej funkcji
  const { add } = wasmModule.instance.exports;
  
  // 4. Wywołanie funkcji skompilowanej np. z C/Rusta
  const sum = add(5, 6);
  console.log(sum); // Wynik: 11
});
```



## 5. Komunikacja z systemem operacyjnym (OS) i WASI

- Kod WebAssembly uruchamiany jest w bezpiecznej piaskownicy (sandbox) i nie ma bezpośredniego dostępu do systemu operacyjnego (np. do systemu plików czy sieci).
- Do interakcji z OS wykorzystuje się interfejs WASI (WebAssembly System Interface) oraz dedykowane środowiska uruchomieniowe (np. Wasmtime lub wbudowany w Node.js moduł node:wasi).

---

> **Podsumowanie:** WebAssembly pozwala uruchamiać w Node.js kod napisany w C++/Runcie z niemal natywną prędkością, co przydaje się przy operacjach wymagających dużej mocy obliczeniowej (np. przetwarzanie wideo, kryptografia, skomplikowana matematyka).

