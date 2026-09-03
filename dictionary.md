# Słownik danych - #BI_NGO na 25-lecie polskiej Wikipedii

Dokument zawiera opis danych udostępnionych w ramach III edycji projektu #BI_NGO realizowanego we współpracy z Wikimedia Polska.

Znajdziesz tutaj informacje o zawartości poszczególnych plików, znaczeniu kolumn, strukturze danych oraz ograniczeniach, które warto uwzględnić podczas analizy.

[← Powrót do README](README.md)

W tabelach stosujemy następujące oznaczenia typów danych: `string` - tekst, `int` - liczba całkowita, `float` - liczba, która może zawierać część dziesiętną, `date` - data, a `datetime` - data i czas.

Puste pole oznacza brak danych w źródle, a nie wartość równą zero. Przed uzupełnieniem brakujących wartości sprawdź znaczenie danej kolumny i sposób przygotowania zbioru.

W części plików miesiąc zapisano jako pełną datę i czas, np. `2016-01-01T00:00:00.000Z`. Pierwszy dzień miesiąca jest w takim przypadku technicznym oznaczeniem całego miesiąca, a nie informacją, że obserwacja dotyczy wyłącznie tego dnia. Oznaczenie `Z` wskazuje czas UTC.

## Wspólne kolumny

Poniższe kolumny powtarzają się w kilku zbiorach, ale nie wszystkie występują w każdym pliku.

| Kolumna | Typ | Opis |
|---|---|---|
| `month` | `datetime` / `int` | Miesiąc, którego dotyczą dane. W plikach 1-4 i 8 jest zapisany jako pełna data i czas, natomiast w plikach rankingowych jako numer miesiąca od 1 do 12. |
| `year` | `int` | Rok, którego dotyczy miesięczny ranking. |
| `page_id` | `int` | Identyfikator strony w polskiej Wikipedii. Może służyć do rozpoznania tej samej strony w kilku zbiorach. |
| `title` | `string` | Tytuł strony lub artykułu. Tytuł może zmieniać się w czasie. |
| `rank` | `int` | Pozycja strony lub artykułu w rankingu dla danego miesiąca. |
| `views` | `int` | Liczba wyświetleń strony w określonym miesiącu. |
| `total.total` | `int` | Łączna wartość metryki w danym miesiącu. Dokładne znaczenie zależy od pliku. Przykładowo w `3-nowe_rejestracje_monthly.csv` oznacza liczbę nowych zarejestrowanych kont, natomiast w `4-aktywni_edytorzy_monthly.csv` liczbę aktywnych edytorów. |
| `total.content` | `int` | Wartość dotycząca treści znajdujących się w głównej przestrzeni Wikipedii. Dokładne znaczenie zależy od pliku. Przykładowo w `2-nowe_artykuly_monthly.csv` oznacza liczbę nowych artykułów, natomiast w `8-edycje_uzytkownikow_monthly.csv` liczbę wykonanych edycji. |

## 1. `1-ogladalnosc_monthly.csv`

[Przejdź do pliku](dane/1-ogladalnosc_monthly.csv)

Zbiór zawiera miesięczne dane dotyczące oglądalności polskiej Wikipedii, z podziałem na typ ruchu oraz sposób dostępu.

Każdy wiersz odpowiada jednemu miesiącowi i jednej kategorii ruchu określonej w kolumnie `agent`.

### Dostępne kolumny

