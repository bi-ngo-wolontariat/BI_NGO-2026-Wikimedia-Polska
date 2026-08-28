# BI_NGO 2026 - Wikimedia Polska

## Dane na 25-lecie polskiej Wikipedii

Repozytorium zawiera dane udostępnione w ramach III edycji wolontariatu analitycznego #BI_NGO, organizowanego z okazji 25-lecia polskiej Wikipedii we współpracy z Wikimedia Polska.

Znajdziesz tutaj dziesięć zestawów danych dotyczących między innymi aktywności Wikipedii, jej użytkowników, redaktorów, edycji, popularności artykułów oraz biografii opisanych w polskiej Wikipedii.

Nie musisz korzystać ze wszystkich plików. Możesz wybrać jeden zbiór, połączyć kilka z nich albo uzupełnić analizę o inne publiczne źródła.

## Najważniejsze linki

- [Strona projektu i szczegółowe informacje o udziale](https://bingo.jezykdanych.pl/)
- [Dane do analizy](dane/)
- [Słownik danych](dictionary.md)
- [Formularz zgłoszenia gotowej wizualizacji](https://docs.google.com/forms/d/e/1FAIpQLSebfpsgvPVQEbwd8dk5UdVomL-DNcmrggKelGgwzLzvf6ebgw/viewform)
- [Materiały graficzne i identyfikacja wizualna](https://bingo.jezykdanych.pl/)

Gotową pracę należy zgłosić za pomocą formularza najpóźniej **3 października 2026 roku do końca dnia**.

## O projekcie #BI_NGO

#BI_NGO to wolontariat analityczny, w którym udział mogą wziąć zarówno osoby stawiające swoje pierwsze kroki w analizie danych, jak i te z większym doświadczeniem. Pracę możesz przygotować w dowolnym narzędziu i w wybranej przez siebie formie - jako dashboard, raport, prezentację, infografikę, plakat lub inną wizualizację danych.

Więcej informacji o projekcie oraz zasadach udziału znajdziesz na [stronie #BI_NGO](https://bingo.jezykdanych.pl/).

## Dostępne dane

Dane znajdują się w folderze [`dane`](dane/).

Zbiory od 1 do 8 zawierają głównie dane zagregowane miesięcznie i mogą być dobrym punktem startowym dla osób początkujących. Plik z biografiami oraz szczegółowe dane o wyświetleniach mają bardziej rozbudowaną strukturę i mogą wymagać dodatkowego przygotowania.

Każdy plik może być analizowany osobno. Część zbiorów posiada również wspólne kolumny, dzięki którym możliwe jest ich łączenie. Szczegółowe informacje znajdziesz w [słowniku danych](dictionary.md).

### Ogólna aktywność Wikipedii

- [`1-ogladalnosc_monthly.csv`](dane/1-ogladalnosc_monthly.csv) - miesięczne dane dotyczące oglądalności polskiej Wikipedii, z podziałem między innymi na sposób dostępu oraz typ ruchu.
- [`3-nowe_artykuly_monthly.csv`](dane/3-nowe_artykuly_monthly.csv) - miesięczna liczba nowych artykułów i stron utworzonych w przestrzeni treści polskiej Wikipedii.

### Użytkownicy i redaktorzy

- [`2-nowe_rejestracje_monthly.csv`](dane/2-nowe_rejestracje_monthly.csv) - miesięczna liczba nowych kont zarejestrowanych w polskiej Wikipedii.
- [`4-aktywni_edytorzy_monthly.csv`](dane/4-aktywni_edytorzy_monthly.csv) - miesięczne dane dotyczące aktywnych edytorów polskiej Wikipedii. Zgodnie z definicją źródłową aktywny edytor to osoba, która wykonała co najmniej pięć edycji w miesiącu.

### Rankingi oglądalności

- [`5-top_1000_artykulow_monthly.csv`](dane/5-top_1000_artykulow_monthly.csv) - miesięczne rankingi najczęściej wyświetlanych stron i artykułów polskiej Wikipedii.
- [`6-polskie_artykuly_w_top_1000_monthly.csv`](dane/6-polskie_artykuly_w_top_1000_monthly.csv) - zestawienie polskich artykułów pojawiających się w miesięcznych rankingach TOP 1000.

### Edycje

- [`7-top_100_najczesciej_edytowanych_artykulow_monthly.csv`](dane/7-top_100_najczesciej_edytowanych_artykulow_monthly.csv) - miesięczne rankingi 100 najczęściej edytowanych artykułów.
- [`8-edycje_uzytkownikow_monthly.csv`](dane/8-edycje_uzytkownikow_monthly.csv) - miesięczne dane dotyczące liczby edycji, z podziałem na typ edytora lub użytkownika.

### Dane biograficzne

- [`9_biografie.tsv`](dane/9_biografie.tsv) - dane dotyczące osób posiadających biografie w polskiej Wikipedii. Zbiór łączy informacje pochodzące z Wikidata i polskiej Wikipedii z informacjami o utworzeniu artykułu oraz jego oglądalności.

Plik zawiera między innymi informacje o dacie urodzenia, płci, identyfikatorach Wikidata i Wikipedii, dacie utworzenia artykułu oraz statystykach jego wyświetleń.

### Szczegółowe dane o wyświetleniach

- [`10-wyswietlenia_monthly_raw_data`](dane/10-wyswietlenia_monthly_raw_data/) - folder zawierający bardziej szczegółowe dane dotyczące miesięcznej oglądalności stron i artykułów.

Dane zostały udostępnione w osobnych plikach dla kolejnych miesięcy. Mogą zostać wykorzystane samodzielnie lub połączone z innymi zbiorami na podstawie wspólnych kolumn, takich jak identyfikator strony i miesiąc obserwacji.


## Metodologia, słownik danych i ograniczenia

Szczegółowy opis zbiorów, kolumn, typów danych oraz najważniejszych definicji znajduje się w pliku [`dictionary.md`](dictionary.md).

Przed rozpoczęciem analizy zwróć szczególną uwagę na następujące zasady:

- puste pole oznacza brak danych w źródle, a nie wartość równą zero,
- poszczególne zbiory mogą obejmować różne zakresy czasowe,
- nie wszystkie wymiary i podziały są dostępne dla całego analizowanego okresu,
- miesięczne rankingi są niezależnymi zestawieniami dla kolejnych miesięcy,
- liczba pozycji po zastosowaniu filtrów może być mniejsza niż wartość wskazana w nazwie rankingu,
- nazwy artykułów mogą zmieniać się w czasie, dlatego przy łączeniu danych warto w pierwszej kolejności korzystać z identyfikatora `page_id`,
- ta sama nazwa kolumny nie zawsze oznacza ten sam poziom agregacji,
- dane biograficzne obejmują wyłącznie osoby, dla których udało się odnaleźć odpowiednie powiązania między Wikidata i polską Wikipedią.

Dane mogą zawierać uproszczenia wynikające ze sposobu działania źródeł, dostępności danych oraz zastosowanej metody agregacji. Przy interpretacji wyników warto uwzględnić kontekst konkretnego zbioru i opisane w słowniku ograniczenia.

## Źródła danych i zasady wykorzystania

Dane pochodzą z publicznych źródeł dostępnych w ekosystemie Wikimedia, w tym między innymi z:
- Wikistats,
- Wikimedia Analytics i Wikimedia AQS,
- Wikidata,
- polskiej Wikipedii,
- publicznych danych dotyczących wyświetleń stron.

Dane mogą być wykorzystywane do przygotowania analiz i wizualizacji w ramach projektu. Możesz również połączyć je z innymi publicznie dostępnymi źródłami.

Przy publikacji pracy:

- wskaż wykorzystane źródła danych,
- zaznacz, które dane pochodzą z repozytorium #BI_NGO,
- dodaj źródła danych zewnętrznych, jeśli zostały wykorzystane,

## Dodatkowe źródła

Dane udostępnione w repozytorium możesz uzupełnić o informacje pochodzące z innych publicznych źródeł. Przydatne mogą być między innymi:

- [Wikistats](https://stats.wikimedia.org/) - statystyki dotyczące projektów Wikimedia,
- [Pageviews Analysis](https://pageviews.wmcloud.org/) - narzędzie do analizy oglądalności wybranych artykułów,
- [Wikidata Query Service](https://query.wikidata.org/) - narzędzie umożliwiające pobieranie danych z Wikidata za pomocą zapytań SPARQL,
- [Wikimedia Commons](https://commons.wikimedia.org/) - repozytorium publicznie dostępnych grafik, zdjęć i innych materiałów,
- [Wikimedia Downloads](https://dumps.wikimedia.org/) - publiczne zrzuty danych projektów Wikimedia.

## Powodzenia

Czekamy na Twoje wizualizacje 💛

Masz pytania? Napisz do nas - [bingo.wolontariat@gmail.com](mailto:bingo.wolontariat@gmail.com).

Klaudia & Ela & Gosia
