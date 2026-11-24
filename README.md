> ⭐ ***README** to coś więcej niż opis. Poprzez nie **pokazujesz swoje mocne strony** – swoją dokładność, sposób myślenia i podejście do rozwiązywania problemów. Niech Twoje README pokaże, że masz **świetne predyspozycje do rozwoju!***
> 
> 🎁 *Zacznij od razu. Skorzystaj z **[szablonu README i wskazówek](https://github.com/devmentor-pl/readme-template)**.* 

&nbsp;


# Stwórz program do przyjmowania dokumentów firmy: wczytywanie, weryfikacja i raport

Zbudujesz mały, **synchroniczny** program uruchamiany z terminala, który wczytuje dokumenty firmy (faktury i umowy) z plików JSON, sprawdza ich poprawność i zapisuje raport. Projekt łączy umiejętności z zadań `#01`–`#05` z `practice-python-ai-prework` i nie wymaga żadnych nowych mechanizmów. To fundament, na którym od Modułu 1 będziesz budować backend Enterprise Document Assistant.

**Zakres projektu:**

- projekt Pythona zarządzany przez `uv`, z konfiguracją w pliku `.env`,
- jedna prosta klasa dokumentu z adnotacjami typów,
- wczytywanie plików JSON z obsługą błędów i walidacją,
- raport JSON,
- testy automatyczne w pytest.


**Co powinno znaleźć się w Twoim projekcie?**

1. Konfiguracja wczytywana z pliku `.env` (przez `python-dotenv`):
   - `DOCS_DIR` – katalog z dokumentami do wczytania (`str`),
   - `OUTPUT_FILE` – ścieżka pliku z raportem (`str`),
   - `AMOUNT_THRESHOLD` – kwota, powyżej której faktura wymaga akceptacji (`float`).

   Brak wymaganej zmiennej kończy się czytelnym komunikatem. Zmienne ustawione już w systemie mają pierwszeństwo przed `.env`.

2. Klasa `Document` (zwykła klasa, bez dziedziczenia):
   - pola `doc_id` (`str`), `kind` (`str`), `title` (`str`), `content` (`str`), `amount` (`float | None`, dla umowy może być `None`),
   - metoda `summary() -> str` zwracająca krótki opis dokumentu.

3. Import i walidacja:
   - wczytanie wszystkich plików `*.json` z `DOCS_DIR` w kolejności posortowanej po nazwie; jeden dokument w jednym pliku,
   - wymagane niepuste teksty (`doc_id`, `title`, `content`), `kind` równe `invoice` albo `contract`, dla faktury skończona, nieujemna kwota,
   - błędny plik lub rekord (np. uszkodzony JSON, pusta treść) nie przerywa działania: trafia do listy błędów z nazwą pliku i przyczyną, a pozostałe pliki są wczytywane,
   - brak katalogu kończy się czytelnym komunikatem; pusty katalog daje pusty raport.

4. Raport JSON w `OUTPUT_FILE` z kluczami:
   - `loaded` – liczba poprawnie wczytanych dokumentów,
   - `errors` – lista obiektów `{"file": ..., "reason": ...}`,
   - `requires_approval` – lista `doc_id` faktur z kwotą **większą niż** `AMOUNT_THRESHOLD` (kwota równa progowi nie wymaga akceptacji).

   Dla pustego katalogu raport to `{"loaded": 0, "errors": [], "requires_approval": []}`. Do komunikatów w terminalu wystarczy `print`.


**Dodatkowe wymagania:**

- kod podzielony na kilka czytelnych modułów w katalogu `src/` i plik `main.py` jako punkt wejścia (`uv run python main.py`); konkretna liczba modułów nie jest wymagana,
- adnotacje typów w funkcjach i metodach, ścieżki przez `pathlib`, pliki otwierane przez `with` z `encoding="utf-8"`,
- plik `.env` w `.gitignore`, a w repozytorium plik `.env.example` z przykładowymi wartościami bez sekretów,
- testy działają offline i nie zależą od losowości ani od dzisiejszej daty.

Testy w katalogu `tests/` importują kod z `src/`. Sam `pytest` nie dodaje katalogu projektu do ścieżki importu, więc bez konfiguracji zobaczysz `ModuleNotFoundError`. Dopisz do `pyproject.toml`:

```toml
[tool.pytest.ini_options]
pythonpath = ["."]
```


**Co powinieneś zrobić?**

- przygotuj w katalogu `data/` co najmniej 4 pliki JSON: 2 poprawne dokumenty (fakturę i umowę) oraz 2 błędne (pusta treść i uszkodzony JSON),
- napisz co najmniej **4 własne testy** pytest w katalogu `tests/`:
  - poprawny dokument,
  - błąd walidacji (np. pusta treść),
  - pominięcie uszkodzonego pliku JSON (pliki tworzone w `tmp_path`),
  - raport z kwotą równą progowi i większą od progu.


**Twoje rozwiązanie powinno składać się z:**

- `pyproject.toml` i `uv.lock`; projekt załóż komendą `uv init --python 3.12`, a zależności dodaj przez `uv add python-dotenv` i `uv add --dev pytest`,
- katalogu `src/` z modułami oraz pliku `main.py`,
- katalogu `tests/` z testami pytest,
- katalogu `data/` z przykładowymi dokumentami,
- plików `.env.example` i `.gitignore`,
- `README.md` z opisem projektu, instrukcją instalacji (`uv sync --locked`), uruchomienia (`uv run python main.py`) i uruchamiania testów (`uv run python -m pytest`).


**Rozszerzenia (opcjonalne)**

Gdy wersja podstawowa działa i ma zielone testy, możesz ją rozbudować. Wersja podstawowa nie może zależeć od żadnego z rozszerzeń.

- obsługa plików CSV obok JSON i więcej danych (np. 6 dokumentów, 8 własnych testów z `parametrize` i fixture),
- klasa abstrakcyjna i klasy `Invoice`/`Contract` (`@dataclass`), pola `tags` i daty,
- faktury nieopłacone po terminie względem daty przekazanej jako argument (nie `date.today()`),
- wspólna klasa `DocumentValidationError` i wyjątki szczegółowe,
- moduł `logging` zamiast `print` (postęp na `INFO`, pominięte pliki na `WARNING`, poziom w `LOG_LEVEL`),
- asynchroniczne przetwarzanie dokumentów przez atrapę API z `asyncio.gather`,
- ponawianie prób pętlą albo dekoratorem; `MAX_RETRIES` to łączna, dodatnia liczba prób,
- w raporcie: nieudane przetworzenia i czas działania.

&nbsp;

> ⭐ ***README** to coś więcej niż opis. Poprzez nie **pokazujesz swoje mocne strony** – swoją dokładność, sposób myślenia i podejście do rozwiązywania problemów. Niech Twoje README pokaże, że masz **świetne predyspozycje do rozwoju!***
> 
> 🎁 *Zacznij od razu. Skorzystaj z **[szablonu README i wskazówek](https://github.com/devmentor-pl/readme-template)**.* 
