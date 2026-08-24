# Słownik danych — #BI_NGO na 25-lecie Wikipedii

## Pliki statystyczne Wikistats (CSV)

Trzon nagłówka większości plików jest wspólny — poniżej raz, potem per plik tylko kolumna wartości i niuanse.

**Kolumny wspólne**

| kolumna | typ | opis |
|---|---|---|
| `page_id` | int | Unikalny identyfikator treści. |
| `month` | datetime | Miesiąc opisywany wierszem, ISO `YYYY-MM-01T00:00:00.000Z`. |
| `total.total` | int | Wartość metryki (całość) - znaczenie zależy od pliku. |
| `total.content` / `total.non-content` | int | Wartość dla przestrzeni treściowych / nietreściowych. |
| `total.desktop` / `total.mobile-web` / `total.mobile-app` | int | Odsłony wg **metody dostępu** (`mobile-app` bywa puste, np. dla `spider` we wczesnych latach). |
| `total.user` / `total.spider` / `total.automated` | int | Odsłony wg **typu agenta** (człowiek / deklarowany bot / heurystycznie automatyczny). |


### Oglądalność wg typu agenta
Rozróżnienie `spider` / `automated` wprowadzono w 1. połowie 2020 r.; dla wcześniejszych miesięcy `automated` jest puste.

- **`1-oglodalnosc_monthly.csv`** — jeden plik agregujący wszystkie typy agentów. Kolumny: `month, agent, total.mobile-app, total.desktop, total.mobile-web`. Kolumna `agent` może przyjmować wartości `user` / `spider` / `automated`. Trzy `total.*` = podział odsłon danego agenta wg metody dostępu. Plik ma format „długi" (2 wiersze/miesiąc do 2020, potem 3).

### Użytkownicy, redaktorzy, nowe strony
- **`2-nowe_rejestracje_monthly.csv`** — nowo zarejestrowani użytkownicy (`total.total`); realnie dane od IX 2005. *Rozbieżność do wyjaśnienia:* suma ≈ 750 tys. vs ≈ 1,5 mln w statystykach plwiki.
- - **`3-nowe_artykuly_monthly.csv`** — nowe strony w przestrzeni treściowej (`total.content`; artykuły ns0).
- Liczby nowych stron **nie uwzględniają usunięć**, więc suma przekracza bieżącą liczbę artykułów. Dok.: `meta.wikimedia.org/wiki/Research:Wikistats_metrics/New_pages`.
- **`4-aktywni_edytorzy_monthly.csv`** — aktywni edytorzy (≥ 5 edycji/mies.). `month, total.total,`; `total.total` = liczba edytorów.

### Rankingi oglądalności
- **`5-top_1000_artykulow_monthly.csv`** — miesięczne rankingi TOP-1000. Kolumny: `page_id, year, month, rank, title, views`. `month` jako liczba (`7`), `title` ze spacjami. Po odsianiu Strony głównej i stron specjalnych zostaje ok. 990 artykułów/miesiąc.
- **`6-polskie_artykuly_w_top_1000_monthly.csv`** — ta sama struktura; artykuły kiedykolwiek w TOP-1000, obecnie występujące **wyłącznie** w plwiki (ok. 3300).

### Edycje
- **`7-top_100_najczesciej_edytowanych_artykulow_monthly.csv`** — miesięczne rankingi najczęściej edytowanych stron: `page_id, year, month, rank, title, edits`. Brak wersji rocznej; wczesne miesiące i dalsze pozycje bywają mało interesujące (dużo stron technicznych).
- **`8-edycje_uzytkownikow_monthly.csv`** — miesięczne edycje. Kolumny: `month, editor_type, total.content, total.non-content`. `editor_type` ∈ {`user` (zarejestrowani), `anonymous` (anonimowi), `group-bot` (oficjalne boty), `name-bot` (heurystycznie uznane za bota)}; `total.content`/`total.non-content` = edycje wg typu strony. Format „długi" (kilka wierszy/miesiąc). **⚠ Defekt danych:** kolumny `month` są zniekształcone (np. `2001--0-9-T00:00:00.000Z` zamiast `2001-09-01T...`).

### Oglądalność, surowe dane
W folderze `views_raw_data` znajdują się surowe dane zawierające informacje o oglądalności. Na każdy miesiąc jest osobny spakowany plik z danymi (od 2015-12) zawierający dwie kolumny: `page_id` oraz `views`. Do artykułu za pomocą jego `page_id` można się odwołać za pomocą adresu `<https://pl.wikipedia.org/w/index.php?curid=XXX>`.

---





Opis kolumn w plikach udostępnianych uczestnikom. Jeden wspólny słownik dla wszystkich zbiorów; każdy plik ma osobną sekcję.

## Konwencje ogólne

