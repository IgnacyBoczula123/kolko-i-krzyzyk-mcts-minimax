# Kółko i Krzyżyk: Implementacja MCTS, Minimax oraz Analiza Wydajności

Projekt powstał na podstawie opracowania naukowego *[Tytuł / Link do artykułu]* i przedstawia kompletną implementację algorytmów przeszukiwania drzewa gry dla kółka i krzyżyka ($3 \times 3$), ich optymalizację oraz eksperymentalną ewaluację skuteczności.

---

## Główne elementy projektu

### 1. Architektura obiektowa i drzewiasta (MCTS z dogrywką)
* **Niemutowalny stan planszy (`State`):** Klasa dziedzicząca po wbudowanym typie `tuple`, wykorzystująca wycinanie (*slicing*) do szybkiego generowania kolejnych stanów oraz umożliwiająca hashowanie planszy w słownikach[cite: 10].
* **Drzewo przeszukiwań (`MCTS_Node`):** Obiektowa struktura drzewa przechowująca relacje rodzic–dziecko, liczbę odwiedzin (`visits`) oraz skumulowany wynik (`total_score`)[cite: 10].
* **Pełny cykl MCTS (`mcts_search`):** Implementacja 4 faz algorytmu: selekcji opartej na wzorze **UCB1**, ekspansji węzłów, losowej dogrywki (*rollout* / `game_simulate`) oraz wstecznej propagacji wyników (`backpropagate`)[cite: 10].

### 2. Wykazanie własności gry o sumie zerowej
* Zarówno w fazie propagacji wstecznej MCTS (`current_result = 1 - current_result`), jak i w eksperymentach symulacyjnych, zysk jednego gracza odpowiada dokładnie stracie drugiego[cite: 10].
* Starcia dwóch silnych agentów MCTS ($m = 1000$ vs $n \to 1000$ iteracji) zbiegają do równowagi na poziomie **50% punktów**[cite: 10].
* Starcia MCTS przeciwko bezbłędnemu agentowi Minimax prowadzą wyłącznie do remisów lub porażek MCTS (krzywe % remisów i % porażek są swoim lustrzanym odbiciem i sumują się do 100%)[cite: 10].

### 3. Optymalizacja i profilowanie kodu (`cProfile`)
* **Optymalizacja sprawdzania końca gry:** Zamiast każdorazowego skanowania wszystkich 8 linii wygrywających, funkcja `game_result` weryfikuje wyłącznie linie krzyżujące się w punkcie ostatniego ruchu (`LINES_FOR_MOVE[last_move]`)[cite: 10].
* **Szybka symulacja (*rollout*):** Funkcja `game_simulate` operuje na mutowalnej liście i przetasowanej kolejce wolnych pól (`random.shuffle`)[cite: 10].
* **Profilowanie wąskich gardeł:** Analiza przy użyciu `cProfile` i `pstats` wykazała, że przy ponad 782 mln wywołań funkcji w 2000 symulowanych partiach najwięcej czasu procesora pochłaniają obliczanie `UCB` (~23.9%), `game_result` (~14.7%) oraz inicjalizacja węzłów `__init__` (~11.3%)[cite: 10].

### 4. Implementacja Minimax i ewaluacja zbieżności MCTS
* **Minimax z karą za głębokość (`10 - d`):** Algorytm znajduje ruchy prowadzące do najszybszego zwycięstwa lub najdłuższej obrony, a dodatkowo buduje tablicę optymalnej polityki (`policy_dict` obejmujący 9 040 stanów), co skraca czas wyboru ruchu w trakcie symulacji do $O(1)$[cite: 10].
* **Benchmark MCTS vs Minimax (12 900 partii):**
  * **1–5 iteracji:** 100% porażek MCTS (zbyt mały budżet, by przetestować wszystkie 9 ruchów otwarcia)[cite: 10].
  * **10–90 iteracji:** Szybki wzrost skuteczności do **~81% remisów** dzięki statystycznej przewadze pól centralnych i narożnych[cite: 10].
  * **100–250 iteracji:** Chwilowy spadek do **~57% remisów** (*pułapka płytkiej eksploracji* — losowe symulacje faworyzują ryzykowne ruchy skuteczne przeciwko losowemu graczowi, ale karane przez Minimaxa)[cite: 10].
  * **Od ~920 iteracji:** Pełna zbieżność do **100% remisów** (0% błędów taktycznych)[cite: 10].

---

## Kluczowe umiejętności wykorzystane w projekcie
* **Algorytmy i struktury danych:** Implementacja drzewa decyzyjnego MCTS (wzór UCB1, selekcja, ekspansja, propagacja wsteczna) oraz rekurencyjnego algorytmu Minimax z memoizacją[cite: 10].
* **Programowanie obiektowe w Pythonie (OOP):** Projektowanie własnych klas (`MCTS_Node`), dziedziczenie po typach niemutowalnych (`State(tuple)`, nadpisywanie `__new__` i `__repr__`) oraz typowanie statyczne (`typing`)