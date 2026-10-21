# Laboratorium 3 — Szyfrowanie blokowe

**3 zadania po 1 pkt.** Orientacyjnie 75 minut pracy z kodem w ramach 105 minut zajęć. Napisz cały kod samodzielnie w **dowolnym języku**. Użyj biblioteki do AES, PBKDF2 i PKCS#7; nie implementuj tych algorytmów od podstaw. Nie ma kodu do uzupełniania. Wystarczy prosty program bez menu i własnego formatu pliku.

Dozwolone: ISOD, dokumentacja, własne notatki i własny wcześniej napisany kod. Podczas zajęć wyłącz czaty i podpowiedzi LLM. Przy odbiorze wyjaśniasz działanie programu.

## Zadanie 1 — ECB i CBC (około 20 min, 1 pkt)

1. Przygotuj 64 bajty: tekst ASCII `0123456789ABCDEF` powtórzony cztery razy. Klucz AES-128 to 16 bajtów zapisanych szesnastkowo jako `000102030405060708090a0b0c0d0e0f` (nie 32 znaki tekstu).
2. Zaszyfruj dane w ECB oraz CBC tym samym kluczem. Dla CBC wylosuj bezpiecznie IV o długości 16 bajtów. **Wyłącz dopełnienie**: dane są wielokrotnością bloku.
3. Wypisz cztery 16-bajtowe bloki każdego kryptogramu w zapisie szesnastkowym. Sprawdź, które bloki się powtarzają. Odszyfruj oba kryptogramy i porównaj odzyskane bajty z wejściem; dla CBC użyj pierwotnego IV.

**W raporcie:** które bloki były równe w każdym trybie, wynik obu porównań po odszyfrowaniu oraz jedno zdanie wyjaśniające obserwację. Pełny wydruk może pozostać w repozytorium.

## Zadanie 2 — Szyfrowanie z hasłem (około 30 min, 1 pkt)

1. Wybierz testowe hasło. Z jego bajtów UTF-8 wyprowadź klucz: **PBKDF2-HMAC-SHA256, 200000 iteracji, klucz 32 B, losowa sól 16 B**. Są to parametry ćwiczenia. Użyj bezpiecznego generatora losowego także do osobnego IV 16 B.
2. Zaszyfruj AES-CBC z PKCS#7, a następnie odszyfruj i usuń dopełnienie. Zachowaj sól i IV potrzebne do odtworzenia klucza i odszyfrowania; wystarczą zmienne w pamięci.
3. Samodzielnie napisz i uruchom trzy testy: puste dane, 16 bajtów ASCII `0123456789ABCDEF` oraz bajty pliku `wiadomosc.txt`. Sprawdź równość bajtów przed i po operacji oraz długość kryptogramu.
4. Zaszyfruj wiadomość jeszcze raz z tym samym hasłem, ale nową solą i IV. Porównaj kryptogramy.

**W raporcie:** wyniki trzech testów i długości kryptogramów, wynik porównania dwóch szyfrowań oraz jedno zdanie o różnicy między solą a IV. Sól i IV mogą być jawne; CBC nie zapewnia integralności.

## Zadanie 3 — Odzyskanie słabego klucza (około 25 min, 1 pkt)

Plik `slaby_ecb.bin` zaszyfrowano AES-128-ECB z PKCS#7. Klucz to jeden znak ASCII ze zbioru `abcdefghijklmnopqrstuvwxyz0123456789`, powtórzony 16 razy. Tekst jawny jest zwykłym tekstem angielskim.

1. Sprawdź wszystkie 36 kluczy. Każdym odszyfruj pierwsze 4096 bajtów kryptogramu, **bez usuwania dopełnienia z próbki**.
2. Napisz obliczanie entropii bajtów: **H = -suma(p_b * log2(p_b))**, gdzie p_b to udział bajtu b w próbce. Pomijaj bajty o liczności zero. Licz po surowych bajtach, nie po znakach ich zapisu hex.
3. Posortuj kandydatów rosnąco według H i wypisz pięciu najlepszych (znak i H). Najlepszym kluczem odszyfruj cały plik, usuń PKCS#7 i sprawdź czytelność tekstu. Entropia jest wskazówką, nie dowodem poprawności.

**W raporcie:** pięciu kandydatów z H do 4 miejsc po przecinku, odzyskany znak klucza i pierwszy wiersz tekstu. Dodaj jedno zdanie: dlaczego jest tylko 36 kluczy i czemu ranking może zawieść dla danych skompresowanych.

## Oddanie — tylko jeden plik raportu

Oddaj **jeden plik TXT, PDF albo MD**: `nazwisko_nrindeksu_lab03` z odpowiednim rozszerzeniem. Raport do jednej strony powinien zawierać imię i nazwisko, numer indeksu, wybrany język, **link do repozytorium GitHub i identyfikator commitu** oraz wyniki i krótkie odpowiedzi wskazane w trzech zadaniach.

Cały samodzielnie napisany kod umieść na GitHubie. Dodaj krótki README z wymaganymi bibliotekami i komendą uruchomienia. Repozytorium musi być dostępne dla prowadzącego (publiczne albo prywatne z nadanym dostępem). Wyniki pomocnicze możesz umieścić w repozytorium. **Nie oddawaj ZIP-a, kodu ani dodatkowych załączników.** Miejsce i termin oddania raportu poda prowadzący.

## Punktacja

Za każde zadanie: **0,50 pkt** za działający, samodzielnie napisany kod na GitHubie; **0,25 pkt** za wymagane wyniki w raporcie; **0,25 pkt** za poprawne wyjaśnienie działania. Łącznie **3 pkt**. Wybór języka nie wpływa na ocenę.
