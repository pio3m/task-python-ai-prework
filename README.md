> ⭐ ***README** to coś więcej niż opis. Poprzez nie **pokazujesz swoje mocne strony** – swoją dokładność, sposób myślenia i podejście do rozwiązywania problemów. Niech Twoje README pokaże, że masz **świetne predyspozycje do rozwoju!***
> 
> 🎁 *Zacznij od razu. Skorzystaj z **[szablonu README i wskazówek](https://github.com/devmentor-pl/readme-template)**.* 

&nbsp;


# Stwórz Document Intake CLI: narzędzie do wczytywania i weryfikacji dokumentów firmy

Zbudujesz małe narzędzie wiersza poleceń, które wczytuje dokumenty firmy (faktury i umowy) z plików, sprawdza ich poprawność, „przetwarza” je przez symulowane zewnętrzne API i zapisuje raport. To fundament, na którym od Modułu 1 będziesz budować backend Enterprise Document Assistant.

**Zakres projektu:**

- projekt Pythona zarządzany przez `uv`, z konfiguracją w pliku `.env`,
- model danych oparty na klasach z adnotacjami typów,
- wczytywanie danych z plików JSON i CSV z obsługą błędów,
- asynchroniczne przetwarzanie z ponawianiem prób,
- testy automatyczne w pytest.


**Co powinno znaleźć się w Twoim projekcie?**

1. Konfiguracja wczytywana z pliku `.env` (przez `python-dotenv`):
   - `INPUT_DIR` – katalog z dokumentami do wczytania (`str`),
   - `OUTPUT_FILE` – ścieżka pliku z raportem (`str`),
   - `AMOUNT_THRESHOLD` – kwota, powyżej której faktura wymaga akceptacji (`float`),
   - `MAX_RETRIES` – liczba prób wywołania API (`int`).

2. Klasy dokumentów:
   - `Document` – klasa abstrakcyjna z polami `doc_id` (`str`), `title` (`str`), `content` (`str`), `tags` (`list[str]`) i abstrakcyjną metodą `summary() -> str`,
   - `Invoice` – dodatkowo `amount` (`float`), `due_date` (`date`), `paid` (`bool`),
   - `Contract` – dodatkowo `party` (`str`), `start_date` (`date`), `end_date` (`date | None`).

3. Własne wyjątki:
   - `DocumentValidationError` – wspólna klasa bazowa błędów walidacji,
   - co najmniej dwa wyjątki szczegółowe, np. `EmptyContentError` i `InvalidAmountError`.

4. Moduł wczytujący dane:
   - funkcja wczytująca wszystkie pliki `*.json` i `*.csv` z `INPUT_DIR`,
   - błędny plik lub rekord nie przerywa działania, tylko trafia do listy błędów z nazwą pliku i powodem.

5. Moduł przetwarzający:
   - funkcja `async process(document: Document) -> dict` symulująca wywołanie zewnętrznego API (`asyncio.sleep`, losowe albo zaplanowane `ConnectionError`),
   - dekorator ponawiający wywołanie do `MAX_RETRIES` razy,
   - przetwarzanie wszystkich dokumentów równolegle przez `asyncio.gather`.

6. Raport w pliku JSON (`OUTPUT_FILE`) z co najmniej: liczbą wczytanych dokumentów, listą błędów walidacji, listą faktur powyżej `AMOUNT_THRESHOLD`, listą nieudanych przetworzeń i czasem działania.


**Dodatkowe wymagania:**

- kod podzielony na moduły w pakiecie (np. `intake/models.py`, `intake/loader.py`, `intake/processor.py`), uruchamiany komendą `uv run python -m intake`,
- adnotacje typów we wszystkich funkcjach i metodach, ścieżki przez `pathlib`, pliki otwierane przez `with` z `encoding="utf-8"`,
- plik `.env` w `.gitignore`, a w repozytorium plik `.env.example` z przykładowymi wartościami bez sekretów,
- co najmniej 8 testów pytest, w tym: `pytest.raises` dla wyjątków walidacji, fixture z przykładowymi dokumentami, `parametrize` oraz test wczytywania na plikach tymczasowych (`tmp_path`); testy nie mogą zależeć od losowości ani od dzisiejszej daty,
- **zamiast `print` użyj modułu `logging`: komunikaty o postępie na poziomie `INFO`, o pominiętych plikach i nieudanych próbach na poziomie `WARNING`, z poziomem ustawianym w `.env` (`LOG_LEVEL`).**

:bulb: **Podpowiedź:**  
Doczytaj, jak skonfigurować moduł `logging` (`basicConfig`, poziomy, `getLogger(__name__)`) i dlaczego w aplikacjach używa się go zamiast `print`.
Zajrzyj tutaj:  
[Logging HOWTO (dokumentacja Pythona)](https://docs.python.org/3/howto/logging.html)


**Co powinieneś zrobić?**

- przygotuj w katalogu `sample_data/` co najmniej 6 dokumentów: faktury i umowy w JSON oraz jeden plik CSV z kilkoma fakturami,
- celowo dodaj dane z błędami (uszkodzony JSON, pusta treść, ujemna kwota), żeby było widać działanie walidacji i raportu błędów.


**Funkcjonalności, które powinieneś napisać:**

- wczytanie konfiguracji z `.env` z czytelnym błędem, gdy brakuje wymaganej zmiennej,
- wczytanie i walidacja wszystkich dokumentów z `INPUT_DIR`,
- `summary()` dla każdego dokumentu w logach na poziomie `INFO`,
- wskazanie faktur powyżej `AMOUNT_THRESHOLD` oraz faktur nieopłaconych po terminie względem daty przekazanej jako argument funkcji (nie liczonej wewnątrz z `date.today()`),
- asynchroniczne przetworzenie poprawnych dokumentów z ponawianiem prób,
- zapis raportu JSON.


**Twoje rozwiązanie powinno składać się z:**

- `pyproject.toml` i `uv.lock` (zależności: `python-dotenv`, `pytest`),
- pakietu `intake/` z modułami modelu, wczytywania, przetwarzania i punktem wejścia `__main__.py`,
- katalogu `tests/` z testami pytest,
- katalogu `sample_data/` z przykładowymi dokumentami,
- plików `.env.example` i `.gitignore`,
- `README.md` z opisem projektu, instrukcją instalacji (`uv sync`), uruchomienia i uruchamiania testów (`uv run pytest`),
- opcjonalnie krótkiego opisu przyjętej struktury / architektury.

&nbsp;

> ⭐ ***README** to coś więcej niż opis. Poprzez nie **pokazujesz swoje mocne strony** – swoją dokładność, sposób myślenia i podejście do rozwiązywania problemów. Niech Twoje README pokaże, że masz **świetne predyspozycje do rozwoju!***
> 
> 🎁 *Zacznij od razu. Skorzystaj z **[szablonu README i wskazówek](https://github.com/devmentor-pl/readme-template)**.* 