| Kolumna | Typ | Opis |
|---|---|---|
| `month` | `datetime` | Opis znajduje się w sekcji [Wspólne kolumny](#wspólne-kolumny). |
| `agent` | `string` | Typ ruchu zarejestrowanego w danych, np. `user` lub `spider`. Zobacz dokładną definicję [Typ agenta](#typ-agenta).|
| `total.mobile-app` | `int` | Liczba wyświetleń z aplikacji mobilnej dla danego miesiąca i typu ruchu. |
| `total.desktop` | `int` | Liczba wyświetleń z komputerów dla danego miesiąca i typu ruchu. |
| `total.mobile-web` | `int` | Liczba wyświetleń z mobilnej wersji strony dla danego miesiąca i typu ruchu. |

### Ograniczenia i uwagi

- Liczba wyświetleń nie jest równoznaczna z liczbą osób odwiedzających Wikipedię.
- Jeden użytkownik może wygenerować wiele wyświetleń.
- Puste pole nie oznacza braku ruchu, lecz brak dostępnej wartości w źródle.
- Przed sumowaniem danych należy uwzględnić zarówno miesiąc, jak i typ ruchu zapisany w kolumnie `agent`.
- Sposób klasyfikowania ruchu automatycznego może zmieniać się w czasie, dlatego nie wszystkie kategorie muszą być dostępne dla całego analizowanego okresu.

## 2. `2-nowe_artykuly_monthly.csv`

[Przejdź do pliku](dane/2-nowe_artykuly_monthly.csv)

Zbiór zawiera miesięczną liczbę nowych stron utworzonych w przestrzeni treści polskiej Wikipedii (z wykluczeniem stron technicznych i pomocniczych).

Każdy wiersz odpowiada jednemu miesiącowi.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `month` | `datetime` |
| `total.content` | `int` |

### Ograniczenia i uwagi

- Dane opisują liczbę utworzonych stron, a nie zmianę całkowitej liczby istniejących artykułów.
- Zbiór nie uwzględnia późniejszego usunięcia stron.
- Sumowanie liczby nowych artykułów w czasie nie musi dawać aktualnej liczby artykułów w Wikipedii.
- [Definicja przestrzeni treści](#przestrze%C5%84-tre%C5%9Bci) wynika z metodologii zastosowanej w źródle Wikimedia.

## 3. `3-nowe_rejestracje_monthly.csv`

[Przejdź do pliku](dane/3-nowe_rejestracje_monthly.csv)

Zbiór zawiera miesięczną liczbę nowych kont zarejestrowanych w polskiej Wikipedii.

Założenie konta umożliwia aktywne współtworzenie projektów Wikimedia, m.in. edytowanie artykułów, dodawanie plików do Wikimedia Commons czy uzupełnianie danych w Wikidata.

Każdy wiersz odpowiada jednemu miesiącowi.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `month` | `datetime` |
| `total.total` | `int` |

### Ograniczenia i uwagi

- Rejestracja konta nie oznacza, że użytkownik rozpoczął edytowanie Wikipedii.
- Jeden użytkownik może posiadać więcej niż jedno konto.
- Dane nie opisują aktywności użytkownika po rejestracji.
- Liczba rejestracji nie powinna być interpretowana jako liczba aktywnych edytorów.

## 4. `4-aktywni_edytorzy_monthly.csv`

[Przejdź do pliku](dane/4-aktywni_edytorzy_monthly.csv)

Zbiór zawiera miesięczne dane dotyczące aktywnych edytorów polskiej Wikipedii.

Zgodnie z definicją zastosowaną w źródle aktywny edytor to osoba, która wykonała co najmniej pięć edycji w danym miesiącu.

Każdy wiersz odpowiada jednemu miesiącowi.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `month` | `datetime` |
| `total.total` | `int` |

### Ograniczenia i uwagi

- Metryka nie obejmuje wszystkich osób, które wykonały przynajmniej jedną edycję.
- Ta sama osoba może występować jako aktywny edytor w wielu kolejnych miesiącach.
- Sumowanie miesięcznych wartości nie daje liczby unikalnych edytorów w całym okresie.
- Definicja aktywnego edytora może różnić się od definicji wykorzystywanej w innych źródłach.

## 5. `5-top_1000_artykulow_monthly.csv`

[Przejdź do pliku](dane/5-top_1000_artykulow_monthly.csv)

Zbiór zawiera miesięczne rankingi najczęściej wyświetlanych stron i artykułów polskiej Wikipedii.

Każdy wiersz odpowiada jednej stronie zajmującej określoną pozycję w rankingu w danym miesiącu.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `page_id` | `int` |
| `year` | `int` |
| `month` | `int` |
| `rank` | `int` |
| `title` | `string` |
| `views` | `int` |


### Ograniczenia i uwagi

- Każdy miesiąc stanowi osobny ranking.
- Ten sam artykuł może występować w wielu miesiącach.
- Liczba pozycji w miesięcznym rankingu może być mniejsza niż wartość wskazana w jego nazwie, ponieważ z danych odfiltrowano strony techniczne oraz inne strony, które nie są artykułami. Przykładowo w lipcu 2015 roku ranking TOP 1000 zawierał po zastosowaniu tych filtrów 972 artykuły.
- Zmiana tytułu artykułu nie musi oznaczać zmiany `page_id`.
- Ranking nie zawiera pełnej historii oglądalności wszystkich artykułów - obejmuje wyłącznie strony znajdujące się w TOP 1000.

## 6. `6-polskie_artykuly_w_top_1000_monthly.csv`

[Przejdź do pliku](dane/6-polskie_artykuly_w_top_1000_monthly.csv)

Zbiór zawiera zestawienie artykułów dostępnych wyłącznie w polskojęzycznej Wikipedii, czyli nieposiadających odpowiedników w innych wersjach językowych, które w ciągu ostatnich 11 lat znalazły się w miesięcznych rankingach TOP 1000 najczęściej wyświetlanych stron polskiej Wikipedii.

Informacja o tym, czy dany artykuł posiada odpowiednik w innych wersjach językowych Wikipedii, została sprawdzona w lipcu 2026 roku.

Każdy wiersz odpowiada jednemu artykułowi zajmującemu określoną pozycję w rankingu w danym miesiącu.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `page_id` | `int` |
| `year` | `int` |
| `month` | `int` |
| `rank` | `int` |
| `title` | `string` |
| `views` | `int` |


### Ograniczenia i uwagi

- Zbiór nie obejmuje wszystkich artykułów polskiej Wikipedii.
- Dane powinny być interpretowane jako część rankingu TOP 1000, a nie pełny obraz oglądalności.
- Ten sam artykuł może występować wielokrotnie - osobno dla kolejnych miesięcy.
- Kryterium zaklasyfikowania artykułu jako „polskiego” wynika ze sposobu przygotowania zbioru i powinno zostać uwzględnione podczas interpretacji danych.
- Przy łączeniu z plikiem nr 5 należy sprawdzić unikalność kombinacji `page_id`, `year` i `month`.

## 7. `7-top_100_najczesciej_edytowanych_artykulow_monthly.csv`

[Przejdź do pliku](dane/7-top_100_najczesciej_edytowanych_artykulow_monthly.csv)

Zbiór zawiera miesięczne rankingi najczęściej edytowanych stron i artykułów polskiej Wikipedii.

Każdy wiersz odpowiada jednej stronie zajmującej określoną pozycję w rankingu w danym miesiącu.

### Dostępne kolumny

| Kolumna | Typ | Opis |
|---|---|---|
| `page_id` | `int` | |
| `year` | `int` | |
| `month` | `int` | |
| `rank` | `int` | |
| `title` | `string` | |
| `edits` | `int` |  Liczba edycji strony lub artykułu w danym miesiącu. |


### Ograniczenia i uwagi

- Każdy miesiąc stanowi osobny ranking.
- Ten sam artykuł może pojawiać się w wielu miesiącach.
- Zbiór obejmuje jedynie strony znajdujące się w TOP 100, a nie pełną historię edycji wszystkich artykułów.
- Duża liczba edycji nie musi oznaczać dużej liczby różnych edytorów.
- Liczba edycji nie pozwala samodzielnie ocenić jakości ani zakresu wprowadzonych zmian.

## 8. `8-edycje_uzytkownikow_monthly.csv`

[Przejdź do pliku](dane/8-edycje_uzytkownikow_monthly.csv)

Zbiór zawiera miesięczne dane dotyczące liczby edycji wykonanych w polskiej Wikipedii, z podziałem na typ użytkownika lub edytora oraz przestrzeń strony.

Każdy wiersz odpowiada jednej kategorii edytora w danym miesiącu.

### Dostępne kolumny

| Kolumna | Typ | Opis |
|---|---|---|
| `month` | `datetime` | |
| `editor_type` | `string` | [Typ edytora](#typ-edytora) zgodnie z klasyfikacją zastosowaną w danych źródłowych. W pliku występują między innymi wartości `anonymous`, `user`, `group-bot` i `name-bot`. |
| `total.content` | `int` | Liczba edycji wykonanych w głównej przestrzeni treści. |
| `total.non-content` | `int` | Liczba edycji wykonanych poza główną przestrzenią treści, np. na stronach dyskusji, stronach użytkowników lub stronach technicznych. |

### Ograniczenia i uwagi

- Nazwy kategorii edytorów należy interpretować zgodnie z wartościami występującymi w pliku.
- Wartości `anonymous`, `user`, `group-bot` i `name-bot` są etykietami pochodzącymi ze źródła. Sama próbka danych nie zawiera pełnej metodologii ich klasyfikacji.
- Liczba edycji nie jest równoznaczna z liczbą edytorów.
- Jeden użytkownik może wykonać wiele edycji w tym samym miesiącu.
- Puste wartości w kolumnach liczbowych oznaczają brak dostępnej wartości, a nie zawsze zero edycji.
- Przed połączeniem tego zbioru z innymi plikami miesięcznymi warto sprawdzić i ujednolicić format kolumny `month`.

## 9. `9-biografie.tsv`

[Przejdź do pliku](dane/9-biografie.tsv)

Zbiór zawiera dane dotyczące osób posiadających biografie w polskiej Wikipedii, dla których istnieje odpowiednie powiązanie z Wikidata. Informacje pochodzą z Wikidata oraz polskiej Wikipedii, lipiec 2026.

Każdy wiersz powinien odpowiadać jednej osobie i powiązanemu z nią artykułowi biograficznemu.

Nazwy części kolumn rozpoczynają się od znaku `?`. Jest to zapis pochodzący z eksportu wyników zapytania SPARQL i stanowi część nazwy kolumny w pliku.

### Dostępne kolumny

| Kolumna | Typ | Opis |
|---|---|---|
| `page_id` | `int` | Opis znajduje się w sekcji [Wspólne kolumny](#wspólne-kolumny). |
| `?item` | `string` | Pełny adres elementu osoby w Wikidata. |
| `?article` | `string` | Pełny adres artykułu biograficznego w polskiej Wikipedii. |
| `?name` | `string` | Nazwa lub etykieta osoby pobrana z Wikidata. |
| `?genderLabel` | `string` | Etykieta płci pobrana z Wikidata. |
| `?created_date` | `date` | Data utworzenia artykułu. |
| `?dateOfBirth` | `date` | Data urodzenia pobrana z Wikidata. Jej dokładność zależy od informacji dostępnych w źródle. |
| `?year` | `int` | Rok urodzenia wyodrębniony z daty urodzenia. |
| `?month` | `int` | Miesiąc urodzenia wyodrębniony z daty urodzenia. Może być pusty, jeśli dokładny miesiąc nie jest znany. |
| `?day` | `int` | Dzień urodzenia wyodrębniony z daty urodzenia. Może być pusty, jeśli dokładny dzień nie jest znany. |
| `?decade` | `float` | Dekada urodzenia wyliczona na podstawie dostępnego roku urodzenia. |
| `?century` | `float` | Wiek urodzenia wyliczony na podstawie dostępnego roku urodzenia. |


### Ograniczenia i uwagi

- Zbiór obejmuje wyłącznie osoby, dla których istnieje powiązanie między elementem Wikidata a artykułem w polskiej Wikipedii.
- Brak osoby w zbiorze nie oznacza, że jej biografia nie istnieje. Powodem może być brak lub niepełne powiązanie w danych źródłowych.
- Nie wszystkie osoby mają pełną datę urodzenia.
- Dla części osób dostępny może być wyłącznie rok urodzenia, dlatego kolumny `?month` i `?day` mogą być puste.
- Etykieta osoby może być dostępna w innym języku niż polski albo może być pusta.
- Informacje w Wikidata mogą zawierać więcej niż jedną wartość dla tej samej właściwości.
- Przed analizą warto sprawdzić unikalność `page_id`, `?item` i `?article`.
- Dane opisują artykuły biograficzne oraz informacje zapisane w Wikidata, a nie bezpośrednio popularność lub znaczenie opisywanych osób.

## 10. `10-wyswietlenia_monthly_raw_data`

[Przejdź do folderu](dane/10-wyswietlenia_monthly_raw_data/)

Folder zawiera bardziej szczegółowe dane dotyczące miesięcznej oglądalności stron polskiej Wikipedii.

Dane zostały podzielone na osobne pliki dla kolejnych miesięcy. Okres obserwacji jest zapisany w nazwie pliku w formacie `YYYY-MM`, np. `2016-01.tsv.gz`.

Każdy wiersz odpowiada jednej stronie w miesiącu wskazanym w nazwie pliku.

### Dostępne kolumny

| Kolumna | Typ |
|---|---|
| `page_id` | `int` |
| `views` | `int` |


### Ograniczenia i uwagi

- Każdy plik obejmuje jeden miesiąc.
- Część plików jest zapisana w skompresowanym formacie `.tsv.gz` i przed otwarciem może wymagać rozpakowania albo użycia narzędzia obsługującego pliki gzip.
- Pliki nie zawierają tytułu strony, dlatego podstawowym identyfikatorem jest `page_id`.
- Zbiór może obejmować również strony techniczne lub strony spoza głównej przestrzeni artykułów.
- Przed połączeniem wielu plików należy sprawdzić zgodność ich schematów.
- Przy łączeniu z plikami nr 5-7 należy utworzyć rok i miesiąc na podstawie nazwy pliku.

## Wspólne kolumny i możliwe połączenia

Poniższa tabela pokazuje kolumny występujące w więcej niż jednym zbiorze. Nie stanowi instrukcji wykonania konkretnego połączenia - przed połączeniem danych należy sprawdzić strukturę i unikalność wierszy.

| Kolumna lub zestaw kolumn | Zbiory | Zastosowanie |
|---|---|---|
| `month` | 1-4 i 8 | Porównanie miesięcznych danych po ujednoliceniu formatu daty. |
| `year`, `month` | 5-7 | Porównanie danych rankingowych dla tego samego miesiąca. |
| `page_id` | 5, 6, 7, 9 i 10 | Identyfikacja tej samej strony w różnych zbiorach. |
| `page_id`, `year`, `month` | 5-7 | Łączenie danych dotyczących tej samej strony i miesiąca. |
| `page_id` oraz okres z nazwy pliku | 5-7 i 10 | Łączenie rankingów z bardziej szczegółowymi danymi o wyświetleniach. |
| `title` | 5-7 | Pomocnicze rozpoznawanie artykułu. Tytuł nie powinien być jedynym kluczem, ponieważ może zmieniać się w czasie. |
| `views` | 5, 6 i 10 | Porównanie liczby wyświetleń w miesięcznych danych o oglądalności. |
| `?item` | 9 | Powiązanie danych biograficznych z dodatkowymi danymi z Wikidata. |
| `?article` | 9 | Bezpośrednie przejście do artykułu w polskiej Wikipedii. |

Kolumny `?year` i `?month` w pliku nr 9 odnoszą się do daty urodzenia i nie powinny być łączone z kolumnami `year` i `month` opisującymi okres obserwacji w pozostałych zbiorach.

Przed połączeniem danych warto sprawdzić:

- co reprezentuje jeden wiersz,
- czy wybrany klucz jest unikalny,
- czy połączenie nie powoduje zwielokrotnienia wartości,
- czy oba zbiory obejmują ten sam zakres czasu,
- czy kolumny użyte do połączenia mają zgodny typ i format.

## Glosariusz metryk

### Wyświetlenia strony

Wyświetlenie oznacza zarejestrowane otwarcie strony zgodnie z metodologią Wikimedia.

Liczba wyświetleń:

- nie oznacza liczby unikalnych osób,
- może obejmować wiele wizyt tej samej osoby,
- zależy od sposobu filtrowania ruchu automatycznego,
- może różnić się od wartości prezentowanych w innych narzędziach, jeśli wykorzystują one inną metodologię lub zakres czasu.

### Typ agenta

W danych dotyczących oglądalności mogą występować trzy typy ruchu:

- `user` – ruch zaklasyfikowany jako pochodzący od użytkowników,
- `spider` – ruch generowany przez roboty indeksujące,
- `automated` – pozostały ruch automatyczny, który nie został zaklasyfikowany jako `spider`.

Nie wszystkie typy ruchu są dostępne dla całego analizowanego okresu:

- kategoria `automated` jest dostępna od 2020 roku,
- ruch `spider` dla aplikacji mobilnej (`mobile-app`) jest dostępny od 2019 roku.

Brak wartości dla wcześniejszych okresów nie oznacza braku takiego ruchu, ale brak dostępnych danych dla danej kategorii.

### Sposób dostępu

W pliku nr 1 sposób dostępu jest zapisany w osobnych kolumnach:

- `total.desktop` - dostęp z komputerów,
- `total.mobile-web` - dostęp przez mobilną wersję strony,
- `total.mobile-app` - dostęp przez aplikację mobilną.

### Przestrzeń treści

`content` oznacza strony znajdujące się w głównej przestrzeni treści, przede wszystkim artykuły.

`non-content` oznacza pozostałe przestrzenie, np. strony dyskusji, strony użytkowników, strony techniczne i pomocnicze.

Dokładny zakres zależy od definicji zastosowanej w źródle.

### Aktywny edytor

W pliku `4-aktywni_edytorzy_monthly.csv` aktywny edytor oznacza osobę, która wykonała co najmniej pięć edycji w danym miesiącu.

Nie jest to liczba wszystkich osób, które wykonały przynajmniej jedną edycję.

### Typ edytora

W pliku nr 8 edycje zostały podzielone według kategorii zapisanych w kolumnie `editor_type`.

W danych występują następujące typy edytorów:

- `anonymous` – edycje wykonane przez użytkowników niezalogowanych,
- `user` – edycje wykonane przez zarejestrowanych użytkowników, którzy nie zostali zaklasyfikowani jako boty,
- `group-bot` – edycje wykonane przez zarejestrowane konta należące do grupy botów,
- `name-bot` – edycje wykonane przez zarejestrowane konta, które nie należą do grupy botów, ale ich nazwa wskazuje, że mogą być botami.

Kategorie `group-bot` i `name-bot` pozwalają rozróżnić boty formalnie oznaczone jako boty od kont, które są traktowane jako boty na podstawie ich nazwy.

### Pozycja w rankingu

Kolumna `rank` określa pozycję artykułu lub strony w rankingu dla konkretnego miesiąca.

Pozycji z różnych miesięcy nie należy traktować jako jednego ciągłego rankingu. Każdy miesiąc stanowi osobne zestawienie.

## Źródła danych i licencje

### Skąd pochodzą dane

| Plik | Źródło | Licencja danych |
|---|---|---|
| 1-4 | Wikistats 2 / Wikimedia Analytics API (AQS) | CC0 1.0 |
| 5, 6 | Zrzuty `pageview_complete` (dumps.wikimedia.org), uzupełnione o metadane stron polskojęzycznej Wikipedii | CC0 1.0 |
| 7 | Statystyki edycji polskojęzycznej Wikipedii (Wikimedia Analytics) | CC0 1.0 |
| 8 | Wikistats 2 / Wikimedia Analytics API (AQS) | CC0 1.0 |
| 9 | Wikidata (zapytanie SPARQL) oraz polskojęzyczna Wikipedia | CC0 1.0 (Wikidata), CC BY-SA 4.0 (Wikipedia) |
| 10 | Zrzuty `pageview_complete` (dumps.wikimedia.org) | CC0 1.0 |

Dane o oglądalności i edycjach pochodzące z Wikimedia Analytics są udostępniane na zasadach [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/deed.pl), czyli w domenie publicznej - zobacz [informację licencyjną dumps.wikimedia.org](https://dumps.wikimedia.org/legal.html).

Dane strukturalne z Wikidata są udostępniane na zasadach CC0 1.0 - zobacz [Wikidata:Licensing](https://www.wikidata.org/wiki/Wikidata:Licensing).

Treść artykułów polskojęzycznej Wikipedii jest udostępniana na licencji [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.pl) oraz GFDL. Same tytuły artykułów i identyfikatory stron obecne w tych zbiorach są krótkimi danymi faktograficznymi, ale jeżeli w pracy konkursowej wykorzystasz fragmenty treści artykułów, obowiązuje licencja CC BY-SA 4.0 wraz z wymogiem podania autorstwa.

### Jak cytować dane

Przy publikacji wyników warto podać źródło, zbiór i datę pobrania, na przykład:

- Wikimedia Foundation, *Pageview complete dumps*, dumps.wikimedia.org, dane pobrane w lipcu 2026 (CC0 1.0).
- Wikidata contributors, *Wikidata*, zapytanie SPARQL wykonane w lipcu 2026 (CC0 1.0).
- Wikipedia contributors, *Wikipedia, wolna encyklopedia* (wersja polskojęzyczna), dane pobrane w lipcu 2026 (CC BY-SA 4.0).

Dodatkowo warto podać link do repozytorium projektu #BI_NGO, z którego pochodzą przygotowane zbiory.

### Linki do źródeł

- [Wikistats 2 - polskojęzyczna Wikipedia](https://stats.wikimedia.org/#/pl.wikipedia.org) - interaktywne statystyki, na których opierają się pliki 1-4 i 8.
- [Wikimedia Analytics API - dokumentacja](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/) oraz [punkt dostępowy API](https://wikimedia.org/api/rest_v1/) - jeżeli chcesz pobrać dane samodzielnie lub rozszerzyć ich zakres.
- [Zrzuty `pageview_complete`](https://dumps.wikimedia.org/other/pageview_complete/) i [ich opis](https://dumps.wikimedia.org/other/pageview_complete/readme.html) - źródło plików 5-7 i 10.
- [Wikidata Query Service](https://query.wikidata.org/) - oficjalny punkt wykonywania zapytań SPARQL do Wikidata.
- [Warunki korzystania z projektów Wikimedia](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use) oraz [polityka User-Agent](https://foundation.wikimedia.org/wiki/Policy:Wikimedia_Foundation_User-Agent_Policy) - jeżeli pobierasz dane z API we własnym skrypcie, ustaw opisowy nagłówek `User-Agent` z kontaktem do siebie.

---

Szczegółowe informacje o projekcie, zasadach udziału i zgłoszeniu gotowej pracy znajdziesz w [README](README.md).

Masz pytania dotyczące danych? Napisz do nas - [bingo.wolontariat@gmail.com](mailto:bingo.wolontariat@gmail.com).

Klaudia & Ela & Gosia