- **Typy:** `string`, `int`, `float`, `date` (`YYYY-MM-DD`), `datetime` (ISO 8601).
- **Puste pole** oznacza brak danych w źródle, a nie zero.
- **Daty w plikach Wikistats** (`month`, `timeRange.start`, `timeRange.end`) mają format ISO `YYYY-MM-01T00:00:00.000Z`; `month` to pierwszy dzień opisywanego miesiąca (UTC).
- **Ziarno czasowe** plików statystycznych jest miesięczne (jeden wiersz = jeden miesiąc), o ile w sekcji pliku nie zaznaczono inaczej.
- **Kolumny `total.*`** pochodzą ze spłaszczenia JSON-a z Wikistats/AQS (obiekt `total` z podkluczami), stąd kropka w nazwie: `total.desktop`, `total.content` itd.
- **Źródła:** Wikidane (SPARQL: `query.wikidata.org`, `qlever.dev/wikidata`), plwiki (API / pageviews) oraz Wikistats/AQS (`stats.wikimedia.org`; dokumentacja metryk na `meta.wikimedia.org`).
- Nazwy kolumn pozostają w oryginale (po angielsku); opisy po polsku.

---

## `biographies_enriched.tsv`

Biografie osób obecnych w polskojęzycznej Wikipedii, zbudowane na podstawie **Wikidanych** (nie plwiki), wzbogacone o datę utworzenia artykułu i statystyki odsłon pobierane per artykuł z plwiki. Powstaje z `biographies_1st_year_and_up.tsv` (surowy wynik SPARQL): `?item` → `qid`, `?name` zastąpione realnym `title` ze strony plwiki, reszta przeniesiona, plus kolumny `created_*` i `views_*`.

**Zakres zbioru i istotne zastrzeżenia:**

- Zbiór obejmuje osoby **urodzone w naszej erze (rok ≥ 1)** i **o znanej dacie urodzenia**. Osoby sprzed roku 1 oraz bez daty urodzenia są pominięte — łączna liczba biogramów w plwiki jest więc istotnie większa niż liczba wierszy tutaj.
- Lista powstała z Wikidanych: biogram istniejący w plwiki, ale bez powiązanego elementu Wikidata, nie trafia do zbioru (por. `no_sitelinks.tsv`).
- **Jeden wiersz na osobę.** Duplikaty wynikające z wielu dat urodzenia usunięto (pozostawiono jedną, losową wartość). To samo dotyczy wielu wartości płci.
- `month` / `day` bywają puste przy ograniczonej precyzji daty urodzenia w Wikidata (np. sama precyzja roczna → `dateOfBirth` = `YYYY-01-01`, `month`/`day` puste).
- `created_ts`, `created_date` oraz wszystkie kolumny `views_*` pochodzą z plwiki (odpytywanie per artykuł), nie z Wikidanych.
- **Uwaga porządkowa:** wartości `article` zachowują nawiasy `<...>` z eksportu WDQS (np. `<https://pl.wikipedia.org/wiki/Artaw>`) — do usunięcia, jeśli potrzebny goły URL.

