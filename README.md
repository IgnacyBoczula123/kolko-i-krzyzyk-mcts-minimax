# Kółko i Krzyżyk: Implementacja MCTS, Minimax oraz Analiza Wydajności

Projekt, oparty na [artykule naukowym M. C. Fu](./021.pdf), przedstawia implementację, optymalizację i ewaluację algorytmów przeszukiwania drzewa dla gry **kółko i krzyżyk** ($3 \times 3$).

## Zakres zrealizowanych prac
* **Implementacja MCTS:** Zbudowano obiektową strukturę drzewa gry realizującą 4 fazy algorytmu Monte Carlo Tree Search (selekcja UCB1, ekspansja, losowa symulacja, propagacja wsteczna).
* **Wdrożenie algorytmu Minimax:** Stworzono algorytm z karą za głębokość i memoizacją, który generuje pełny słownik optymalnej polityki ruchów (pozwalający na odczyt w czasie $O(1)$).
* **Optymalizacja wydajności:** Za pomocą narzędzi `cProfile` i `pstats` zidentyfikowano wąskie gardła, zoptymalizowano logikę sprawdzania warunków końca gry i przyspieszono losowe dogrywki.
* **Ewaluacja (Benchmark):** Przeprowadzono eksperymenty (12 900 partii) udowadniające własność gry o sumie zerowej oraz wykazujące pełną zbieżność MCTS do bezbłędnej strategii Minimax, identyfikując tzw. "pułapkę płytkiej eksploracji".

## Wykorzystane umiejętności
* **Sztuczna inteligencja i algorytmy:** Przeszukiwanie drzew decyzyjnych, Minimax z memoizacją, MCTS (UCB1), teoria gier (gra o sumie zerowej).
* **Python (Programowanie Obiektowe):** Projektowanie własnych klas, dziedziczenie po typach niemutowalnych, typowanie statyczne.
* **Inżynieria wydajności:** Profilowanie i optymalizacja kodu źródłowego (`cProfile`, `pstats`), redukcja złożoności obliczeniowej.
* **Analiza i wizualizacja danych:** Projektowanie symulacji eksperymentalnych, agregacja wyników (`pandas`) oraz wizualizacja zbieżności algorytmicznej (`matplotlib`).