| kolumna | typ | opis | zakres okna (months_covered) |
|---|---|---|---|
| `article` | string | Pełny URL artykułu w plwiki (sitelink), w nawiasach `<...>`. |  |
| `genderLabel` | string | Etykieta płci z Wikidata (P21), np. `mężczyzna`, `kobieta`, `genderfluid`. Może być pusta. |  |
| `dateOfBirth` | date | Data urodzenia (P569) w formacie `YYYY-MM-DD` (przycięta do daty; w plikach surowych bywa `...T00:00:00Z`). |  |
| `year` | int | Rok urodzenia. |  |
| `month` | int | Miesiąc urodzenia; pusty przy precyzji rocznej. |  |
| `day` | int | Dzień urodzenia; pusty przy precyzji rocznej lub miesięcznej. |  |
| `decade` | float | Dekada urodzenia = rok zaokrąglony w dół do 10 (`⌊year/10⌋×10`; rok 25 → `20.0`, rok 1980 → `1980.0`). |  |
| `century` | float | Wiek (stulecie) w konwencji historycznej = `⌊(year−1)/100⌋+1` (rok 25 → `1.0`, rok 1980 → `20.0`). |  |
| `qid` | string | Identyfikator Wikidanych (np. `Q42`); wyodrębniony z `?item`. |  |
| `title` | string | Tytuł artykułu w plwiki (czytelny tytuł strony, nie etykieta z Wikidanych). |  |
| `page_id` | int | `page_id` w plwiki (namespace 0). |  |
| `created_ts` | string | Znacznik utworzenia strony w formacie MediaWiki `YYYYMMDDHHMMSS` (UTC). |  |
| `created_date` | date | Data utworzenia strony `YYYY-MM-DD`. |  |
| `views_median_month_y1` | float | Mediana miesięcznych sum odsłon w ostatnim roku. | 2025-07..2026-06 |
| `views_daily_y1` | float | Średnia dzienna liczba odsłon, 1. rok wstecz. | 2025-07..2026-06 |
| `months_y1` | int | Liczba pełnych miesięcy użytych do wyliczenia, 1. rok wstecz. | 2025-07..2026-06 |
| `views_daily_y2` | float | Średnia dzienna liczba odsłon, 2. rok wstecz. | 2024-07..2025-06 |
| `months_y2` | int | Liczba pełnych miesięcy, 2. rok wstecz. | 2024-07..2025-06 |
| `views_daily_y3` | float | Średnia dzienna liczba odsłon, 3. rok wstecz. | 2023-07..2024-06 |
| `months_y3` | int | Liczba pełnych miesięcy, 3. rok wstecz. | 2023-07..2024-06 |
| `views_daily_y4` | float | Średnia dzienna liczba odsłon, 4. rok wstecz. | 2022-07..2023-06 |
| `months_y4` | int | Liczba pełnych miesięcy, 4. rok wstecz. | 2022-07..2023-06 |
| `views_daily_y5` | float | Średnia dzienna liczba odsłon, 5. rok wstecz. | 2021-07..2022-06 |
| `months_y5` | int | Liczba pełnych miesięcy, 5. rok wstecz. | 2021-07..2022-06 |
| `views_daily_y6` | float | Średnia dzienna liczba odsłon, 6. rok wstecz. | 2020-07..2021-06 |
| `months_y6` | int | Liczba pełnych miesięcy, 6. rok wstecz. | 2020-07..2021-06 |
| `views_daily_y7` | float | Średnia dzienna liczba odsłon, 7. rok wstecz. | 2019-07..2020-06 |
| `months_y7` | int | Liczba pełnych miesięcy, 7. rok wstecz. | 2019-07..2020-06 |
| `views_daily_y8` | float | Średnia dzienna liczba odsłon, 8. rok wstecz. | 2018-07..2019-06 |
| `months_y8` | int | Liczba pełnych miesięcy, 8. rok wstecz. | 2018-07..2019-06 |
| `views_daily_y9` | float | Średnia dzienna liczba odsłon, 9. rok wstecz. | 2017-07..2018-06 |
| `months_y9` | int | Liczba pełnych miesięcy, 9. rok wstecz. | 2017-07..2018-06 |
| `views_daily_y10` | float | Średnia dzienna liczba odsłon, 10. rok wstecz. | 2016-07..2017-06 |
| `months_y10` | int | Liczba pełnych miesięcy, 10. rok wstecz. | 2016-07..2017-06 |
| `views_daily_y11` | float | Średnia dzienna liczba odsłon, 11. rok wstecz. | 2015-07..2016-06 |
| `months_y11` | int | Liczba pełnych miesięcy, 11. rok wstecz. | 2015-07..2016-06 |
| `views_daily_full` | float | Średnia dzienna liczba odsłon z całego załadowanego okna. | 2015-07..2026-06 |
| `months_full` | int | Liczba pełnych miesięcy użytych w całym oknie. | 2015-07..2026-06 |
| `peak_to_median` | float | Wskaźnik skokowości: najlepszy miesiąc / mediana miesięcy. | 2015-07..2026-06 |

---

## `biographies_1st_year_and_up.tsv`
Surowy wynik zapytania SPARQL (nagłówki z prefiksem `?`, URI w `<...>`, stringi w cudzysłowach — konwencja TSV z WDQS). Źródło dla `biographies_enriched.tsv`.

| kolumna | typ | opis |
|---|---|---|
| `?item` | string | URI elementu Wikidata (np. `<http://www.wikidata.org/entity/Q42>`). |
| `?article` | string | Pełny URL artykułu w plwiki (sitelink), w `<...>`. |
| `?name` | string | Etykieta osoby (rdfs:label); w wersji wzbogaconej zastąpiona realnym `title`. |
| `?genderLabel` | string | Etykieta płci (P21). |
| `?dateOfBirth` | date | Data urodzenia (P569), `YYYY-MM-DD`. |
| `?year` `?month` `?day` | int | Składowe daty; `month`/`day` puste przy niższej precyzji. |
| `?decade` `?century` | float | Dekada / wiek urodzenia (definicje jak w `biographies_enriched.tsv`). |

---



## Glosariusz metryk

**Unique devices (unikalne urządzenia).** Szacunkowa liczba odrębnych urządzeń odwiedzających dany projekt w danym okresie — używana jako przybliżenie liczby czytelników, ponieważ do korzystania z Wikipedii nie trzeba się logować, więc uniques liczy się na podstawie cookies. Metoda „Last-Access": w żądaniach ustawiany jest cookie przechowujący jedynie datę ostatniej wizyty (rok-miesiąc-dzień), odświeżany przy każdym żądaniu i wygasający po ok. 30 dniach; do uniques danego miesiąca zaliczane jest urządzenie, które nie ma jeszcze w tym miesiącu zapisanej wizyty (lub nie ma cookie w ogóle). Liczenie jest z założenia prywatne — nie identyfikuje, nie „odciska palca" (fingerprint) ani nie śledzi użytkowników.

**To nie to samo co odsłony (`pageviews`):** jedno urządzenie generuje wiele odsłon, a ta sama osoba na telefonie i na laptopie liczy się jako dwa urządzenia — stąd wartości są o rząd wielkości niższe niż odsłony. Ograniczenia: ruch botów/automatów często nie przyjmuje cookie, co podbija pulę żądań „bez cookie"; dla małych projektów metryka bywa mocno zmienna. Miesięczne dane per domena dostępne od stycznia 2016 (stąd zakres pliku od 2016). Dotyczy pliku `oglądalność_Wikipedii_2016_2026_monthly_desktop_mobile_devices.csv`.

Źródła: `meta.wikimedia.org/wiki/Research:Unique_devices`, `wikitech.wikimedia.org/wiki/Analytics/Data_Lake/Traffic/Unique_Devices`, wpis WMF „Introducing the unique devices dataset" (2016).

**Pageviews (odsłony).** Zliczenie żądań traktowanych jako wyświetlenie treści strony. Żądanie jest odsłoną, gdy m.in.: kod HTTP to 200 lub 304; host to domena wiki (np. `pl.wikipedia.org`, ale nie `upload.`/`www.`); żądanie nie jest podglądem (`preview`) ani automatycznie wywołaną stroną `Special:`; a URL prowadzi do treści (`/wiki/...`, `/w/index.php?title=...`) lub ma znacznik `pageview=1` (np. treść z aplikacji mobilnej). Domyślnie (agent `user`) odsłony wykluczają rozpoznane boty/spidery. Definicja obowiązuje od maja 2015. **To nie liczba czytelników** — jedna osoba/urządzenie generuje wiele odsłon (por. unique devices). Źródło: `meta.wikimedia.org/wiki/Research:Page_view`.

**Typy agenta: `user` / `spider` / `automated`.** Podział odsłon wg tego, kto je generuje. `spider` — żądania, w których bot sam deklaruje się w nagłówku User-Agent (np. słowem „bot"). `automated` — ruch automatyczny rozpoznany heurystycznie (wolumen, wzorce aktywności), mimo że nie deklaruje się jako bot; kategoria wydzielona ok. 2020 (wcześniej wpadał do `user`/`spider`). `user` — reszta, traktowana jako ruch ludzki. Źródła: `wikitech.wikimedia.org/wiki/Analytics/Data_Lake/Traffic/BotDetection`, dokumentacja AQS „Page views".

**Metoda dostępu: `desktop` / `mobile-web` / `mobile-app`.** Sposób, w jaki żądano strony: wersja desktopowa, mobilna wersja webowa, albo aplikacja mobilna. Odpowiada kolumnom `total.desktop` / `total.mobile-web` / `total.mobile-app`. Źródło: `wikitech.wikimedia.org/wiki/Data_Platform/Data_Lake/Traffic/Pageview_hourly`.

**Content vs non-content (przestrzenie nazw).** „Content" to przestrzenie nazw skonfigurowane jako treściowe (`$wgContentNamespaces`); w plwiki jest to przestrzeń główna (ns0, artykuły). „Non-content" to pozostałe przestrzenie (dyskusje, `Wikipedia:`, `Szablon:`, `Kategoria:` itd.). Podział dotyczy kolumn `total.content` / `total.non-content` w plikach edycji i nowych stron.

**Typ edytora: `anonymous` / `user` / `group-bot` / `name-bot`** (klasyfikacja Wikistats). `anonymous` — edycje bez zalogowania (z adresu IP). `user` — konta zarejestrowane, nie-boty. `group-bot` — konta należące (w chwili edycji) do grupy z flagą „bot". `name-bot` — konta spoza grupy bot, ale rozpoznane jako bot heurystyką nazwy (np. „bot" w nazwie). Rozdzielenie na dwa typy botów bierze się stąd, że flaga bota to członkostwo w grupie, które zmienia się w czasie, więc część edycji botopodobnych nie ma formalnej flagi.

**Aktywni edytorzy (≥ 5 edycji/mies.).** W Wikistats „aktywny edytor" danego miesiąca to konto z co najmniej 5 edycjami w tym miesiącu (osobno bywa „bardzo aktywny" ≥ 100). Plik `active_editors_(5_and_over_edits)_...` używa progu 5.

